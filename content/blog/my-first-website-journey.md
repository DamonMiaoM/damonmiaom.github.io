---
title: "My First Website — A Beginner's Journey"
date: 2026-03-26T14:00:00
description: "How a life insurance agent with no coding background built and deployed a personal website in one afternoon — every obstacle, every fix, every lesson."
---

*一个没有编程背景的寿险顾问，如何在一个下午完整搭建并上线个人网站——每一个障碍、每一次修复、每一条经验。*

*How a life insurance agent with no coding background built and deployed a real website in one afternoon — every obstacle, every fix, and every lesson.*

---

## 我们做了什么
<p class="heading-note">What We Built</p>

我们用 **Hugo**（一个把简单文本文件转换成完整网站的工具）搭建了一个自定义"forest"主题的个人网站——简洁、极简，包含：

*A personal website using **Hugo** (a tool that turns simple text files into a full website) with a custom "forest" theme — clean, minimal, with:*

- 主页：个人标语 + 最新博客文章 / A homepage with tagline and recent blog posts
- About、Blog、Projects、Gallery 四个导航页面 / About, Blog, Projects, and Gallery pages
- 每次推送代码后自动部署 / Automatic deployment every time you push changes to GitHub

---

## 整体逻辑——它是如何运行的
<p class="heading-note">The Big Picture — How It All Works</p>

在深入每个障碍之前，先建立整体认知模型：

*Before diving into the obstacles, here is the mental model you need:*

```
你写内容（文本文件）
        ↓
Hugo 把它转成 HTML（真正的网站）
        ↓
Git 追踪所有变更（像存档历史）
        ↓
GitHub 把代码存到云端（备份 + 分享）
        ↓
GitHub Actions 自动构建并发布网站
        ↓
你的网站上线：damonmiaom.github.io
```

可以这样理解：你写一份 Word 文档，存到 iCloud，每次保存后都会自动打印并展示在橱窗里。

*Think of it like writing a Word document, saving it to iCloud, and having it automatically printed and displayed in a shop window every time you save.*

---

## 核心概念——用生活语言解释
<p class="heading-note">Key Concepts Explained Simply</p>

| 术语 | 通俗解释 |
|---|---|
| **Git** | 追踪每次文件变更——像 Word 的"修订追踪"，但用于代码 |
| **GitHub** | 在线存放代码，类似代码版的 iCloud |
| **Branch（分支）** | 项目的并行副本，可以安全实验，不影响主版本 |
| **Pull Request (PR)** | 请求将分支合入主版本——类似"请审核并批准这份草稿" |
| **Merge（合并）** | 批准 PR，正式将分支合入主线 |
| **GitHub Actions** | 推送代码后自动执行的任务——本项目中负责构建和发布网站 |
| **Hugo** | 将 Markdown 文本转成完整 HTML 网站的工具 |
| **Deploy（部署）** | 发布网站，让全世界都能访问 |

| Term | Plain English |
|---|---|
| **Git** | A system that tracks every change you make — like "Track Changes" in Word, but for code |
| **GitHub** | A website that stores your Git files online, like iCloud for code |
| **Branch** | A parallel copy of your project where you can experiment safely |
| **Pull Request (PR)** | A proposal to add your branch's changes into main — like "please review and approve this draft" |
| **Merge** | Accepting the PR — officially combining the branch into main |
| **GitHub Actions** | Automatic tasks that run whenever you push code — builds and publishes your website |
| **Hugo** | A tool that converts simple Markdown text files into a full HTML website |
| **Deploy** | Publishing your site so the world can see it |

---

## 实战记录——逐步经历
<p class="heading-note">The Journey — Step by Step</p>

### 第 0 步：开始之前的障碍
<p class="heading-note">Step 0: Getting Started</p>

**发生了什么：** Claude Code 桌面应用报错：

*What happened: Claude Code desktop app showed an error:*

> "Unable to start session: account information is unavailable because your sign-in has expired."

**为什么发生：** 登录会话超时过期——和任何 App 自动退出登录一样正常。

*Why it happened: The login session had expired — like being automatically logged out of an app after inactivity.*

**如何解决：** 点击左下角头像 → 退出登录 → 重新登录。就这么简单！

*How we fixed it: Clicked the avatar icon (bottom-left) → Sign Out → Sign In again. Simple!*

**经验：** 这是所有 App 都会出现的正常情况，不要慌，重新登录即可。

*Lesson: This is a normal thing that happens with any app. Don't panic — just re-login.*

---

### 第 1 步：路径错误——两个不同的世界
<p class="heading-note">Step 1: Wrong Path — Two Different Worlds</p>

**发生了什么：** Claude.ai Cowork 给出的指令是 `cd /home/user/damonmiaom.github.io`，但这个路径在 Mac 上根本不存在。

*What happened: Instructions from Claude.ai Cowork said `cd /home/user/damonmiaom.github.io` — but this path didn't exist on my Mac.*

**为什么发生：** 涉及两个完全不同的环境：

*Why it happened: There are TWO separate environments:*

- **Claude.ai Cowork** = 云端 Linux 电脑，路径格式是 `/home/user/...` / Cloud Linux computer
- **你的 Mac（Claude Code）** = 本地电脑，路径格式是 `/Users/Damon/...` / Your local Mac

**类比：** 就像有人给你一份上海某家店的导航，但你其实在北京。两个城市都是真实的，但导航只在正确的地方有效。

*The analogy: It's like someone giving you directions to a shop in Shanghai when you're actually in Beijing.*

---

### 第 2 步：克隆仓库
<p class="heading-note">Step 2: Cloning the Repository</p>

**我们做了什么：**

*What we did:*

```bash
cd /tmp && git clone https://github.com/DamonMiaoM/damonmiaom.github.io.git site
cd site
git checkout -b claude/build-hugo-site-Dak4R
```

- `git clone` = 把 GitHub 仓库下载一份到本地（像从 iCloud 下载文件夹）/ downloaded a copy to your Mac
- `git checkout -b` = 创建新分支（安全的并行副本）/ created a new safe branch

**为什么用分支：** 主版本网站保持干净。如果出了问题，不合并就行——主线不受影响。

*Why we used a branch: So the main version of your site stays clean. If something goes wrong, we just don't merge.*

---

### 第 3 步：GitHub 个人访问令牌
<p class="heading-note">Step 3: GitHub Personal Access Token</p>

**发生了什么：** 要从 Cowork 向 GitHub 推送代码，需要身份验证——证明你就是你。

*What happened: To push code to GitHub, we needed authentication — proof that you are really you.*

**如何解决：**

*How we fixed it:*

1. 前往 github.com/settings/tokens / Go to github.com/settings/tokens
2. 新建"classic"令牌，勾选 `repo` + `workflow` 权限 / Create a new "classic" token with `repo` + `workflow` scopes
3. 保存好（只显示一次！）/ Save it — it only shows once!
4. `git push` 提示输入密码时用它代替 / Use it as the password when git asks

**经验：** Token 就像你 GitHub 账户的钥匙，保管好，不要分享，不要粘贴进聊天窗口。

*Lesson: This token is like a key to your GitHub account. Keep it safe — never paste it into chat.*

---

### 第 4 步：命令粘贴到了错误的地方
<p class="heading-note">Step 4: Commands Pasted in the Wrong Place</p>

**发生了什么：** git push 命令被粘贴进了 Cowork 的**聊天框**，而不是**终端**。

*What happened: The git push commands got pasted into the Cowork **chat box** instead of the **terminal**.*

Claude.ai Cowork 有两个部分：聊天框（和 Claude 对话）和终端（运行命令）。它们看起来很像，但功能完全不同。

*Claude.ai Cowork has two parts: the chat box (talking to Claude) and the terminal (running commands). They look similar but do very different things.*

**经验：** 在编程工具中，按下回车前，先确认你在哪里输入。

*Lesson: In coding tools, always check WHERE you are typing before you hit Enter.*

---

### 第 5 步：创建 Pull Request 并合并
<p class="heading-note">Step 5: Creating the Pull Request and Merging</p>

在 GitHub 上创建 PR，将开发分支合入 `main`，然后点击"Merge pull request"。

*On GitHub, created a PR to merge the development branch → `main`, then clicked "Merge pull request."*

这触发了 GitHub Actions 自动构建并部署网站。

*This triggered GitHub Actions to automatically build and deploy the site.*

**安全网类比 / Safety net analogy:**

- 分支 = 旁边写的草稿 / Branch = a draft written on the side
- 主线 = 正式出版的书 / Main = the official published book
- PR = "请审核并批准" / PR = "please review and approve"
- 合并 = "批准，加入书中" / Merge = "approved, add it to the book"

---

### 第 6 步：第一次 404 错误
<p class="heading-note">Step 6: The First 404 Error</p>

**发生了什么：** 访问 damonmiaom.github.io，看到"404 — File not found"。

*What happened: Visited damonmiaom.github.io and saw a "404 — File not found" page.*

**为什么发生：** GitHub Pages 还未配置成使用 GitHub Actions，仍在用旧的 Jekyll 方式，无法构建 Hugo 网站。

*Why it happened: GitHub Pages was still using the old Jekyll method, which couldn't build a Hugo site.*

**如何解决：** Settings → Pages → Build and deployment → Source → 选择 **"GitHub Actions"**

*How we fixed it: Settings → Pages → Source → selected **"GitHub Actions"***

**经验：** 使用自定义构建工具（如 Hugo）时，务必将 GitHub Pages 源设置为"GitHub Actions"。

*Lesson: When using a custom build tool like Hugo, always set GitHub Pages source to "GitHub Actions."*

---

### 第 7 步：还是 404——竞争工作流问题
<p class="heading-note">Step 7: Still 404 — The Competing Workflows Problem</p>

**发生了什么：** 即使 Hugo 工作流显示 ✅ 成功，网站仍然 404。

*What happened: Even after the Hugo workflow showed ✅ Success, the site was still 404.*

**为什么发生（关键）：** PR 合并时，两个工作流同时运行：

*Why it happened: Two workflows ran at the same time:*

1. **我们的 Hugo 工作流** — 25 秒完成 ✅ / Our Hugo workflow — 25 seconds ✅
2. **GitHub 遗留工作流** — 42 秒完成 ✅ / GitHub's legacy workflow — 42 seconds ✅

遗留工作流晚了 17 秒完成，它**覆盖**了 Hugo 的部署。后完成者获胜——而获胜的是错误的那个。

*The legacy workflow finished 17 seconds LATER and **overwrote** Hugo's deployment. Last one to finish wins — and the wrong one won.*

**如何解决：** 手动触发一次新的 Hugo 工作流，没有竞争——成功保留。

*How we fixed it: Manually triggered a new Hugo workflow run. No competition this time — it stuck.*

**经验：** 两个进程写向同一个地方时，后完成者获胜。调试部署问题时，务必检查是否有竞争工作流。

*Lesson: When two processes write to the same place, the last one wins. Always check for competing workflows.*

---

### 第 8 步：成功！
<p class="heading-note">Step 8: SUCCESS!</p>

网站在 https://damonmiaom.github.io 完美加载。Forest 主题上线，导航、标语、第一篇文章一切就绪。

*The site loaded perfectly at https://damonmiaom.github.io — forest theme live, navigation, tagline, and first blog post all working.*

**从零到上线总计约 2 小时（含所有排错）**

*Total time from zero to live website: ~2 hours (including all the troubleshooting!)*

---

## 我学到了什么
<p class="heading-note">What I Learned</p>

1. **两个环境并存** — 云端和本地是两台不同的机器，要知道自己在哪里 / **Two environments exist** — cloud and local are separate machines
2. **Git 分支是安全网** — 自由实验，主线保持干净 / **Git branches are safety nets** — experiment freely, main stays clean
3. **PR + Merge = 安全发布方式** / **PR + Merge = the safe way** to publish changes
4. **GitHub Actions = 自动化** — 它替你构建和部署 / **GitHub Actions = automation** — builds and deploys for you
5. **404 不一定意味着失败** — 通常是"还没部署"或"配置有误" / **404 doesn't always mean failure** — often "not deployed yet"
6. **竞争工作流会互相覆盖** — 出问题时检查 Actions 标签 / **Competing workflows can overwrite each other** — check the Actions tab
7. **GitHub Token** = 推送代码的安全密钥 / **GitHub Token** = your secure key for pushing code

---

## 技术栈
<p class="heading-note">The Tech Stack</p>

| 工具 | 角色 | 类比 |
|---|---|---|
| **Hugo** | 从文本文件构建网站 | 印刷机 |
| **Git** | 追踪所有变更 | Word 的"修订追踪" |
| **GitHub** | 在线存储代码 | 代码版 iCloud |
| **GitHub Actions** | 自动化构建与部署 | 机器人出版商 |
| **GitHub Pages** | 免费托管网站 | 免费橱窗展示 |
| **Markdown** | 简单的内容文本格式 | 简化版 Word |

| Tool | Role | Analogy |
|---|---|---|
| **Hugo** | Builds the website from text files | A printing press |
| **Git** | Tracks all changes | "Track Changes" in Word |
| **GitHub** | Stores code online | iCloud for code |
| **GitHub Actions** | Automates build & deploy | A robot publisher |
| **GitHub Pages** | Hosts the website for free | A free shop window |
| **Markdown** | Simple text format for content | A simplified Word format |

---

*"学习技术最好的方式，就是去做一个真实的东西。"*

*"The best way to learn technology is to build something real."*

<p class="post-revised">Published: Mar 26, 2026, 14:00</p>
