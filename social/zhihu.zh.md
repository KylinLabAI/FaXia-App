---
title: "为什么我写了一个能同时发11个平台的开源工具——发匣 FaXia 技术实现与思考"
platform: zhihu
language: zh-CN
date: 2026-07-10
description: "深入介绍 发匣 FaXia 的设计理念和技术架构：如何用一套引擎同时支持 API 和 Playwright 两种发布模式，覆盖 11 个内容平台。"
keywords: ["发匣 FaXia", "多平台发布", "Playwright", "Electron", "Markdown", "开源工具"]
tags: ["独立开发", "效率工具", "开源", "Electron", "Playwright"]
category: Tech
word_count: 2200
author: "kylinlab.tech"
---

# 我写了一个能同时把文章发到 11 个平台的开源工具——再也不用当搬运工了

**结论先行：发匣 FaXia 让你写一份 Markdown，并发发布到 GitHub Pages、微信公众号、知乎、掘金、头条、微博、Reddit、Twitter、Medium、Facebook、LinkedIn，图片自动上传、格式自动适配。纯本地、开源、免费。**

## 为什么做这个工具

如果你写过技术博客，你一定经历过这个流程：

写完文章 → 打开知乎 → 粘贴 → 调格式 → 传图片 → 发布 → 打开掘金 → 粘贴 → 调格式 → 传图片 → 发布 → 打开 Medium → 粘贴 → 调格式 → ……

11 个平台，每个都要重复一遍。一篇文章发完，一小时过去了。更要命的是，你的中文版本和英文版本是分开维护的，改了一个忘了另一个，内容永远不同步。

市面上有没有能"一稿多发"的工具？有，但要么是 SaaS（你的内容和凭证要上传到别人服务器），要么只支持少数平台，要么收费还很贵。

**结论：没有我想要的东西，那就自己做一个。**

## 架构设计

发匣 FaXia 的核心架构分三层：

**第一层：发布引擎核心（shared core）**
- Markdown 解析器：处理 frontmatter、提取元数据
- 图片处理管道：检测本地图片 → 上传到各平台图床 → 回写 URL
- 格式转换器：Markdown → 各平台支持的格式（HTML、富文本等），针对每个平台的编辑器特性做适配

**第二层：平台适配器（11 个独立模块）**
- **API 型**（GitHub Pages、Medium、Reddit、X/Twitter、Facebook、LinkedIn）：直接用各平台的 REST API 发布
- **API + Headless 混合型**（微信公众号、知乎、掘金）：以 API 为主，部分场景用 Playwright 兜底
- **纯 Headless 型**（头条号、微博）：这两个平台没有公开的写入 API，用 Playwright 模拟真实浏览器操作

每个适配器是独立模块，一个失败完全不影响其他平台——这是并发发布稳定性的关键保障。

**第三层：用户界面**
- CLI（`faxia`）：Node.js 命令行，支持脚本化和 CI/CD 集成
- 桌面端（Electron）：平台选择器、凭证管理器、实时预览、发布历史

两套界面共享第一层和第二层代码，功能完全一致。

## 几个有意思的技术细节

### 1. 图片上传的链路

你在 Markdown 里写 `![架构图](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/social/images/architecture.png)`，发匣 FaXia 的处理链路是：

```
解析 Markdown → 检测本地图片引用 → 
对每个平台并行上传 → 返回平台图床 URL → 
生成每个平台的独立 HTML/Markdown（图片链接已替换）
```

这意味着同一张图片，在知乎文章里指向知乎图床，在 Medium 里指向 Medium 图床，在 GitHub Pages 里是你自己的仓库。每个平台的读者都能正常加载——不会出现"知乎文章引用了本地路径"的尴尬。

### 2. Playwright 模式的可靠性设计

头条号和微博没有公开写入 API，只能模拟浏览器操作。这里面坑很多：

- **选择器脆弱**：平台前端一更新，选择器就失效。我们用了多层降级策略：优先语义选择器（`role`/`text`），再降级到 CSS class，最后用 XPath
- **登录态维护**：Cookie 过期后会自动检测并提示用户更新，不会默默发布失败
- **每一步都有超时重试**：网络抖动导致的单个操作失败不会让整个发布流程崩溃

### 3. 并发发布 + 错误隔离

11 个平台并发执行，用的是 Node.js 的 `Promise.allSettled()`——不管哪个平台成功或失败，都不影响其他平台。发布完成后返回汇总结果：每个平台的状态 + 成功时的直达链接 + 失败时的具体错误信息。

## 踩过的坑

1. **微信公众号的素材管理接口和发布接口是两套系统**——先上传图片获取 media_id，再图文素材里引用，最后才能发布。而且 Token 每 2 小时过期，中间要无缝刷新
2. **知乎的富文本编辑器在不同状态下行为不同**——新文章和编辑已有文章的元素结构不一样，Playwright 脚本要做两套逻辑
3. **Medium 的 API 需要先拿到 authorId 和 publicationId**——这两个值不能直接从用户名推导，需要额外的查找请求
4. **不同平台的 Markdown 方言不同**——有的不支持脚注、有的表格语法不一样、有的代码块需要特定语言标识符——转换器要逐一适配

## 总结

发匣 FaXia 目前处于 alpha 阶段，11 个平台的核心发布都已跑通，桌面端可用但细节还在打磨。

如果你也是需要多平台分发的内容创作者，试试看：[GitHub - KylinLabAI/发匣 FaXia-App](https://github.com/KylinLabAI/发匣 FaXia-App)。纯本地运行，代码完全开源（MIT），不收集任何数据。

有什么需求或建议，欢迎提 Issue。如果觉得有用，也欢迎 Star 支持一下。
