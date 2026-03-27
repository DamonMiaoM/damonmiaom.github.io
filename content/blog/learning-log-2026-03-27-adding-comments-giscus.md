---
title: "How I Added a Comment Zone to My Blog — and What It Took to Get There"
date: 2026-03-27
author: Damon Miao
description: "A step-by-step account of adding Giscus comments to a Hugo site on GitHub Pages — image swaps, gallery lightboxes, debugging errors, and a 5-step app setup done by hand."
tags: [learning-log, hugo, giscus, github, comments, beginners]
slug: adding-comments-giscus-hugo
---

# Learning Log — March 27, 2026
# 学习日志 — 2026年3月27日

**Topic:** Adding a Comment Zone, a New Hero Image, and a Gallery Lightbox
**主题：** 为博客添加评论区、更换首页背景图、图库弹出效果

---

## What Happened Today / 今天发生了什么

Today I upgraded my personal site in three meaningful ways. I swapped the homepage background from a lotus to a water ripple photo I personally chose. I added a click-to-enlarge effect to the gallery. And most importantly — I gave my readers a place to talk back. Setting up the comment system turned out to be a five-step journey that required me to read, copy, and paste values myself. I did every step. That felt good.

今天我对个人网站做了三项有意义的升级：把首页背景从莲花换成了我亲自选的水波纹照片，为图库添加了点击放大效果，最重要的是——我给读者开辟了一个可以回应我的地方。评论系统的搭建是一段五步的旅程，需要我亲手阅读、复制和粘贴数值。每一步我都自己完成了。那种感觉很好。

---

## Change #1: A New Hero Image
## 变化 #1：换一张新的首页背景图

I had a photo saved in my `Personal Website/Images` folder — dark teal water with soft concentric ripples. It felt more like me than the lotus. So I asked Claude to swap it in.

我在 `Personal Website/Images` 文件夹里有一张照片 — 深青色的水面，柔和的同心涟漪。它比莲花更像我。于是我让 Claude 把它换进去。

**What changed in the code:**

The image is referenced in `main.css` in two places — once for the homepage hero banner, once for a sidebar panel. Both lines had to be updated:

```css
/* Before */
background-image: url('/images/lotus.jpg');

/* After */
background-image: url('/images/ripple.jpg');
```

The photo file was also copied into the site's `static/images/` folder, and added to the Gallery page using the same `<figure>` format as the existing lotus entry.

**新概念：静态文件夹 (Static Folder)**

In Hugo, anything placed in the `static/` folder becomes directly accessible on the website. Put `ripple.jpg` there, and the site can find it at `/images/ripple.jpg`. Think of it as the site's public filing cabinet.

在 Hugo 里，放入 `static/` 文件夹的内容可以直接在网站上访问。把 `ripple.jpg` 放进去，网站就能通过 `/images/ripple.jpg` 找到它。把它想象成网站的公开文件柜。

---

## Change #2: Gallery Lightbox — Images That Jump to Greet You
## 变化 #2：图库弹出效果 — 像打招呼一样弹起来的图片

I wanted gallery images to expand when clicked — not just scale up flatly, but with a little bounce, like a friendly greeting. This is called a **lightbox** effect.

我希望图库的图片点击后能放大 — 不是平淡地缩放，而是带点弹跳感，像一个友好的打招呼。这叫做**灯箱**效果。

**How it works:**

1. A hidden overlay (`<div id="lightbox">`) sits invisibly over the whole page
2. Clicking an image copies its source into the overlay and makes it visible
3. A CSS animation makes the image bounce in using a spring-like curve

```css
@keyframes gallery-bounce {
  0%   { transform: scale(0.4); opacity: 0; }
  60%  { transform: scale(1.06); opacity: 1; }
  80%  { transform: scale(0.97); }
  100% { transform: scale(1); }
}
```

The numbers `0.4 → 1.06 → 0.97 → 1` are what create the bounce — it overshoots slightly, then settles. Press `Esc` or click anywhere to close.

数字 `0.4 → 1.06 → 0.97 → 1` 就是弹跳的秘密 — 稍微超出一点，然后回落到位。按 `Esc` 或点击任意处关闭。

---

## Change #3: Adding a Comment Zone (The Main Event)
## 变化 #3：添加评论区（今天的重头戏）

### Why Giscus?

For a static site on GitHub Pages, you can't have a database — so traditional comment systems don't work. **Giscus** solves this by storing comments in **GitHub Discussions**, which is free, has no ads, and fits the minimal style of my site perfectly.

对于 GitHub Pages 上的静态网站，没有数据库 — 传统评论系统无法使用。**Giscus** 的解法是把评论存储在 **GitHub Discussions** 里，免费、无广告，风格简洁，完全符合我网站的气质。

### The Five Steps I Completed

| Step | What I Did | How |
|------|-----------|-----|
| 1 | Installed the Giscus GitHub App | `github.com/apps/giscus` → Install → selected my repo |
| 2 | Enabled GitHub Discussions | Repo Settings → Features → checked Discussions |
| 3 | Got my Repo ID | Claude fetched it via the GitHub API: `R_kgDORxEswg` |
| 4 | Got my Category ID | Visited `giscus.app`, entered my repo, copied `data-category-id` |
| 5 | Typed the Category ID myself | Pasted `DIC_kwDORxEsws4C5X_o` directly into the chat |

Step 5 was my choice. I wanted to prove to myself I'd actually read and understood the instructions — not just watched Claude do everything.

第五步是我的主动选择。我想向自己证明，我真的读懂了指引 — 不只是在看 Claude 操作。

### The Error Along the Way

Before the setup was complete, the comment box showed this message:

> *An error occurred: giscus is not installed on this repository*

**What this meant:** The code was correctly placed in the template, but the Giscus app hadn't been installed on GitHub yet. The fix wasn't in the code — it was a permission step in GitHub settings.

**这意味着什么：** 代码已经正确写入模板，但 Giscus 应用还没有在 GitHub 上安装。问题不在代码里 — 而是 GitHub 设置里的一个授权步骤。

**Key lesson:** Not every error means the code is wrong. Sometimes the infrastructure hasn't been set up yet.

**关键领悟：** 不是每个错误都意味着代码写错了。有时候是底层的基础设施还没准备好。

### Where Comments Appear (And Where They Don't)

Comments only appear on **blog posts** — not on Projects, About, or Gallery. This was done with a one-line condition in the Hugo template:

评论区只出现在**博客文章**里 — 不在项目、关于或图库页面。这通过 Hugo 模板里的一行条件实现：

```html
{{ if eq .Section "blog" }}
  <!-- Giscus comment widget -->
{{ end }}
```

**What this means in plain English:** "Only show the comment section if we're currently inside the blog section."

**用中文说：** "只有当前页面属于博客板块时，才显示评论区。"

---

## New Vocabulary / 今天学到的新词汇

### Static Site / 静态网站
A website made of fixed files — HTML, CSS, images. No database, no server-side logic. Fast, cheap to host, but requires creative solutions for dynamic features like comments.

由固定文件组成的网站 — HTML、CSS、图片。没有数据库，没有服务器端逻辑。速度快、托管便宜，但需要创意来实现评论这样的动态功能。

### GitHub Discussions
A built-in feature of GitHub repositories where people can have threaded conversations. Giscus turns these into a blog comment system — each blog post maps to one Discussion thread.

GitHub 仓库的内置功能，允许人们进行有主题的对话。Giscus 把它变成博客评论系统 — 每篇博客文章对应一个 Discussion 线程。

### Node ID / 节点 ID
GitHub's internal identifier for a repository — different from the numeric ID. Giscus requires this specific format (e.g., `R_kgDORxEswg`) because it uses GitHub's GraphQL API under the hood.

GitHub 对仓库的内部标识符 — 与数字 ID 不同。Giscus 需要这种特定格式（例如 `R_kgDORxEswg`），因为它底层使用的是 GitHub 的 GraphQL API。

---

## Summary / 今日总结

| What | Result |
|------|--------|
| Hero image | Swapped lotus → water ripple |
| Gallery | New photo added + click-to-enlarge lightbox |
| Comment zone | Fully live on all blog posts via Giscus |
| Bug fixed | Comments removed from Projects page |
| Personal milestone | Typed and submitted a config value myself for the first time |

---

## A Note to My Future Self / 写给未来的自己

If you ever need to set up Giscus on a new site, the checklist is:
1. Install app at `github.com/apps/giscus`
2. Enable Discussions in repo Settings
3. Get `data-repo-id` and `data-category-id` from `giscus.app`
4. Paste both into the `single.html` template
5. Make the comment block conditional: `{{ if eq .Section "blog" }}`

如果你以后需要在新网站上配置 Giscus，检查清单如下：
1. 在 `github.com/apps/giscus` 安装应用
2. 在仓库设置里开启 Discussions
3. 从 `giscus.app` 获取两个 ID
4. 粘贴到 `single.html` 模板里
5. 加上条件判断，只在博客页面显示

---

*Written with Claude Code | Ghostty Terminal | MacBook Pro*
*2026-03-27*
