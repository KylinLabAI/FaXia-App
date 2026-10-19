# FaXia Demo Walkthrough

[中文介绍](demo.zh.md)

## Product Overview

![FaXia Logo](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/logo_256.png)

**FaXia** is a multi-platform content publishing tool that enables content creators to publish Markdown articles to 11+ platforms with one click. This demo walks through the key features including multi-platform publishing, blog status tracking, comment monitoring, and platform credential management.

![App Overview](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-publish.png)

- Unified interface for managing all your content publishing needs
- Six main tabs: Publish, Published, Comments, Settings, Account, and About (plus a Feedback button)
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

**Publish Mode & Platform Selection**

Choose a single file or a multi-platform directory, then tick the target platforms:

![Publish](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-publish.png)

- Toggle between **Single File** and **Directory (Multi-Platform)** modes
- Pick the Markdown file to publish
- Select target platforms from the checklist (Pages, Toutiao, WeChat, Zhihu, Juejin, Weibo, and more)
- Automatic image upload and URL replacement per platform

**Publishing Progress**

Watch each platform publish concurrently with live status:

![Publishing](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-publishing.png)

- Each platform processes independently with a "Working…" indicator
- One platform failure does not affect the others
- Use **Dry Run** to preview, then **Publishing…** to execute

**Publish Results**

Review the per-platform outcome and direct links:

![Published blogs](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-published-blogs.png)

![Published micro-blogs](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-published-miroblogs.png)

- See which platforms succeeded, which are pending, and which failed
- Direct links to the published content on each platform
- WeChat surfaces a **Draft** status with the pending-publish reason
- Identify articles that need re-publishing or manual follow-up

#### 2. Published History & Sync

Browse your full publishing history and re-sync records from each platform:

![Published](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-settings-4.png)

- View all published articles grouped by title, with per-platform records
- **Sync from <platform>** pulls the latest published list back into FaXia
- **Remove Duplicates** cleans repeated history entries
- Totals show how many blogs and platform records are tracked

#### 3. Blog Comments

Monitor comments across all platforms:

![Comments](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-comments.png)

- Centralized, per-article list of comments from connected platforms
- Filter by platform (with comment counts)
- Jump to the original article or its comment thread
- Manage replies from within the app

#### 4. Settings

**App Data Directory & Log Level**

![Settings: data directory and log level](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-settings-1.png)

- Configure where sessions, history, screenshots, and config are stored
- Reset to the platform default location
- Set the app log verbosity (fatal → detail); changes apply immediately and persist across restarts

**Blog Root Directory**

![Settings: blog root](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-settings-2.png)

- Point FaXia at the root folder where your blog Markdown files live
- Used to locate article files for publishing

**Platform Credentials**

Configure credentials for each platform from one unified panel:

![Settings: credentials](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-settings-3.png)

- Platform chips show configured (filled) vs. not-configured (empty) at a glance
- Pages (GitHub/GitLab/Gitee): provider, access token, repository, branch, and publish/image directories, with **Save** and **Test**
- Cookie / Playwright platforms (Zhihu, Juejin, Toutiao, Weibo): log in once and the session is persisted for headless publishing

#### 5. Account

Manage LianXia integration and sign-in:

![Account](https://raw.githubusercontent.com/KylinLabAI/Faxia-App/master/demo/resources/faxia-account.png)

- View integration, app, device, and sign-in status
- **Check for updates**
- Sign in with GitHub or WeChat

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
| Publish & Mode | Single-file or directory, pick target platforms | faxia-publish.png |
| Publishing Progress | Concurrent per-platform publishing + dry run | faxia-publishing.png |
| Publish Results | Per-platform success / draft / failure with links | faxia-published-blogs.png |
| Publish Results (micro) | Micro-blog publish results | faxia-published-miroblogs.png |
| Published History & Sync | History list + per-platform sync | faxia-settings-4.png |
| Blog Comments | Monitor comments across all platforms | faxia-comments.png |
| Data Directory & Log | Storage location + log verbosity | faxia-settings-1.png |
| Blog Root | Blog files location | faxia-settings-2.png |
| Credentials | Unified platform credential panel | faxia-settings-3.png |
| Account | LianXia integration & sign-in | faxia-account.png |

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