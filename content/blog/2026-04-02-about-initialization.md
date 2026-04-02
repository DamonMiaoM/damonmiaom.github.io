---
title: "项目初始化：在开始之前，先告诉 Claude 你是谁"
date: 2026-04-02T10:00:00
author: Damon Miao
description: "用 What / Why / How 的逻辑，搞清楚 Claude Code 项目初始化到底在做什么，以及不做会发生什么。"
tags: [Claude Code, 42plugin, 学习笔记]
slug: about-initialization
---

## 什么是项目初始化？

<p class="heading-note">What Is Project Initialization?</p>

你打开一个新文件夹，准备用 Claude Code 开始一个项目。

在你敲下第一行需求之前，有一件事值得先做：**告诉 Claude，这个项目是什么，它应该怎么工作，有什么规则需要遵守。**

这件事，就叫做项目初始化。

*Before you type your first request, there's one thing worth doing: tell Claude what this project is, how it should work, and what rules to follow. That's project initialization.*

---

## 为什么需要它？

<p class="heading-note">Why Does It Matter?</p>

Claude Code 的每一次对话，都从零开始。它不会自动记得你上次说了什么，也不知道你的项目叫什么名字、用什么语言、有什么约定。

如果没有初始化，每次打开新 session，你都要重新解释背景；Claude 给出的建议可能和你已有的架构冲突；跨多次对话的工作会失去连续性。

**做了初始化，Claude 一开口就知道自己在哪里，在做什么。**

*Claude Code starts fresh every session. Without initialization, you repeat yourself every time, suggestions may conflict with your existing architecture, and work loses continuity across sessions. With initialization, Claude knows the context from the very first message.*

---

## 怎么做？

<p class="heading-note">How To Do It</p>

### 方法一：Claude Code 内建的 `/init`

Claude Code 自带一个 `/init` 命令。它会**扫描你已有的代码库**，自动分析结构，然后生成一个 `CLAUDE.md` 文件。

适合：项目已经存在，想让 Claude 理解现有代码。

*Claude Code has a built-in `/init` command. It scans your existing codebase, analyzes the structure, and generates a `CLAUDE.md` file. Best for: projects that already exist.*

### 方法二：42plugin 的 `cc101-init` Skill

`cc101-init` 是 42plugin 提供的自定义 Skill，适合**从零开始的新项目**。它不分析代码，而是问你两个问题——项目名称和类型——然后一次性创建完整的项目脚手架。

*`cc101-init` is a custom skill from 42plugin, designed for brand-new projects. It asks two questions — name and type — then creates a complete project scaffold in one step.*

| | Claude Code `/init` | 42plugin `cc101-init` |
|--|--|--|
| 适合场景 | 已有代码库 | 全新项目 |
| 工作方式 | 分析现有代码 | 询问后生成 |
| 创建文件 | CLAUDE.md | CLAUDE.md + README + .gitignore + .claudeignore + 目录结构 |

---

## 初始化创建了哪些文件？它们为什么存在？

<p class="heading-note">What Files Are Created, and Why?</p>

### CLAUDE.md — Claude 的岗位说明书

这是最重要的文件。Claude Code 在每次 session 开始时会自动读取它，就像员工上班前看一眼今天的工作要求。

你可以在里面写：项目是什么、用什么技术、有什么规则、什么事情不能做。

**不创建它会怎样？** Claude 对你的项目一无所知，每次对话都像第一次见面。

*CLAUDE.md is Claude's briefing document — read automatically at the start of every session. Without it, Claude knows nothing about your project. Every session starts from zero.*

---

### README.md — 写给人看的项目说明

README 是给**人**看的——包括未来的你。当你三个月后回来看这个项目，README 告诉你："这个项目是什么，当时为什么建它，怎么用它。"

**不创建它会怎样？** 三个月后你打开这个文件夹，完全不记得它是干什么的。

*README.md is for humans — including your future self. Without it, you'll open the folder three months later and have no idea what it was for.*

---

### .gitignore — 告诉 Git 什么不需要记录

Git 默认会追踪文件夹里的所有变化。但有些文件不应该被追踪：比如包含密码的 `.env` 文件，或者系统自动生成的 `.DS_Store`。

`.gitignore` 就是一份"排除名单"，列出哪些文件不需要 Git 管理。

**不创建它会怎样？** 你可能不小心把 API Key 提交到 GitHub，任何人都能看到。这是真实发生过的安全事故。

*`.gitignore` is an exclusion list — it tells Git what NOT to track. Without it, you might accidentally commit your API keys to GitHub. This is a real security risk that has happened to many developers.*

---

### .claudeignore — 告诉 Claude 什么不需要读

和 `.gitignore` 类似，`.claudeignore` 告诉 Claude 哪些文件不需要读取。这样 Claude 不会把注意力浪费在无关文件上，也避免它读到不该读的内容。

**不创建它会怎样？** Claude 可能把时间花在读取大量无关文件上，降低效率。

*`.claudeignore` tells Claude what files to skip. Without it, Claude may waste context reading irrelevant files — reducing efficiency and focus.*

---

## 一句话总结

<p class="heading-note">One-Line Summary</p>

初始化不是"程序员才需要做的仪式"，而是让 Claude 真正能帮上你忙的前提条件。

**五分钟的初始化，换来每次对话都不用重新解释。**

*Initialization isn't a developer ritual. It's the prerequisite for Claude to actually help you. Five minutes of setup saves you from explaining context every single session.*
