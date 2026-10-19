---
title: "Stop Copy-Pasting Your Articles Across Platforms — I Built a Tool That Does It for Free"
platform: medium
language: en-US
date: 2026-07-10
description: "After years of manually posting articles to 11 different platforms, I built FaXia — an open-source, local-first tool that publishes everywhere at once."
keywords: ["FaXia", "writing", "productivity", "content creation", "open source", "markdown"]
tags: ["Writing", "Productivity", "Open Source", "Content Creation", "Developer Tools"]
category: Tech
word_count: 1100
author: "kylinlab.tech"
medium_tags: ["Writing", "Productivity", "Open Source", "Programming", "Technology"]
medium_canonical_url: ""
medium_publication: ""
---

# Stop Copy-Pasting Your Articles Across Platforms — I Built a Tool That Does It for Free

*Writing the article is the easy part. Publishing it everywhere is the grind.*

---

The best hour of my writing week is when I finish a draft. The worst hour is what comes next.

I write technical articles. Code samples, architecture diagrams, benchmarks — the kind of content that looks terrible when the formatting breaks. And I publish across both Chinese and English platforms because my audience is split between them.

The post-writing ritual used to go like this:

Open Zhihu. Paste. Fix the code blocks (Zhihu's editor strips indentation). Upload each image one by one. Publish.

Open Juejin. Paste. Different Markdown dialect — reformat the tables. Re-upload the images. Publish.

Open WeChat Official Account. Paste. Find that WeChat's rich text editor mangled the headings. Fix them. Upload images *again* — this time through their media management API. Publish.

Repeat for Toutiao, Weibo, Medium, Reddit, GitHub Pages, X/Twitter, Facebook, LinkedIn.

Eleven platforms. An hour of busywork. And if I fix a typo later? Start over.

## The Moment I Decided to Build It

It wasn't one moment. It was the hundredth time I realized I'd posted to 10 platforms but forgotten one. Or updated an article on Zhihu but not on Medium — so readers on different platforms were seeing different versions of the same piece.

I looked for existing solutions. A few multi-platform publishing tools exist, but they share the same problems:

- **SaaS model**: You give your platform credentials and content to a third party. For WeChat and Zhihu, that means handing over access tokens that can do *anything* on your account.
- **Limited platform coverage**: Most support 3-5 Western platforms. None support Toutiao or Weibo. None handle the Chinese/English split well.
- **Closed source**: Can't audit what happens to your data.

I wanted something that runs on my machine, covers the platforms I actually use, and doesn't send my credentials anywhere. So I built it.

## What FaXia Does

**It takes one Markdown file and publishes it to every platform you've configured — all at once, in parallel.**

The concept is simple. The implementation is where things got interesting.

### The Platform Matrix

FaXia supports 11 platforms. They break down into three categories:

**API-first platforms** (straightforward): GitHub Pages uses Git pushes. Medium, Reddit, X/Twitter, Facebook, and LinkedIn use REST APIs. Each has its quirks — Medium requires a `contentLicense` field that their docs barely mention, LinkedIn's API rate limits are aggressive — but the basic model is the same.

**Hybrid platforms** (tricky): WeChat Official Account, Zhihu, and Juejin *have* APIs, but they're incomplete or unreliable. WeChat's access token expires every 2 hours. Zhihu's API doesn't expose image upload for all account types. Juejin's API is undocumented and changes without notice. For these, FaXia uses API as the primary path with Playwright browser automation as a fallback.

**Headless-only platforms** (no choice): Toutiao and Weibo have no public write APIs at all. Everything goes through Playwright — simulating a real browser clicking through their web editors.

> The biggest engineering lesson here: platform APIs are promises, not guarantees. A platform can change its editor UI, deprecate an endpoint, or add a CAPTCHA at any moment. The only reliable strategy is defense in depth.

### Image Handling That Actually Works

Every platform has its own image hosting. You can't just link to a local file and expect it to work everywhere. FaXia's image pipeline:

1. Parse the Markdown AST and identify all local image references
2. For each platform being published to, upload each image to that platform's image hosting
3. Generate platform-specific output files with all image URLs rewritten to point to the correct hosting

This means a code screenshot in your article shows up on Zhihu loaded from Zhihu's CDN, on Medium loaded from Medium's CDN, and on GitHub Pages loaded from your repository. Zero broken images, zero manual uploads.

### The Interface Question: CLI vs GUI

I built both. The publishing core is a shared TypeScript library. The CLI (`faxia`) wraps it in a Commander.js interface — ideal for scripting, CI/CD pipelines, and anyone who lives in the terminal. The Electron desktop app wraps the same core with a visual platform selector, credential manager, and article preview.

Both are first-class interfaces. There's no "CLI is the real tool and the GUI is an afterthought" — and no "GUI is the product and CLI is a checkbox." They serve different workflows, and that's fine.

## What I Learned

Building adapters for 11 separate platforms teaches you things:

**APIs lie.** Or at least, they omit. The WeChat Official Account API documentation doesn't tell you that the material upload endpoint and the draft publish endpoint use different authentication scopes. You find that out by debugging 401 errors at 1 AM.

**Frontends are fragile.** When Toutiao updates their publishing UI (which they do every few months), CSS selectors break. The fix was a multi-level fallback strategy: try semantic selectors first (`button[name="publish"]`), then text content, then CSS classes, then XPath as a last resort. Each selector gets a 5-second timeout before falling through to the next.

**Concurrency is the easy part.** `Promise.allSettled` handles parallel publishing beautifully — one platform failing never blocks the rest. The hard part is the state management around *partial success*. If 9 out of 11 platforms succeed, how do you communicate that clearly to the user? How do they retry just the failed two? Getting the UX right here took more time than the publishing logic itself.

## Where It's At

FaXia is in **alpha**. All 11 platform adapters work. The CLI is stable. The desktop app is functional but still being polished (the credential setup flow in particular needs more guardrails and better error messages).

Coming next: scheduled publishing, a cross-platform analytics dashboard, and a plugin system so the community can contribute adapters for platforms I don't use.

## Try It

The project is fully open source under the MIT license. No cloud service, no telemetry, no account. Everything runs on your machine.

- **GitHub**: [KylinLabAI/FaXia-App](https://github.com/KylinLabAI/FaXia-App)
- **CLI quick start**: `npm install -g faxia && faxia publish your-article.md`
- **Desktop app**: download from the GitHub Releases page

If you publish content across multiple platforms — especially across languages — give it a try. I built it to solve my own problem, and I hope it solves yours too.

*If you found this helpful, follow for more content on developer tools and productivity. What's your biggest pain point in content distribution?*
