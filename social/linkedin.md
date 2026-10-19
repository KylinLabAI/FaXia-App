---
title: "FaXia: The Open-Source Tool That Publishes Your Markdown to 11 Platforms at Once"
platform: linkedin
language: en-US
date: 2026-07-10
description: "Announcing FaXia: a local-first, open-source publishing tool that lets content creators publish one Markdown file to 11+ platforms simultaneously."
keywords: ["FaXia", "content creation", "developer tools", "open source", "productivity"]
tags: ["OpenSource", "DeveloperTools", "ContentCreation", "Productivity"]
category: Tech
word_count: 650
author: "kylinlab.tech"
audience: "Software engineers, technical content creators, and developer advocates"
---

# FaXia: The Open-Source Tool That Publishes Your Markdown to 11 Platforms at Once

## The Challenge

Content creators in tech know this routine: you write a great article, then spend another hour distributing it. Open 11 tabs. Copy-paste into each editor. Re-upload images. Fix formatting quirks. Juggle Chinese and English versions. Realize you forgot one platform and start over.

For developer advocates and technical content creators, this is a tax on productivity — time spent on distribution instead of creation. And most existing solutions are cloud-based SaaS tools, which means trusting a third party with your platform credentials and content.

## Our Approach

**FaXia** takes a different approach: a fully local, open-source tool that runs on your machine. You provide one Markdown file. It publishes to every platform you've configured — in parallel, with per-platform error isolation.

The architecture is straightforward: a shared publishing core handles Markdown parsing, image upload pipelines, and format conversion. Eleven independent platform adapters implement a unified interface — API-based where possible (Medium, Reddit, GitHub Pages), Playwright-based where necessary (Toutiao, Weibo). Each adapter fails independently, so one platform's issue never blocks the rest.

Users choose their interface: a CLI (`faxia`) for scripting and CI integration, or an Electron desktop app for visual platform selection and credential management.

## Key Outcomes

- **11 platforms supported**: GitHub Pages, WeChat, Zhihu, Juejin, Toutiao, Weibo, Reddit, X/Twitter, Medium, Facebook, LinkedIn
- **Hours saved per week**: what used to take 60 minutes of copy-pasting now completes in seconds
- **100% local**: credentials, content, and publish history never leave the user's device
- **Open source (MIT)**: fully auditable, no vendor lock-in
- **Concurrent publishing**: all platforms fire in parallel with `Promise.allSettled` — one failure never blocks the rest

## What's Next

FaXia is in alpha. Core publishing works across all 11 platforms. On the roadmap: scheduled publishing, cross-platform analytics, and a plugin system for custom adapters.

We're looking for early users and contributors — especially people who publish across both Chinese and English platforms and feel the distribution pain firsthand.

What's your current workflow for multi-platform publishing? Are you using a tool, or still doing it manually? Share your experience below.
