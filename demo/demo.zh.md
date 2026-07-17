# 发匣 FaXia 演示文档

[English](demo.md)

## 产品概述

![发匣 Logo](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/logo_256.png)

**发匣 FaXia** 是一个多平台内容发布工具，支持内容创作者一键将 Markdown 文章发布到 11+ 个平台。本演示文档展示了关键功能，包括多平台发布、博客状态跟踪、评论监控和平台凭证管理。

![应用概览](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/App_overview.png)

- 统一界面管理所有内容发布需求
- 四个主要标签页：发布、已发布、评论、设置
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

**发布模式选择**

选择发布方式：

![发布模式](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/publish_mode.png)

- 选择单平台发布或多平台同时发布
- 选择要发布的 Markdown 文件
- 发布前配置选项

**单平台发布**

发布到特定平台：

![单平台发布](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/publish_mode_one_platform.png)

- 从可用选项中选择单个目标平台
- 发布前预览内容
- 支持演练模式验证
- 自动图片上传和 URL 替换

**全平台发布**

同时发布到多个平台：

![全平台发布](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/publish_mode_all_platforms.png)

- 选择多个平台并发发布
- 每个平台独立处理
- 一个平台失败不影响其他平台
- 批量发布节省时间和精力

**已发布博客列表**

跟踪发布历史和状态：

![已发布博客列表](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/published_blog_list.png)

- 查看所有已发布文章及其状态指示
- 查看各平台成功或失败情况
- 识别需要重新发布的文章
- 各平台已发布内容的直达链接
- 支持更新已发布的文章

#### 2. 博客评论

监控所有平台的评论：

![博客评论](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/blog_comments.png)

- 所有已连接平台评论的集中视图
- 实时反馈收集
- 评论状态跟踪（新、已回复、已解决）
- AI 辅助回复建议
- 快速跳转到各平台原始评论
- 按平台和状态筛选评论

#### 3. 设置

**数据目录配置**

设置数据存储偏好：

![数据目录](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_data_directory.png)

- 配置博客数据本地存储位置
- 设置文章文件的默认博客文件夹
- 管理数据存储位置
- 查看存储使用信息

**博客数据设置**

配置博客相关设置：

![博客数据设置](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_data_blog.png)

- 设置默认博客目录
- 配置文章文件命名约定
- 设置预览选项
- 管理草稿和已发布内容的分离

**平台凭证**

配置每个支持平台的凭证：

**GitHub Pages 凭证**

![GitHub Pages 凭证](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_credentials_pages.png)

- 配置 GitHub 访问令牌
- 设置仓库信息
- 配置 Pages 部署设置

**微信公众号凭证**

![微信凭证](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_credentials_wechat.png)

- 配置微信公众号 API 凭证
- 设置 App ID 和 App Secret
- 配置 token 和 encodingAESKey

**知乎凭证**

![知乎凭证](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_credentials_zhihu.png)

- 配置知乎认证
- 设置 Playwright 模式以提高可靠性
- 配置浏览器自动化设置

**掘金凭证**

![掘金凭证](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_credentials_juejin.png)

- 配置掘金 API 凭证
- 设置访问令牌
- 配置发布选项

**头条号凭证**

![头条号凭证](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_credentials_toutiao.png)

- 配置头条号 API 凭证
- 设置客户端 ID 和客户端密钥
- 配置内容发布设置

**微博凭证**

![微博凭证](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_credentials_weibo.png)

- 配置微博 API 凭证
- 设置 App Key 和 App Secret
- 配置 OAuth 设置

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
| 应用概览 | 统一的发布管理界面 | App_overview.png |
| 发布模式 | 选择单平台或多平台发布 | publish_mode.png |
| 单平台发布 | 发布到特定平台 | publish_mode_one_platform.png |
| 全平台发布 | 同时发布到多个平台 | publish_mode_all_platforms.png |
| 已发布列表 | 查看发布历史和状态 | published_blog_list.png |
| 博客评论 | 监控所有平台评论 | blog_comments.png |
| 数据目录 | 配置数据存储位置 | settings_data_directory.png |
| 博客设置 | 配置博客相关设置 | settings_data_blog.png |
| GitHub Pages | 配置 GitHub Pages 凭证 | settings_credentials_pages.png |
| 微信 | 配置微信公众号凭证 | settings_credentials_wechat.png |
| 知乎 | 配置知乎凭证 | settings_credentials_zhihu.png |
| 掘金 | 配置掘金凭证 | settings_credentials_juejin.png |
| 头条号 | 配置头条号凭证 | settings_credentials_toutiao.png |
| 微博 | 配置微博凭证 | settings_credentials_weibo.png |

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