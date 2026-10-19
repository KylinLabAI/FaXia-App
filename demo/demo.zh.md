# 发匣 FaXia 演示文档

[English](demo.md)

## 产品概述

![发匣 Logo](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/logo_256.png)

**发匣 FaXia** 是一个多平台内容发布工具，支持内容创作者一键将 Markdown 文章发布到 11+ 个平台。本演示文档展示了关键功能，包括多平台发布、博客状态跟踪、评论监控和平台凭证管理。

![应用概览](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-publish.png)

- 统一界面管理所有内容发布需求
- 六个主要标签页：发布、已发布、评论、设置、账户、关于（另有反馈按钮）
- 支持 GitHub Pages、微信公众号、知乎、掘金、头条号、微博、Reddit、X/Twitter、Medium、Facebook、LinkedIn

## 核心价值

| 你得到什么 | 怎么做到的 |
|---|---|
| **每周多出几小时写作** | 11+ 平台并发发布，原来一小时的搬运工活，几秒钟干完 |
| **读者在哪里都能看到你** | 原生适配所有主流中英文平台 |
| **图片再也不挂了** | 自动上传图片并替换每个平台的 URL |
| **格式每次都对** | 智能 Markdown 转 HTML + 代码高亮，针对每个平台做过适配 |
| **发之前先确认** | dry-run 模式和凭证有效性检查 |
| **内容和凭证都在你手里** | 开源、纯本地工具，没有云端服务 |

## 适用角色

- **内容创作者**：作家、博主、技术作者，需要在多个平台发布内容
- **开发者布道师**：技术专业人士，需要同时面向中英文受众发布内容
- **营销团队**：需要在不同社交平台发布相同内容的团队

---

## 业务流程概述

### 流程图

```
内容创作 → 平台适配 → FaXia 发布 → 多平台分发 → 反馈收集 → 内容迭代
```

### 阶段表

| 阶段 | 描述 | 涉及工具 |
|-------|-------------|----------------|
| 内容创作 | 用 Markdown 编写核心内容 | 任意 Markdown 编辑器 |
| 平台适配 | 根据需要创建平台特定版本 | FaXia 目录模式 |
| 发布 | 发布到所有配置的平台 | FaXia CLI 或桌面应用 |
| 分发 | 内容在所有平台上线 | 平台 API / Playwright |
| 反馈收集 | 监控各平台评论 | FaXia 评论标签页 |
| 内容迭代 | 更新并重发布文章 | FaXia 更新功能 |

---

## 角色详情

### 内容创作者操作演示

#### 1. 发布功能

**发布模式与平台选择**

选择单文件或多平台目录模式，然后勾选目标平台：

![发布](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-publish.png)

- 在**单文件**和**目录（多平台）**模式之间切换
- 选择要发布的 Markdown 文件
- 从清单中选择目标平台（Pages、头条、微信、知乎、掘金、微博等）
- 自动上传图片并按平台替换 URL

**发布进度**

实时查看每个平台并发发布的状态：

![发布中](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-publishing.png)

- 每个平台独立处理，显示“Working…”状态
- 一个平台失败不影响其他平台
- 先用**演练（Dry Run）**预览，再点击**发布中**执行

**发布结果**

查看每个平台的发布结果和直达链接：

![已发布博客](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-published-blogs.png)

![已发布微博客](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-published-miroblogs.png)

- 查看哪些平台成功、哪些待处理、哪些失败
- 各平台已发布内容的直达链接
- 微信会显示**草稿（Draft）**状态及待发布原因
- 识别需要重新发布或人工跟进的文章

#### 2. 已发布历史与同步

浏览完整发布历史，并从各平台重新同步记录：

![已发布](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-settings-4.png)

- 按标题分组查看所有已发布文章及其各平台记录
- **从<平台>同步**把最新已发布列表拉回 FaXia
- **移除重复项**清理重复的历史记录
- 汇总显示已跟踪的博客数与平台记录数

#### 3. 博客评论

监控所有平台的评论：

![评论](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-comments.png)

- 按文章集中展示各已连接平台的评论
- 按平台筛选（含评论数）
- 跳转到原始文章或其评论区
- 在应用内管理回复

#### 4. 设置

**应用数据目录与日志级别**

![设置：数据目录与日志级别](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-settings-1.png)

- 配置会话、历史、截图和配置的存储位置
- 重置为平台默认位置
- 设置应用日志详细度（fatal → detail）；更改立即生效并在重启后保留

**博客根目录**

![设置：博客根目录](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-settings-2.png)

- 指向存放博客 Markdown 文件的根目录
- 用于定位待发布的文章文件

**平台凭证**

在一个统一面板中配置各平台凭证：

![设置：凭证](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-settings-3.png)

- 平台标签以实心/空心一眼区分已配置/未配置
- Pages（GitHub/GitLab/Gitee）：服务商、访问令牌、仓库、分支、发布/图片目录，带**保存**和**测试**
- Cookie / Playwright 平台（知乎、掘金、头条、微博）：登录一次，会话即被持久化用于无头发布

#### 5. 账户

管理 LianXia 集成与登录：

![账户](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-account.png)

- 查看集成、应用、设备与登录状态
- **检查更新**
- 使用 GitHub 或微信登录

---

## 技术架构

### 整体架构

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

### 关键亮点

1. **共享核心**：CLI 和桌面应用使用同一套 TypeScript 发布引擎
2. **并发发布**：使用 `Promise.allSettled` 并行执行，错误隔离
3. **多层平台支持**：有官方 API 的平台使用 API 方式，没有的使用 Playwright
4. **图片管道**：自动上传到每个平台的图床，并重写 URL
5. **本地优先**：所有凭证和数据存储在本地，永不发送到外部服务器

---

## 业务规模和约束

### 规模

- 支持无限文章和发布历史
- 并发发布受平台 API 速率限制
- 桌面应用已测试 11 个平台同时运行

### 约束

- 平台 API 速率限制可能影响发布速度
- 某些平台需要 Playwright 模式，需要安装 Chromium
- 微信 access_token 每 2 小时过期（工具自动刷新）
- 平台 UI 变更可能需要更新适配器

### 限制

- 某些平台不支持文章更新（只支持创建）
- 目前不支持视频上传
- 定时发布功能在路线图中

---

## 快速参考

| 功能 | 描述 | 图片 |
|---------|-------------|-------|
| 发布与模式 | 单文件或目录，选择目标平台 | faxia-publish.png |
| 发布进度 | 各平台并发发布 + 演练 | faxia-publishing.png |
| 发布结果 | 各平台成功/草稿/失败与链接 | faxia-published-blogs.png |
| 发布结果（微博客） | 微博客发布结果 | faxia-published-miroblogs.png |
| 已发布历史与同步 | 历史列表 + 各平台同步 | faxia-settings-4.png |
| 博客评论 | 监控所有平台评论 | faxia-comments.png |
| 数据目录与日志 | 存储位置 + 日志详细度 | faxia-settings-1.png |
| 博客根目录 | 博客文件位置 | faxia-settings-2.png |
| 平台凭证 | 统一的平台凭证面板 | faxia-settings-3.png |
| 账户 | LianXia 集成与登录 | faxia-account.png |

---

## 快速开始

1. **下载** — 从 [Gitee Releases](https://gitee.com/KylinLab/FaXia-App/releases/latest) 获取安装包
2. **安装** — 运行安装程序并按照提示操作
3. **启动** — 从应用程序文件夹打开发匣
4. **配置凭证** — 进入设置标签页，配置各平台的凭证
5. **准备内容** — 用 Markdown 格式编写文章
6. **发布** — 选择文件并选择目标平台，点击发布
7. **监控** — 在已发布标签页查看状态，在评论标签页查看反馈

---

**一次创作，多平台发布。**

[下载发匣](https://gitee.com/KylinLab/FaXia-App/releases/latest)