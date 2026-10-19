---
title: "从零构建多平台发布工具：Electron + Playwright + 11个平台适配器的架构实战"
platform: juejin
language: zh-CN
date: 2026-07-10
description: "深入 发匣 FaXia 的技术架构：如何设计一个支持 11 个平台的并发发布引擎，包括 API 适配层、Playwright Headless 模式、图片处理管道和 CLI/Electron 双界面设计。"
keywords: ["发匣 FaXia", "Electron", "Playwright", "多平台发布", "架构设计", "开源"]
tags: ["Electron", "Playwright", "架构", "开源", "Node.js"]
category: Tech
word_count: 2500
author: "kylinlab.tech"
juejin_category: "前端"
juejin_tags: ["Electron", "Playwright", "Node.js", "架构设计", "开源"]
---

# 从零构建多平台发布工具：Electron + Playwright + 11 个平台适配器的架构实战

## 背景

写技术博客的人都有一个共同的痛点：写完文章要手动分发到各个平台。知乎、掘金、微信公众号、Medium、GitHub Pages……每个平台的操作流程不一样，一篇文章发完 11 个平台至少一小时。

市面上有"一稿多发"的工具，但要么是云端服务（你要把账号密码交给别人），要么支持的平台太少，要么收费。

于是我决定自己造一个——**发匣 FaXia**，开源、本地运行、支持 11 个平台。本文将深入介绍它的技术架构和核心实现。

## 技术选型

| 层面 | 技术 | 选型理由 |
|------|------|----------|
| 桌面端框架 | Electron | 跨平台（Win/Mac/Linux），成熟的生态 |
| CLI 框架 | Node.js + Commander.js | 脚本化友好，和桌面端共享发布核心 |
| 浏览器自动化 | Playwright | 比 Puppeteer 更稳定，多浏览器支持 |
| 发布引擎语言 | TypeScript | 类型安全，11 个适配器的接口约束很重要 |
| Markdown 处理 | remark + rehype | 插件化 AST 操作，方便扩展 |
| 打包 | electron-builder | 支持三平台打包和自动更新 |

## 架构设计

### 三层架构

```
┌─────────────────────────────────────┐
│         用户界面层                    │
│  ┌─────────────┐  ┌───────────────┐ │
│  │ Electron GUI │  │   faxia CLI   │ │
│  └──────┬───────┘  └──────┬────────┘ │
├─────────┼──────────────────┼──────────┤
│         ▼                  ▼          │
│  ┌─────────────────────────────────┐ │
│  │       发布引擎核心               │ │
│  │  • Markdown 解析 & 格式转换     │ │
│  │  • 图片上传管道                  │ │
│  │  • 发布历史管理                  │ │
│  │  • 并发调度器                    │ │
│  └──────────────┬──────────────────┘ │
├─────────────────┼────────────────────┤
│                 ▼                    │
│  ┌─────────────────────────────────┐ │
│  │      平台适配器层 (11个)         │ │
│  │  API型  │  混合型  │  Headless型│ │
│  └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

关键设计原则：**平台适配器是纯函数式的独立模块**，每个适配器暴露统一的 `IPublisher` 接口：

```typescript
interface IPublisher {
  readonly platform: Platform;
  publish(article: Article): Promise<PublishResult>;
  update(article: Article, existingId: string): Promise<PublishResult>;
  validateCredentials(): Promise<boolean>;
  uploadImage(image: Buffer, filename: string): Promise<string>;
}
```

这个接口让并发调度器可以统一处理所有平台，不需要知道每个平台的内部实现细节。

### 并发发布与错误隔离

核心调度逻辑：

```typescript
async function publishAll(
  article: Article,
  platforms: IPublisher[]
): Promise<Map<Platform, PublishResult>> {
  const results = await Promise.allSettled(
    platforms.map(p => p.publish(article))
  );

  const resultMap = new Map();
  results.forEach((r, i) => {
    const platform = platforms[i].platform;
    if (r.status === 'fulfilled') {
      resultMap.set(platform, r.value);
    } else {
      resultMap.set(platform, {
        success: false,
        error: r.reason.message,
        platform
      });
    }
  });
  return resultMap;
}
```

用 `Promise.allSettled` 而非 `Promise.all`——任何一个平台失败不能阻塞其他平台的发布。这在模拟浏览器操作的平台上尤其重要，因为这些平台更容易受网络和页面结构变化影响。

### 图片处理管道

这是最容易被低估的复杂模块。处理链路：

```
Markdown 文件 → remark 解析 AST → 遍历 image 节点 →
  检测是否为本地路径 → 对每个平台并行上传 →
  生成平台独立的输出（图片 URL 已替换为对应图床地址）
```

关键设计点：

1. **只上传一次**：同一张图在多个平台引用？先在本地缓存处理，然后并发上传到各平台图床
2. **失败降级**：如果某个平台的图床上传失败，记录错误但不阻塞其他平台——那个平台的输出文件中保留原始路径，并在结果报告中标记
3. **格式转换**：上传前自动转换 WebP/AVIF 等不被部分平台支持的格式

### Playwright 适配器的稳定性策略

头条号和微博没有公开写入 API，只能用浏览器自动化。浏览器自动化的核心挑战是**脆弱性**——平台前端一更新，选择器就失效。

我们的策略：

```typescript
// 多层降级选择器策略
async function safeClick(page: Page, selectors: Selector[]): Promise<void> {
  for (const sel of selectors) {
    try {
      const el = await page.waitForSelector(sel.value, { timeout: 5000 });
      if (el) {
        await el.click();
        return;
      }
    } catch {
      continue; // 尝试下一个选择器
    }
  }
  throw new Error(`所有选择器均失败: ${selectors.map(s => s.value).join(', ')}`);
}

// 优先级：role > text > testid > css class > xpath
const submitSelectors: Selector[] = [
  { type: 'role', value: 'button[name="发布"]' },
  { type: 'text', value: '发布' },
  { type: 'css', value: '.publish-btn' },
  { type: 'css', value: '.submit-button' },
  { type: 'xpath', value: '//button[contains(text(), "发布")]' },
];
```

每个操作都有超时 + 重试机制（默认 3 次），单次失败不会导致整个发布流程崩溃。

## 跨平台文件分发模式

我们支持两种使用模式：

**单文件模式**：一个 `.md` 文件发到所有平台，适合中英文差别不大的场景。

**目录模式**：一个目录下放多个平台文件，按文件名匹配：

```
my-article/
├── zhihu.zh.md               → 知乎
├── wechat-official-account.zh.md → 公众号
├── juejin.zh.md              → 掘金
├── medium.md                 → Medium
└── default.md                → 其余平台降级使用
```

文件名到平台的匹配规则用简单的映射表，没有匹配到就用 `default.md` 或目录中第一个 `.md` 文件。

## 踩坑记录

1. **微信公众号素材接口的 Token 刷新坑**：access_token 有效期 2 小时，在批量上传图片 + 发布的过程中可能刚好过期。解决方案是在每次 API 调用前检查 token 过期时间，提前刷新
2. **知乎编辑器的 Shadow DOM**：知乎新版编辑器用了 Web Components，普通选择器穿透不了 Shadow DOM。需要 Playwright 的 `>>` 穿透语法
3. **Medium 的 Content License 字段**：API 文档没有明确说明，实际上 `contentLicense` 是必填字段，不传会静默失败
4. **Electron 打包体积**：Playwright 的 Chromium 二进制有 ~150MB，不适合内嵌。最终方案是在首次运行时引导用户安装，或者 CLI 用户自行 `npx playwright install chromium`

## 总结

发匣 FaXia 目前 alpha 阶段，11 个平台的核心发布已跑通。整个项目约 8000 行 TypeScript，其中适配器代码占了一半（每个平台平均 ~350 行，头条和微博因为有 Playwright 逻辑稍多）。

对想做类似工具的同学，几点建议：
- 适配器接口一定要统一，否则并发调度层没法写
- Playwright 选择器一定要做多层降级，不然平台一更新就挂
- 不要低估图片处理的复杂度——上传、格式转换、错误恢复，每个环节都要考虑

GitHub：[KylinLabAI/发匣 FaXia-App](https://github.com/KylinLabAI/发匣 FaXia-App)，欢迎 Star、Issue 和 PR。

后续计划：定时发布、数据看板、插件系统。敬请关注。
