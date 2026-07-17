# FaXia Demo Walkthrough

[中文介绍](demo.zh.md)

## Product Overview

![FaXia Logo](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/logo_256.png)

**FaXia** is a multi-platform content publishing tool that enables content creators to publish Markdown articles to 11+ platforms with one click. This demo walks through the key features including multi-platform publishing, blog status tracking, comment monitoring, and platform credential management.

![App Overview](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/App_overview.png)

- Unified interface for managing all your content publishing needs
- Four main tabs: Publish, Published, Comments, and Settings
- Support for GitHub Pages, WeChat Official Account, Zhihu, Juejin, Toutiao, Weibo, Reddit, X/Twitter, Medium, Facebook, and LinkedIn

## Core Value

| Benefit | How it's achieved |
|---|---|
| **Save hours per week** | 11+ platforms concurrent publishing, what used to take an hour is done in seconds |
| **Reach readers everywhere** | Native adapters for all major Chinese and English platforms |
| **Images never break** | Automatic image upload and URL replacement per platform |
| **Correct formatting every time** | Smart Markdown to HTML with syntax highlighting, tuned for each platform |
| **Safe preview before publishing** | Dry-run mode and credential validation |
| **Your content stays yours** | Open-source, local-only tool with no cloud service |

## Applicable Roles

- **Content Creators**: Writers, bloggers, technical authors who publish across multiple platforms
- **Developer Advocates**: Technical professionals who need to distribute content to both Chinese and English audiences
- **Marketing Teams**: Teams that need to publish the same content across different social platforms

---

## Business Process Overview

### Flow Diagram

```
Content Creation → Platform Adaptation → FaXia Publishing → Multi-platform Distribution → Feedback Collection → Content Iteration
```

### Phase Table

| Phase | Description | Tools Involved |
|-------|-------------|----------------|
| Content Creation | Write core content in Markdown | Any Markdown editor |
| Platform Adaptation | Create platform-specific versions if needed | FaXia directory mode |
| Publishing | Publish to all configured platforms | FaXia CLI or desktop app |
| Distribution | Content live on all platforms | Platform APIs / Playwright |
| Feedback Collection | Monitor comments across platforms | FaXia Comments tab |
| Content Iteration | Update and republish articles | FaXia update feature |

---

## Role Details

### Content Creator Walkthrough

#### 1. Publishing

**Publish Mode Selection**

Choose how you want to publish your content:

![Publish Mode](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/publish_mode.png)

- Select between publishing to a single platform or all platforms
- Choose your Markdown file for publishing
- Configure publishing options before execution

**Publish to One Platform**

Publish your content to a specific platform:

![Publish to One Platform](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/publish_mode_one_platform.png)

- Select a single target platform from the available options
- Preview content before publishing
- Supports dry-run mode for verification
- Automatic image upload and URL replacement

**Publish to All Platforms**

Simultaneously publish to multiple platforms:

![Publish to All Platforms](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/publish_mode_all_platforms.png)

- Select multiple platforms for concurrent publishing
- Each platform processes independently
- One platform failure does not affect others
- Batch publishing saves time and effort

**Published Blog List**

Track your publishing history and status:

![Published Blog List](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/published_blog_list.png)

- View all published articles with status indicators
- See which platforms succeeded and which failed
- Identify articles that need re-publishing
- Direct links to published content on each platform
- Support for updating previously published articles

#### 2. Blog Comments

Monitor comments across all platforms:

![Blog Comments](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/blog_comments.png)

- Centralized view of comments from all connected platforms
- Real-time feedback collection
- Comment status tracking (new, replied, resolved)
- AI-assisted reply suggestions
- Quick navigation to original comment on each platform
- Comment filtering by platform and status

#### 3. Settings

**Data Directory Configuration**

Set up your data storage preferences:

![Data Directory](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_data_directory.png)

- Configure where blog data is stored locally
- Set default blog folder for article files
- Manage data storage location
- View storage usage information

**Blog Data Settings**

Configure blog-related settings:

![Blog Data Settings](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_data_blog.png)

- Set default blog directory
- Configure article file naming conventions
- Set preview options
- Manage draft and published content separation

**Platform Credentials**

Configure credentials for each supported platform:

**GitHub Pages Credentials**

![GitHub Pages Credentials](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_credentials_pages.png)

- Configure GitHub access token
- Set repository information
- Configure Pages deployment settings

**WeChat Official Account Credentials**

![WeChat Credentials](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_credentials_wechat.png)

- Configure WeChat Official Account API credentials
- Set app ID and app secret
- Configure token and encodingAESKey

**Zhihu Credentials**

![Zhihu Credentials](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_credentials_zhihu.png)

- Configure Zhihu authentication
- Set up Playwright mode for better reliability
- Configure browser automation settings

**Juejin Credentials**

![Juejin Credentials](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_credentials_juejin.png)

- Configure Juejin API credentials
- Set access token
- Configure publishing options

**Toutiao Credentials**

![Toutiao Credentials](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_credentials_toutiao.png)

- Configure Toutiao API credentials
- Set client ID and client secret
- Configure content publishing settings

**Weibo Credentials**

![Weibo Credentials](https://raw.githubusercontent.com/KylinLabAI/FaXia-App/master/demo/resources/settings_credentials_weibo.png)

- Configure Weibo API credentials
- Set app key and app secret
- Configure OAuth settings

---

## Technical Architecture

### Overall Architecture

```
┌─────────────────────────────────────┐
│         User Interface Layer        │
│  ┌─────────────┐  ┌───────────────┐ │
│  │ Electron GUI │  │   faxia CLI   │ │
│  └──────┬───────┘  └──────┬────────┘ │
├─────────┼──────────────────┼──────────┤
│         ▼                  ▼          │
│  ┌─────────────────────────────────┐ │
│  │       Publishing Engine Core     │ │
│  │  • Markdown Parsing & Conversion│ │
│  │  • Image Upload Pipeline        │ │
│  │  • Publish History Management   │ │
│  │  • Concurrent Scheduler         │ │
│  └──────────────┬──────────────────┘ │
├─────────────────┼────────────────────┤
│                 ▼                    │
│  ┌─────────────────────────────────┐ │
│  │      Platform Adapters (11)     │ │
│  │  API | Hybrid | Headless        │ │
│  └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

### Key Highlights

1. **Shared Core**: Both CLI and desktop app use the same TypeScript publishing engine
2. **Concurrent Publishing**: Uses `Promise.allSettled` for parallel execution with error isolation
3. **Multi-layer Platform Support**: API-based for platforms with official APIs, Playwright-based for those without
4. **Image Pipeline**: Automatic upload to each platform's image hosting with URL rewriting
5. **Local-First**: All credentials and data stored locally, never sent to external servers

---

## Business Scale and Constraints

### Scale

- Supports unlimited articles and publishing history
- Concurrent publishing limited by platform API rate limits
- Desktop app tested with 11 platforms running simultaneously

### Constraints

- Platform API rate limits may affect publishing speed
- Some platforms require Playwright mode which needs Chromium installation
- WeChat access tokens expire every 2 hours (auto-refreshed by the tool)
- Platform UI changes may require adapter updates

### Limitations

- Some platforms do not support article updates (create-only)
- Video uploads not currently supported
- Scheduled publishing is on the roadmap

---

## Quick Reference

| Feature | Description | Image |
|---------|-------------|-------|
| App Overview | Unified interface for publishing management | App_overview.png |
| Publish Mode | Select single or multi-platform publishing | publish_mode.png |
| Single Platform | Publish to one specific platform | publish_mode_one_platform.png |
| All Platforms | Publish to multiple platforms concurrently | publish_mode_all_platforms.png |
| Published List | View publishing history and status | published_blog_list.png |
| Blog Comments | Monitor comments across all platforms | blog_comments.png |
| Data Directory | Configure data storage location | settings_data_directory.png |
| Blog Settings | Configure blog-related settings | settings_data_blog.png |
| GitHub Pages | Configure GitHub Pages credentials | settings_credentials_pages.png |
| WeChat | Configure WeChat Official Account credentials | settings_credentials_wechat.png |
| Zhihu | Configure Zhihu credentials | settings_credentials_zhihu.png |
| Juejin | Configure Juejin credentials | settings_credentials_juejin.png |
| Toutiao | Configure Toutiao credentials | settings_credentials_toutiao.png |
| Weibo | Configure Weibo credentials | settings_credentials_weibo.png |

---

## Getting Started

1. **Download** — Get the installer from [GitHub Releases](https://github.com/KylinLabAI/FaXia-App/releases/latest)
2. **Install** — Run the installer and follow prompts
3. **Launch** — Open FaXia from your applications folder
4. **Configure Credentials** — Go to Settings tab and configure credentials for each platform
5. **Prepare Content** — Write your article in Markdown format
6. **Publish** — Select your file and choose target platforms, then click publish
7. **Monitor** — Check Published tab for status and Comments tab for feedback

---

**One creation, multi-platform publishing.**

[Download FaXia](https://github.com/KylinLabAI/FaXia-App/releases/latest)