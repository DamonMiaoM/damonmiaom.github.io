---
title: "10 Most Powerful Claude Code Commands — And Why You Should Use Them"
date: 2026-03-27T20:00:00
author: Damon Miao
description: "A life insurance agent's guide to the 10 most powerful Claude Code commands — with bilingual explanations, real use cases, and a call to action for anyone ready to build with AI."
tags: [claude-code, AI, commands, productivity, beginners, bilingual]
slug: top-10-claude-code-commands
---

# 10 Most Powerful Claude Code Commands — And Why You Should Use Them
# 10 个最强大的 Claude Code 命令 — 以及你为什么应该使用它们

*Written by a life insurance agent who learned these the hard way — one terminal session at a time.*
*由一位寿险代理人撰写，这些命令都是一个终端会话一个终端会话学来的。*

---

I love Claude.

After 23 sessions, 281 messages, and 49 hours inside Claude Code — building websites, converting documents, writing bilingual scripts, and rushing to finish insurance comparison decks before catching a train — I've learned which commands actually matter. These are the 10 I reach for most, and the ones that, once you understand them, change how you think about working with AI.

在 23 次会话、281 条消息、49 小时的 Claude Code 使用后 — 我建了网站、转换了文档、写了双语脚本，甚至赶在赶火车前生成了保险对比材料 — 我学会了哪些命令真正有用。这是我最常用的 10 个，一旦理解了它们，你看待 AI 协作的方式就会改变。

---

## 1. `claude`
### The Gateway / 一切的起点

**What it does:** Launches Claude Code in interactive mode — your AI partner is ready.

**用途：** 启动 Claude Code 的交互模式 — 你的 AI 伙伴准备就绪。

```bash
claude
```

**When to use it:** Every single time. This is the front door. Whether you want to write code, draft an email, analyze a document, or just think out loud — `claude` opens the conversation.

**何时使用：** 每一次。这是大门。无论你想写代码、起草邮件、分析文档，还是只是想整理思路 — `claude` 开启对话。

**Why it's powerful:** Claude Code isn't just a chatbot — it's an agent. Once you type `claude`, it can read your files, run shell commands, search the web, and write code autonomously. That single word unlocks a capable collaborator.

**为什么强大：** Claude Code 不只是聊天机器人 — 它是一个代理。一旦你输入 `claude`，它就能读取你的文件、运行终端命令、搜索网页、自主编写代码。那一个词解锁了一个有能力的合作者。

**Result you'll see:**
```
✓ Claude Code initialized
> What would you like to work on?
```

---

## 2. `claude --resume` / `claude -r`
### Your Session Library / 你的会话图书馆

**What it does:** Opens an interactive list of all your past conversations so you can pick up exactly where you left off.

**用途：** 打开所有历史会话的交互式列表，让你从上次离开的地方继续。

```bash
claude -r
```

**When to use it:** When you were mid-task and got interrupted. When you want to reference something you discussed yesterday. When you've been building something across multiple sessions.

**何时使用：** 当你在做任务中途被打断时。当你想参考昨天讨论的内容时。当你在多个会话中持续构建某个项目时。

**Why it's powerful:** Most people treat each AI conversation as disposable. `--resume` changes that — it turns Claude Code into a project partner with memory, continuity, and context. Your whole working history is accessible.

**为什么强大：** 大多数人把每次 AI 对话都当成一次性的。`--resume` 改变了这一点 — 它把 Claude Code 变成了一个有记忆、有连续性、有上下文的项目伙伴。你所有的工作历史都可以访问。

**Result you'll see:**
```
? Select a conversation to resume:
  ▸ [2026-03-27] Building Hugo blog post (23 msgs)
    [2026-03-26] Insurance comparison Excel + PPT (41 msgs)
    [2026-03-25] Word-to-PPT converter fix (18 msgs)
```

---

## 3. `claude --continue` / `claude -c`
### Instant Resume / 即刻续接

**What it does:** Skips the selection menu and immediately resumes your most recent conversation.

**用途：** 跳过选择菜单，立即续接你最近的一次对话。

```bash
claude -c
```

**When to use it:** You closed the terminal five minutes ago and now remember something you wanted to add. You want to continue today's session without hunting for it.

**何时使用：** 你五分钟前关闭了终端，现在想起了还有东西要补充。你想继续今天的会话，不想在列表里翻找。

**Why it's powerful:** Frictionless continuation. One flag, and you're back in context — Claude remembers everything from your last session. No repetition, no re-explanation needed.

**为什么强大：** 无摩擦续接。一个标志，你就回到了上下文中 — Claude 记得你上次会话的一切。不需要重复，不需要重新解释。

**Result you'll see:**
```
Resuming last conversation from 14 minutes ago...
[Previous context loaded]
>
```

---

## 4. `claude -p "your prompt here"`
### Headless Power Mode / 无头强力模式

**What it does:** Runs Claude non-interactively — you give it a task via command line and it returns the output directly, no conversation needed.

**用途：** 非交互式运行 Claude — 通过命令行给它一个任务，它直接返回输出，不需要对话。

```bash
claude -p "Summarize all .md files in ./notes/ into a weekly report" --allowedTools "Read,Write,Glob"
```

**When to use it:** Automating repetitive tasks. Running Claude as part of a script or pipeline. Processing files in batch without sitting there and typing.

**何时使用：** 自动化重复性任务。将 Claude 作为脚本或流水线的一部分。批量处理文件，不需要坐在那里打字。

**Why it's powerful:** This is Claude as a command-line utility. You can chain it with other tools, schedule it as a cron job, or use it to pre-process data before a meeting. For anyone doing repetitive document work — this is transformative.

**为什么强大：** 这是 Claude 作为命令行工具的使用方式。你可以把它与其他工具链接，作为定时任务调度，或者在开会前用它预处理数据。对于任何做重复性文档工作的人 — 这是颠覆性的。

**Result you'll see:**
```
# Weekly Summary Report
## Key themes across 12 notes:
1. Client follow-up patterns...
2. Product comparison notes...
[Full report written to summary.md]
```

---

## 5. `/clear`
### Fresh Start / 重新开始

**What it does:** Clears the current conversation context — wipes the slate clean without ending the session.

**用途：** 清除当前对话的上下文 — 擦干净黑板，但不结束会话。

```bash
/clear
```

**When to use it:** You've been working on one task and want to switch to something completely unrelated. The conversation has drifted and Claude is getting confused. You want to start a clean sub-task without opening a new terminal.

**何时使用：** 你一直在做一个任务，想切换到完全不相关的事情。对话已经偏离了，Claude 开始混淆。你想在不打开新终端的情况下开始一个干净的子任务。

**Why it's powerful:** Context is memory — and a cluttered context leads to confused answers. `/clear` lets you reset Claude's working memory while keeping the session alive. Think of it as clearing your desk before starting a different project.

**为什么强大：** 上下文就是记忆 — 杂乱的上下文会导致混乱的答案。`/clear` 让你在保持会话活跃的同时重置 Claude 的工作记忆。把它想象成在开始不同项目前清理你的桌面。

**Result you'll see:**
```
✓ Context cleared. Ready for a new task.
>
```

---

## 6. `/compact`
### Intelligent Compression / 智能压缩

**What it does:** Summarizes the conversation history into a compact form to free up context space, while preserving the essential information Claude needs.

**用途：** 将对话历史总结成紧凑形式，释放上下文空间，同时保留 Claude 所需的核心信息。

```bash
/compact
# or with custom instructions:
/compact Focus on the Hugo website changes we made
```

**When to use it:** Long sessions where you've gone deep on a topic. When Claude warns that the context is getting full. When you want to keep working but the conversation has grown unwieldy.

**何时使用：** 深入某个主题的长会话。当 Claude 警告上下文快满了时。当你想继续工作但对话已经变得难以管理时。

**Why it's powerful:** Unlike `/clear` which forgets everything, `/compact` is surgical — it distills the conversation into the key facts and decisions, so Claude keeps working intelligently without losing the thread.

**为什么强大：** 不同于忘记一切的 `/clear`，`/compact` 是精准的 — 它把对话提炼成关键事实和决定，让 Claude 在不失去线索的情况下继续智能工作。

**Result you'll see:**
```
✓ Conversation compacted. Context reduced by 73%.
Key facts retained: Hugo site structure, CSS changes, git workflow
>
```

---

## 7. `/init`
### Project Foundation / 项目基础

**What it does:** Analyzes your current project directory and generates a `CLAUDE.md` file — a persistent instruction file that Claude reads at the start of every session.

**用途：** 分析你当前的项目目录并生成 `CLAUDE.md` 文件 — 一个 Claude 在每次会话开始时读取的持久指令文件。

```bash
/init
```

**When to use it:** When starting any new project with Claude. When you want Claude to always know your tech stack, preferences, and rules without you repeating them every time.

**何时使用：** 用 Claude 开始任何新项目时。当你希望 Claude 总是了解你的技术栈、偏好和规则，不需要每次重复时。

**Why it's powerful:** `CLAUDE.md` is your project's permanent memory. Once it's set up, Claude walks into every session already knowing your structure, your rules, and your goals. One setup saves hours of repetition across dozens of sessions.

**为什么强大：** `CLAUDE.md` 是你项目的永久记忆。一旦设置好，Claude 进入每次会话时就已经了解你的结构、规则和目标。一次设置节省了几十次会话中的几小时重复。

**Result you'll see:**
```
✓ CLAUDE.md created
Detected: Hugo static site, GitHub Pages deployment, bilingual content
Instructions written: git workflow, push after changes, preferred tone
```

---

## 8. `/memory`
### Persistent Intelligence / 持久智能

**What it does:** Manages Claude's long-term memory — facts, preferences, and patterns that persist across all sessions and all projects.

**用途：** 管理 Claude 的长期记忆 — 跨所有会话和所有项目持续存在的事实、偏好和模式。

```bash
/memory
```

**When to use it:** When you want Claude to remember who you are, how you like to work, and what matters to you — permanently, not just in one session.

**何时使用：** 当你想让 Claude 永久记住你是谁、你喜欢怎么工作、什么对你重要时 — 不只是一次会话。

**Why it's powerful:** This is what transforms Claude from a smart tool into a genuine collaborator. It can remember that you're a life insurance agent who prefers warm tone in writing, that you speak Chinese first and English second, that you're learning Python as a beginner. Every future session starts smarter.

**为什么强大：** 这是将 Claude 从聪明的工具转变为真正的合作者的关键。它可以记住你是一名喜欢温暖写作风格的寿险代理人，你中文优先英文其次，你作为初学者在学 Python。每次未来的会话都从更智能的起点开始。

**Result you'll see:**
```
Memory saved: User is a MetLife life insurance agent, prefers bilingual (Chinese/English) output, beginner in coding but experienced in finance and client communication.
```

---

## 9. `/cost`
### Stay in Control / 保持掌控

**What it does:** Shows you the token usage and estimated cost for your current session.

**用途：** 显示当前会话的 token 使用量和预估费用。

```bash
/cost
```

**When to use it:** During any long or complex session. Before you go deep on a large task. When you want to understand how much context you've consumed.

**何时使用：** 在任何长时间或复杂的会话中。在你深入大型任务之前。当你想了解已消耗多少上下文时。

**Why it's powerful:** Awareness creates efficiency. When you see the token count climbing, you know to `/compact` or `/clear` to stay lean. It also helps you understand which types of tasks are "expensive" — helping you get more from every session.

**为什么强大：** 意识创造效率。当你看到 token 计数在上升时，你知道该 `/compact` 或 `/clear` 来保持精简。它还帮助你了解哪些类型的任务是"昂贵的" — 帮助你从每次会话中获得更多价值。

**Result you'll see:**
```
Session cost: ~$0.14
Tokens used: 12,847 input / 3,201 output
Context: 67% full
```

---

## 10. `/insights`
### Your AI Mirror / 你的 AI 镜子

**What it does:** Generates a full analytics report on how you've been using Claude Code — what's working, where friction is, and what to try next.

**用途：** 生成关于你如何使用 Claude Code 的完整分析报告 — 什么有效，哪里有摩擦，接下来尝试什么。

```bash
/insights
```

**When to use it:** After a few weeks of regular use. When you feel like you're getting stuck in patterns. When you want an honest audit of how you're actually working with AI.

**何时使用：** 经过几周的定期使用后。当你感觉陷入了模式时。当你想对自己实际如何与 AI 协作进行诚实审计时。

**Why it's powerful:** Most people use tools without ever stepping back to examine how they're using them. `/insights` gives you data: session counts, friction points, satisfaction patterns, and personalized suggestions. It's like a performance review for your AI workflow.

**为什么强大：** 大多数人使用工具时从不退后一步审视自己如何使用它们。`/insights` 给你数据：会话次数、摩擦点、满意度模式和个性化建议。这就像是对你 AI 工作流的绩效评审。

**Result you'll see:**
```
23 sessions analyzed · 281 messages · 49h
Top friction: Auth failures (5 sessions lost)
Top strength: Iterative bilingual content creation
Suggestion: Create a custom /blog skill for your Hugo workflow
```

---

## Quick Reference / 快速参考

| Command | Best For | 最适合 |
|---------|----------|--------|
| `claude` | Starting any session | 开始任意会话 |
| `claude -r` | Returning to past work | 回到过去的工作 |
| `claude -c` | Continuing last session instantly | 立即续接上次会话 |
| `claude -p "..."` | Automating tasks without interaction | 无交互自动化任务 |
| `/clear` | Switching topics cleanly | 干净切换话题 |
| `/compact` | Staying lean in long sessions | 长会话中保持精简 |
| `/init` | Setting up a new project | 设置新项目 |
| `/memory` | Teaching Claude who you are | 让 Claude 了解你 |
| `/cost` | Monitoring usage | 监控使用量 |
| `/insights` | Auditing your AI workflow | 审计你的 AI 工作流 |

---

## Call to Action — Start Building
## 行动号召 — 开始构建

If you're reading this and you haven't tried Claude Code yet, I want to say this clearly: **you don't need a computer science background to start.** I'm a life insurance agent. I know CFA-level finance and Buddhist philosophy. A few months ago, I didn't know what a terminal was.

如果你正在读这篇文章，还没有尝试过 Claude Code，我想清楚地说：**你不需要计算机科学背景就能开始。** 我是一名寿险代理人。我懂 CFA 级别的金融和佛学。几个月前，我不知道终端是什么。

Today, I have a live personal website, a Word-to-PPT converter tool, a bilingual wedding script, and a growing library of insurance materials — all built with Claude Code, one command at a time.

今天，我有了一个上线的个人网站、一个 Word 转 PPT 的工具、一份双语婚礼主持词，以及一个不断增长的保险材料库 — 全部都用 Claude Code 构建的，一次一个命令。

**The 10 commands above are your foundation.** Start with `claude`, graduate to `claude -r`, and eventually reach for `claude -p` when you're ready to automate. Use `/init` on your first real project. Let `/memory` remember your name, your role, your preferences. And once in a while, run `/insights` to see how far you've come.

**上面这 10 个命令是你的基础。** 从 `claude` 开始，升级到 `claude -r`，等你准备好自动化时就用 `claude -p`。在你的第一个真实项目上用 `/init`。让 `/memory` 记住你的名字、你的角色、你的偏好。偶尔跑一次 `/insights`，看看你走了多远。

The tools are here. The capability is real. The only thing between you and an AI-powered workflow is one terminal command.

工具在这里。能力是真实的。你和 AI 驱动的工作流之间唯一隔着的，就是一个终端命令。

**Type `claude` and begin.**

**输入 `claude`，开始吧。**

---

*I love Claude. And I think you will too.*
*我爱 Claude。我相信你也会的。*

---

*Written with Claude Code | Ghostty Terminal | MacBook Pro*
*2026-03-27 · Damon Miao*
