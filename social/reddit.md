---
title: "I made an open-source tool that publishes Markdown to 11 platforms at once — no cloud, fully local"
platform: reddit
language: en-US
date: 2026-07-10
description: "FaXia: local-first, open-source publishing tool that takes one Markdown file and publishes it to 11 platforms simultaneously with auto image upload."
keywords: ["FaXia", "open source", "self-hosted", "publishing", "markdown", "content creation"]
tags: ["opensource", "selfhosted", "writing", "productivity"]
category: Tech
word_count: 600
author: "kylinlab.tech"
subreddit: "r/selfhosted"
reddit_flair: "Showcase"
reddit_post_type: "self"
---

# I made an open-source tool that publishes Markdown to 11 platforms at once — no cloud, fully local

**TL;DR:** FaXia takes one Markdown file, auto-uploads images, converts formatting, and publishes to 11 platforms (Zhihu, Medium, WeChat, Reddit, etc.) in parallel. Runs entirely on your machine. MIT licensed. GitHub: KylinLabAI/FaXia-App

---

I write technical content across both Chinese and English platforms. Every time I finish an article, I go through the same pain: open 11 tabs, copy-paste into each platform's editor, reformat code blocks and tables, re-upload images one by one, and make sure the Chinese version and English version don't diverge.

I looked for existing tools. Most are SaaS — your credentials and content go to someone else's server. Others support only 3-4 platforms. None supported Chinese platforms well.

So I built FaXia.

## What it does

- Takes one Markdown file (or a directory with platform-specific versions)
- Uploads local images to each platform's image hosting, rewrites URLs
- Converts Markdown to each platform's expected format (HTML with platform-specific quirks handled)
- Publishes to all 11 platforms concurrently
- Remembers where each article was published — re-running updates existing posts instead of creating duplicates
- Dry-run mode to validate everything before actually publishing

## How it works

It's an Electron desktop app + a Node.js CLI (`tdp`), sharing the same publishing core.

For platforms with public APIs (Medium, Reddit, GitHub Pages, LinkedIn, Facebook, X/Twitter), it uses their REST APIs directly. For platforms without APIs (Weibo, Toutiao) or where the API is unreliable (Zhihu), it uses Playwright to automate a real browser.

Each platform adapter is an independent module. They all implement the same interface, so the concurrent scheduler doesn't care about implementation details. `Promise.allSettled` means one platform failing never blocks the rest.

Your credentials (API tokens, cookies) are stored in a local config file or environment variables. Nothing leaves your machine.

## What's next

It's in alpha — all 11 platforms work, but the GUI is still being polished. Roadmap: scheduled publishing, cross-platform analytics dashboard, plugin system for custom adapters.

---

I built this for my own workflow, but it's been really useful, so I'm sharing it. It's MIT licensed — use it, fork it, contribute if you want.

I'd love feedback on the architecture, especially the Playwright-based adapters (those are the trickiest part). What would you want to see in a tool like this?
