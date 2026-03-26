---
title: "How to Add a Project to Your Personal Website"
date: 2026-03-26T16:00:00
---

*一个关于"把新项目展示在个人网站上"的完整记录——每一条命令、每一个概念、每一个新手需要知道的道理。*

*A complete record of how to showcase a new project on your personal website — every command, every concept, and everything a beginner needs to understand.*

---

## 我们做了什么
<p class="heading-note">What We Built</p>

在我的个人网站 Projects 页面上，新增了 **Word to PPT Transformer** 这个项目的展示入口——和已有的 AI Glossary 保持一致的格式：项目名称、简介、跳转链接。

整个过程只需要改动**一个文件**，再执行三条命令。没有界面操作，没有拖拽，全部通过终端完成。

*We added the **Word to PPT Transformer** project to the Projects page of the personal website — matching the format of the existing AI Glossary entry: title, description, and a link. The entire process required editing **one file** and running three commands.*

---

## 整体思路——为什么要这样做
<p class="heading-note">The Big Picture — Why We Do It This Way</p>

在继续之前，先建立一个心智模型：

*Before going further, here's the mental model you need:*

```
你编辑一个文本文件（projects.md）
        ↓
Hugo 把它转成网页 HTML（真正的网站）
        ↓
Git 追踪你做了哪些改动（像 Word 的"修订追踪"）
        ↓
git push 把改动上传到 GitHub
        ↓
GitHub Actions 自动重新构建并发布网站
        ↓
全世界都能看到更新后的页面
```

*You edit a text file → Hugo turns it into a web page → Git tracks your changes → `git push` uploads to GitHub → GitHub Actions rebuilds and publishes the site → The world sees the update.*

---

## 核心概念
<p class="heading-note">Key Concepts</p>

| 术语 | 通俗解释 |
|---|---|
| **Markdown** | 一种极简的写作格式，用 `##` 代替标题、`[文字](链接)` 代替超链接 |
| **`content/projects.md`** | 你网站 Projects 页的"源头"——改这个文件，页面就变了 |
| **`git add`** | 把改动"放进打包袋"，准备提交 |
| **`git commit`** | 正式保存这个版本，配上一句说明——像给一张照片写注释 |
| **`git push`** | 把本地改动上传到 GitHub，触发网站重新发布 |
| **GitHub Actions** | 每次 push 后自动运行的机器人，负责构建和发布网站 |

| Term | Plain English |
|---|---|
| **Markdown** | A simple writing format — `##` makes a heading, `[text](link)` makes a hyperlink |
| **`content/projects.md`** | The source file for your Projects page — edit this, the page changes |
| **`git add`** | Put your changes into a "bag" ready to be committed |
| **`git commit`** | Officially save this version with a short note — like captioning a photo |
| **`git push`** | Upload your local changes to GitHub, triggering the site to republish |
| **GitHub Actions** | An automated robot that runs every time you push, building and publishing your site |

---

## 完整操作过程
<p class="heading-note">The Full Process, Step by Step</p>

### 第 1 步：找到网站文件夹
<p class="heading-note">Step 1: Locate the Site Folder</p>

网站的所有文件都存在本地一个文件夹里。我们的在：

*All your website files live in a local folder. Ours is at:*

```
/Users/Damon/Projects/damonmiaom.github.io
```

这个文件夹就是整个网站的"源码"。你在这里改动内容，再推送到 GitHub，网站就会更新。

*This folder is the "source" of your entire website. Edit content here, push to GitHub, and the site updates.*

---

### 第 2 步：找到 Projects 页面的源文件
<p class="heading-note">Step 2: Find the Projects Page Source File</p>

Hugo 网站的内容文件都放在 `content/` 目录下。Projects 页面对应的文件是：

*Hugo stores all page content in the `content/` folder. The Projects page lives at:*

```
content/projects.md
```

打开这个文件，可以看到 AI Glossary 项目已经在里面了：

*Opening this file, we could see the AI Glossary entry already there:*

```markdown
---
title: "Projects"
---

## AI Glossary

A living reference of AI and machine learning terminology, built for clarity over comprehensiveness.

[View AI Glossary →](https://damonmiaom.github.io/AI-Glossary/)
```

这就是整个 Projects 页面的全部内容。没有复杂的 HTML，只有简单的 Markdown 文本。

*This is the entire Projects page. No complex HTML — just plain Markdown text.*

---

### 第 3 步：读懂 Markdown 格式
<p class="heading-note">Step 3: Understanding the Markdown Format</p>

每个项目的格式是固定的三段式：

*Each project follows the same three-part structure:*

```markdown
## 项目名称

项目的一句话简介。

[跳转链接文字 →](项目网址)
```

- `##` = 二级标题，在网页上显示为较大的加粗字体
- 下面一行 = 项目简介
- `[文字](网址)` = 超链接，方括号里是显示文字，圆括号里是真实网址

*`##` = a second-level heading (rendered as large bold text). The next line is the description. `[text](url)` is a hyperlink — square brackets hold the display text, parentheses hold the actual URL.*

---

### 第 4 步：添加新项目
<p class="heading-note">Step 4: Add the New Project Entry</p>

按照同样的格式，在文件末尾加上 Word to PPT Transformer：

*Following the same format, we added the Word to PPT Transformer at the end of the file:*

```markdown
## Word to PPT Transformer

A web tool that converts Word exam papers into PowerPoint presentations,
making it easy to repurpose educational content for classroom slides.

[View Word to PPT Transformer →](https://damonmiaom.github.io/Word_To_PPT/)
```

改动后，完整的 `projects.md` 文件变成这样：

*After the edit, the complete `projects.md` file looks like this:*

```markdown
---
title: "Projects"
---

## AI Glossary

A living reference of AI and machine learning terminology, built for clarity over comprehensiveness.

[View AI Glossary →](https://damonmiaom.github.io/AI-Glossary/)

## Word to PPT Transformer

A web tool that converts Word exam papers into PowerPoint presentations,
making it easy to repurpose educational content for classroom slides.

[View Word to PPT Transformer →](https://damonmiaom.github.io/Word_To_PPT/)
```

一个项目页面，就是这么简单。

*A project page is really that simple.*

---

### 第 5 步：确认改动——`git status`
<p class="heading-note">Step 5: Check the Changes — `git status`</p>

在把改动推送出去之前，先确认 Git 知道我们改了什么：

*Before pushing, we confirmed Git was aware of our change:*

```bash
cd /Users/Damon/Projects/damonmiaom.github.io
git status
```

输出结果：

*Output:*

```
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  modified:   content/projects.md
```

**解读：** `modified: content/projects.md` 告诉我们，Git 检测到这个文件被改动了，但还没有被"打包"准备提交。

*Reading the output: `modified: content/projects.md` tells us Git detected the change, but it hasn't been staged (packaged) for commit yet.*

---

### 第 6 步：暂存改动——`git add`
<p class="heading-note">Step 6: Stage the Change — `git add`</p>

```bash
git add content/projects.md
```

这条命令的意思是："把 `content/projects.md` 这个文件的改动，放进准备提交的打包袋里。"

*This command says: "Put the changes in `content/projects.md` into the bag of things I'm about to commit."*

**为什么不直接用 `git add .`（添加所有文件）？**

因为 `.` 会把当前目录下的**所有**改动都打包，包括你可能不想提交的临时文件、个人配置等。指定具体文件名是更安全、更清晰的做法。

*Why not just `git add .` (add everything)? Because `.` would stage **all** changes in the folder — including temporary files or personal configs you might not want to publish. Naming the specific file is safer and cleaner.*

---

### 第 7 步：提交改动——`git commit`
<p class="heading-note">Step 7: Commit the Change — `git commit`</p>

```bash
git commit -m "Add Word to PPT Transformer to Projects page"
```

`-m` 后面跟的是这次提交的**说明信息**（commit message）——简短描述你做了什么。

*The `-m` flag is followed by a **commit message** — a short description of what you changed.*

输出结果：

*Output:*

```
[main 75868e7] Add Word to PPT Transformer to Projects page
 1 file changed, 6 insertions(+)
```

**解读：**
- `main` = 当前所在分支
- `75868e7` = 这次提交的唯一 ID（哈希值）——Git 自动生成
- `1 file changed, 6 insertions(+)` = 改动了 1 个文件，新增了 6 行内容

*Reading the output: `main` is the current branch. `75868e7` is the unique ID (hash) Git auto-generated for this commit. `1 file changed, 6 insertions(+)` confirms exactly what changed.*

**类比：** `git commit` 就像在 Word 里点击"另存为新版本"，并给这个版本写一个备注。

*Analogy: `git commit` is like "Save as new version" in Word — with a label describing what changed.*

---

### 第 8 步：推送到 GitHub——`git push`
<p class="heading-note">Step 8: Push to GitHub — `git push`</p>

```bash
git push origin main
```

**逐词解释：**

*Word by word:*

- `git push` = 把本地的提交上传到远程服务器
- `origin` = 远程服务器的名字（默认叫 `origin`，指向 GitHub 上的仓库）
- `main` = 推送到哪个分支（`main` 是主分支，也是网站的来源分支）

输出结果：

*Output:*

```
To https://github.com/DamonMiaoM/damonmiaom.github.io.git
   4c93d88..75868e7  main -> main
```

**解读：** 这一行告诉我们，GitHub 上的 `main` 分支已经从提交 `4c93d88` 更新到了 `75868e7`——正是我们刚才的那次提交。

*Reading the output: This line confirms GitHub's `main` branch was updated from commit `4c93d88` to `75868e7` — our new commit.*

---

### 第 9 步：等待自动发布
<p class="heading-note">Step 9: Wait for Automatic Publishing</p>

`git push` 完成后，我们什么都不用再做了。

*After `git push` completes, nothing else is needed.*

GitHub 收到推送后，会自动触发一个叫 **GitHub Actions** 的流程：

*GitHub receives the push and automatically triggers a process called **GitHub Actions**:*

```
git push 完成
    ↓
GitHub 收到新提交
    ↓
GitHub Actions 工作流启动
    ↓
Hugo 重新构建整个网站（~30秒）
    ↓
新网站文件自动发布到 GitHub Pages
    ↓
约 1 分钟后，damonmiaom.github.io 显示最新内容
```

你可以在 GitHub 仓库的 **Actions** 标签页看到这个过程的实时状态。绿色 ✅ 表示成功，红色 ❌ 表示出了问题。

*You can watch this process in real time under the **Actions** tab of your GitHub repository. Green ✅ means success; red ❌ means something went wrong.*

---

## 三条命令的完整总结
<p class="heading-note">The Three Commands — A Complete Summary</p>

```bash
# 第一步：把改动放进"打包袋"
git add content/projects.md

# 第二步：正式保存这个版本，并写上说明
git commit -m "Add Word to PPT Transformer to Projects page"

# 第三步：上传到 GitHub，触发网站自动更新
git push origin main
```

这三条命令是你在这个网站上发布任何内容都会用到的核心流程。记住它们。

*These three commands are the core workflow for publishing anything on this site. Memorize them.*

---

## 新手常见问题
<p class="heading-note">Common Beginner Questions</p>

**Q：为什么不能直接在 GitHub 网页上编辑？**

可以。但在本地编辑再推送，你可以先在自己电脑上预览效果，也更容易养成好习惯。

*A: You can. But editing locally and pushing lets you preview before publishing, and builds better habits.*

---

**Q：commit message 重要吗？**

非常重要。几个月后回头看历史记录时，"Add Word to PPT Transformer to Projects page" 比 "update" 有意义得多。写清楚"做了什么"，对未来的自己是一种善意。

*A: Very. "Add Word to PPT Transformer to Projects page" is far more useful than "update" when you look back months later. Be kind to your future self.*

---

**Q：推送后多久能看到更新？**

通常 30 秒到 1 分钟。如果等了 3 分钟还没更新，去 GitHub Actions 标签页检查是否有错误。

*A: Usually 30 seconds to 1 minute. If it hasn't updated after 3 minutes, check the GitHub Actions tab for errors.*

---

**Q：如果写错了怎么办？**

直接改文件，再走一遍 `git add` → `git commit` → `git push` 流程，新版本会覆盖旧版本。

*A: Just edit the file again and run the same three commands. The new version will overwrite the old one.*

---

## 整个流程的本质
<p class="heading-note">The Essence of This Workflow</p>

你刚刚做的事情，和全世界所有软件工程师每天在做的事情，在本质上是一样的：

*What you just did is, in essence, the same thing every software engineer in the world does every day:*

1. **修改文件** — 做出改变 / **Edit a file** — make a change
2. **`git add`** — 确认要提交什么 / **`git add`** — decide what to commit
3. **`git commit`** — 保存版本快照 / **`git commit`** — save a snapshot
4. **`git push`** — 发布到世界 / **`git push`** — publish to the world

规模可以从个人网站扩展到几百人的工程团队——但这个核心循环永远不变。

*The scale might range from a personal website to a team of hundreds — but this core loop never changes.*

---

*"每一个大项目，都是从改动一个文件开始的。"*

*"Every big project starts with editing one file."*

<p class="post-revised">Published: Mar 26, 2026, 16:00</p>
