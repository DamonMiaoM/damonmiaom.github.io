---
title: "How I Built a Journal Section with a Web Editor — No Terminal Required"
date: 2026-03-27T18:00:00
author: Damon Miao
description: "A full account of creating a separate Journal section on my Hugo site and setting up Decap CMS with Netlify Identity — so I can write and publish from any browser, no code needed."
tags: [learning-log, hugo, cms, netlify, journal, beginners]
slug: journal-cms-setup
---

# Learning Log — March 27, 2026
# 学习日志 — 2026年3月27日

**Topic:** Building a Journal Section + Web-Based Publishing with Decap CMS
**主题：** 创建 Journal 栏目 + 用 Decap CMS 实现网页端直接发布

---

## What Happened Today / 今天发生了什么

Today I gave my site a new dimension. I created a separate Journal section — distinct from the Blog — and set up a web-based editor so I can write and publish new entries directly from a browser, without touching the terminal or pushing any code. The path to get there involved more dead ends than expected, but each one taught me something real about how the web works.

今天我为网站增添了新的维度。我创建了一个独立的 Journal 栏目——区别于 Blog——并搭建了一个网页编辑器，让我可以直接在浏览器里写作和发布，不需要碰终端或推送代码。这条路上遇到的弯路比预期的多，但每一个弯路都让我真正理解了一些网络运作的原理。

---

## Why a Separate Journal Section? / 为什么要单独建一个 Journal？

My Blog has a clear theme: *Notes on AI, technology, and thinking.* A dinner journal, a travel note, a personal reflection — those belong somewhere else. Mixing them would dilute both.

The Journal is for daily life. The Blog is a learning log. Two different registers, two different audiences, two different sides of the same person.

我的 Blog 有明确主题：AI、技术、思考的笔记。吃饭日记、旅行记录、个人感悟——那些应该有自己的位置。混在一起只会稀释彼此。

Journal 是日常生活。Blog 是学习日志。两种不同的语气，两种不同的读者，同一个人的两面。

---

## What Changed in the Code / 代码层面发生了什么

### 1. New Hugo Section: `/journal/`

Hugo organizes content into **sections** — folders inside `content/`. Creating `content/journal/` automatically makes `damonmiaom.github.io/journal/` a real page. I added:

- `content/journal/_index.md` — the section index
- `themes/forest/layouts/journal/list.html` — the archive list layout
- `themes/forest/layouts/journal/single.html` — the individual entry layout

Hugo 通过**栏目**（`content/` 里的文件夹）组织内容。创建 `content/journal/` 会自动生成 `damonmiaom.github.io/journal/` 页面。

**New concept: Hugo Layouts**

Hugo applies different templates to different sections. By creating `layouts/journal/`, I told Hugo: *"use these specific templates for Journal pages, not the default ones."* Each section can have its own look and feel.

**新概念：Hugo 布局（Layouts）**

Hugo 对不同栏目应用不同模板。通过创建 `layouts/journal/`，我告诉 Hugo："对 Journal 页面用这套专属模板，不用默认的。"每个栏目都可以有自己独特的外观。

### 2. Journal Titles Are Automatic Dates

Each Journal entry uses `.Date.Format "Jan 2, 2006"` in the template — so the heading always renders as **"Mar 27, 2026"** regardless of what filename I use. No manual title needed.

每篇 Journal 的标题在模板里使用 `.Date.Format "Jan 2, 2006"`，所以标题永远自动渲染为 **"Mar 27, 2026"** 这样的日期格式，无需手动输入。

### 3. Distinct Visual Identity

The Journal list page uses the same split-layout as the Blog, but with a different photo on the right panel:

| Section | Side Image |
|---------|-----------|
| Blog | `ripple.jpg` — dark teal water ripples |
| Journal | `waterlily.jpg` — white lotus on still water |

This single CSS line creates the distinction:

```css
.journal-split-right {
  background-image: url('/images/waterlily.jpg') !important;
}
```

---

## Setting Up the Web Editor / 搭建网页编辑器

This was the most technical part — and the most educational.

### What I Wanted

A URL I can open in any browser → log in → write → upload photos → click Publish → done. No terminal. No git. No Markdown syntax required (Rich Text mode handles it).

我想要的：打开某个网址 → 登录 → 写作 → 上传图片 → 点发布 → 完成。不需要终端，不需要 git，不需要 Markdown 语法。

### The Tool: Decap CMS

Decap CMS is an open-source web interface that saves your content directly to GitHub as Markdown files. It's invisible to visitors — only the author sees it.

Decap CMS 是一个开源网页界面，把内容直接以 Markdown 文件保存到 GitHub。访客看不到它，只有作者能使用。

The admin panel lives at a special URL. Everything I write there becomes a real file committed to my GitHub repo, which triggers GitHub Actions to rebuild and deploy the site.

管理界面在一个专属网址。我在那里写的所有内容都会变成提交到 GitHub 仓库的真实文件，触发 GitHub Actions 重新构建并部署网站。

### The Dead End: PKCE Authentication

My site is hosted on GitHub Pages, which is a static server — it can't run server-side code. Decap CMS has a feature called **PKCE** designed specifically for this: a way to authenticate with GitHub directly from the browser, no server needed.

我的网站托管在 GitHub Pages——一个静态服务器，不能运行服务端代码。Decap CMS 有一个叫 **PKCE** 的功能专门为此设计：直接在浏览器里通过 GitHub 认证，无需服务器。

In theory. In practice, Decap CMS 3.x has a persistent bug where PKCE config is silently ignored — the CMS always falls back to Netlify's authentication proxy, which returns "Not Found" unless your site is registered there.

理论上如此。实际上，Decap CMS 3.x 有一个持久性 bug：PKCE 配置被静默忽略——CMS 总是回退到 Netlify 的认证代理，而该代理对未注册的网站返回 "Not Found"。

**New concept: OAuth Authentication**

When you click "Login with GitHub" on a website, GitHub needs to verify you are who you say you are, then tell the website "yes, this person is allowed." This exchange requires a trusted middleman. PKCE was supposed to eliminate the middleman. When it failed, we needed a real one.

**新概念：OAuth 认证**

当你在网站上点击"用 GitHub 登录"时，GitHub 需要验证你的身份，然后告诉网站"是的，这个人有权限"。这个交换需要一个可信的中间人。PKCE 本应消除中间人。当它失败时，我们需要一个真正的中间人。

### The Solution: Netlify as Auth Proxy

**Netlify** is a free hosting platform with built-in Identity services. The solution: deploy the same GitHub repo to Netlify (for free), use Netlify purely as the authentication layer, while the main site continues to live on GitHub Pages.

**Netlify** 是一个内置身份验证服务的免费托管平台。解决方案：把同一个 GitHub 仓库也部署到 Netlify（免费），纯粹用 Netlify 做认证层，而主网站继续托管在 GitHub Pages 上。

The writing workflow now uses the Netlify URL for the admin panel. Every published entry commits to GitHub, triggers GitHub Actions, and appears on `damonmiaom.github.io` within a minute.

写作流程现在用 Netlify 的网址访问管理界面。每篇已发布的日记都会提交到 GitHub，触发 GitHub Actions，在一分钟内出现在 `damonmiaom.github.io` 上。

### The Setup Steps I Completed by Hand

| Step | What I Did |
|------|-----------|
| 1 | Created a Netlify account via GitHub login |
| 2 | Connected my GitHub repo to Netlify |
| 3 | Set build command to `hugo --buildFuture` |
| 4 | Enabled Netlify Identity |
| 5 | Enabled Git Gateway (lets Netlify commit to GitHub on my behalf) |
| 6 | Invited myself as an admin user |
| 7 | Set my password via the recovery email |

Steps 4–7 are the human parts. Every time I clicked a button and filled in a field myself, I understood a little more about what was actually happening.

第 4–7 步是我亲手完成的部分。每次我自己点击按钮、填写字段，我就对正在发生的事情多理解了一点。

---

## My First Journal Entry / 我的第一篇日记

Before even testing images, I wrote something real — a reflection on why I want this Journal at all:

在测试图片上传之前，我写下了真实的内容——一篇关于为什么想要这个 Journal 的思考：

> *在这里疯狂地记日记。不同于 Blog 里的内容，Journal 中的都是日常生活。反观 Blog，我现在都是让 Claude Code 帮我完成一项任务之后，wrap up 成一篇技术性的文章，在我看来它本质上是 Learning Log。先拿到结果，再学习。*

That line — *先拿到结果，再学习* — is actually a good description of how I've been learning to use AI tools. Get the outcome first. Understand it after. Both things are real learning.

那句话——*先拿到结果，再学习*——其实很好地描述了我学习使用 AI 工具的方式。先获得结果，再去理解。两者都是真正的学习。

---

## What I Can Now Do / 我现在能做什么

| Task | How |
|------|-----|
| Write a Journal entry | Open `sparkly-klepon-26f158.netlify.app/admin/` → New Entry |
| Upload photos | Rich Text mode → image icon → select from computer |
| Embed videos | Paste YouTube/Bilibili link in Rich Text mode |
| Publish | Click Publish → live in ~1 minute |
| Write a Blog post | Still via terminal / git push (intentional — Blog = technical log) |

The two workflows match the two purposes. Journal entries are spontaneous and personal — low barrier, browser only. Blog posts are more considered and technical — worth the extra step of opening the terminal.

两种工作流匹配两种用途。Journal 随性而私人——门槛低，只需浏览器。Blog 更深思熟虑、更技术性——值得多走开终端这一步。

---

## Key Concepts Learned Today / 今天学到的关键概念

**Hugo Sections & Layouts** — Content organization and template routing in a static site generator.
Hugo 的栏目与布局 — 静态网站生成器中的内容组织与模板路由。

**CMS (Content Management System)** — A web interface for editing site content without touching code.
内容管理系统 — 无需接触代码即可编辑网站内容的网页界面。

**OAuth & Authentication Proxy** — How websites verify your identity through a trusted third party.
OAuth 与认证代理 — 网站如何通过可信第三方验证你的身份。

**Static vs. Dynamic Hosting** — GitHub Pages can serve files but not run code. Netlify can do both.
静态与动态托管的区别 — GitHub Pages 只能提供文件，不能运行代码。Netlify 两者都能做。

**Git Gateway** — A service that lets Netlify commit files to your GitHub repo on your behalf.
Git Gateway — 一项让 Netlify 代表你向 GitHub 仓库提交文件的服务。
