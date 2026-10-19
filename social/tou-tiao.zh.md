---
title: "写一次 Markdown，同时发 11 个平台：开源工具 发匣 FaXia 上手指南"
platform: tou-tiao
language: zh-CN
date: 2026-07-10
description: "零基础教程：用 发匣 FaXia 实现 Markdown 文章一键分发到微信公众号、知乎、掘金、头条等 11 个内容平台，附带详细配置步骤。"
keywords: ["发匣 FaXia", "多平台发布", "Markdown", "教程", "效率工具"]
tags: ["教程", "效率工具", "内容创作"]
category: Tech
word_count: 1500
author: "kylinlab.tech"
toutiao_category: "科技 > 编程"
toutiao_original: true
toutiao_tags: ["发匣 FaXia", "Markdown", "多平台发布", "效率工具"]
visibility: "公开"
---

# 写一次 Markdown，同时发 11 个平台：开源工具 发匣 FaXia 上手

你是不是也有这种经历：写完一篇文章后，打开知乎贴一遍、打开掘金贴一遍、打开公众号贴一遍……每个平台都要手动调格式、传图片、改链接，发完一圈一小时没了。

今天给大家介绍一个我刚发现的效率工具——**发匣 FaXia**。开源免费，一条命令把文章同步到 11 个平台，图片自动上传、格式自动适配。关键是纯本地运行，你的账号密码和文章内容不会上传到任何服务器。

## 支持哪些平台？

国内 5 个 + 国外 6 个，总共 11 个：

- **国内**：微信公众号、知乎、稀土掘金、头条号、微博
- **国外**：GitHub Pages、Medium、Reddit、X(Twitter)、Facebook、LinkedIn

你可以全选一起发，也可以只选其中几个。每个平台独立运行，一个出问题不影响其他。

## 怎么装？

**Windows/Mac/Linux 都有桌面版**，去 GitHub Release 页面下载安装包就行。

如果你习惯命令行，一行搞定：

```bash
npm install -g blog-assistant
```

## 第一步：配置凭证

装好后，先配好要发布平台的账号信息。

**桌面端**：打开设置 → Credentials 标签页，为每个平台填写凭证。有的平台填 API Token，有的需要浏览器 Cookie（按页面提示操作就行）。

填完后点 Test Connection 测试一下，看到绿色对勾就说明配置成功了。

**命令行**：编辑 `~/.blogassistant/config.json` 文件，然后运行 `tdp check` 检查。

> 小提示：知乎推荐启用 Playwright 模式，更稳定。需要先额外装一下：`npx playwright install chromium`

## 第二步：写文章

按你平时的习惯写 Markdown 就行。建议在文章最前面加一个 YAML 头部（可选）：

```yaml
---
title: "文章标题"
tags: ["技术", "教程"]
cover_image: "./cover.png"
summary: "一句话摘要"
---
```

配图用本地路径就好，工具会自动上传到对应平台。

## 第三步：一键发布

命令行：

```bash
# 发到所有已配置平台
tdp publish 我的文章.md

# 只发到知乎和掘金
tdp publish 我的文章.md -p zhihu,juejin

# 先预演一下，确认没问题再真发
tdp publish 我的文章.md --dry-run
```

桌面端：打开应用 → 拖文件进去 → 勾选平台 → 点发布。

发完后每个平台会显示成功/失败状态，成功了会给你直达链接。

## 进阶玩法：不同平台发不同版本

中文平台和英文平台的读者口味不一样，你可以准备多个版本：

```
文章目录/
├── zhihu.zh.md          # 知乎用这个
├── wechat-official-account.zh.md  # 公众号用这个
├── medium.md            # Medium 用这个
└── juejin.zh.md         # 掘金用这个
```

然后直接发布整个目录：`tdp publish 文章目录/`。工具会自动匹配文件名到对应的平台。

## 踩坑提醒

1. **微信公众号的 Token 每两小时过期**——如果发布失败，去设置里刷新一下 Token
2. **头条和微博用的是 Cookie 登录**——Cookie 失效后要从浏览器重新导出
3. **部分平台网络可能不通**——如果在国内发 Medium 或 Reddit，需要开代理
4. **头条号不支持更新已发文章**——发之前一定用 dry-run 确认内容没问题

## 总结

发匣 FaXia 目前是 alpha 阶段（新功能还在加），但核心的 11 平台发布已经跑通。对需要多平台分发的内容创作者来说，能把每天"发文章"的时间从 1 小时压缩到 1 分钟。

纯本地运行、代码开源（MIT 协议）、不收集任何数据——你的账号和文章始终在自己手里。

去 GitHub 搜 **KylinLabAI/发匣 FaXia-App** 就能找到，觉得有用的话给个 Star 支持一下。

有问题欢迎在评论区交流，我会尽量回复。
