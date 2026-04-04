---
title: "Claude Code 学习入门指南"
date: 2026-04-04T10:00:00
author: Damon Miao
description: "从零开始系统学习 Claude Code：核心概念、安装配置、扩展能力、自动化调度，以及一条实用的四阶段学习路线。"
tags: [Claude Code, 跟着CC学CC, 学习指南, 工具探索, AI辅助开发]
series: 跟着CC学CC
slug: claude-code-study-guide
---

## 这篇文章讲什么？

<p class="heading-note">What This Guide Covers</p>

Claude Code 是 Anthropic 出的 AI 编程助手，直接跑在终端里，能帮你读代码、改 bug、重构、写测试、发 PR。GitHub 上 109k Star，算是目前最火的 AI 编程工具之一。

这篇文章帮你从零理清 Claude Code 是什么、怎么装、核心功能有哪些、能扩展到什么程度，最后给你一条四阶段学习路线。

*Claude Code is Anthropic's agentic coding CLI — it reads your codebase, fixes bugs, refactors, writes tests, and prepares PRs. With 109k GitHub stars, it's one of the most popular AI coding tools. This guide covers what it is, how to install it, core features, extensibility, and a 4-phase learning roadmap.*

---

## 学科定位

<p class="heading-note">Where Does Claude Code Fit?</p>

- **一级学科：** 工学
- **二级学科：** AI 辅助开发工具 (AI-Assisted Development)
- **核心问题：**
  1. 如何用 AI 代理自动化开发工作流？
  2. 如何通过自然语言指令写代码、调 bug、重构？
  3. 如何把 AI 编码工具接到现有开发流程里？

---

## 官方文档与核心资源

<p class="heading-note">Official Docs & Key Resources</p>

| 资源 | 类型 | 链接 |
|------|------|------|
| Official Documentation (85篇) | 官方文档 | [code.claude.com/docs/en/](https://code.claude.com/docs/en/) |
| Overview | 入门 | [产品概述、安装、平台支持](https://code.claude.com/docs/en/overview) |
| How Claude Code Works | 架构 | [Agentic 循环、工具分类、模型选择](https://code.claude.com/docs/en/how-claude-code-works) |
| Best Practices | 实践 | [验证优先、上下文管理](https://code.claude.com/docs/en/best-practices) |
| Common Workflows | 实战 | [理解代码库、调试、重构](https://code.claude.com/docs/en/common-workflows) |
| GitHub Repo | 源码 | [github.com/anthropics/claude-code](https://github.com/anthropics/claude-code) |

官方文档按主题分了 8 大类：

| 分类 | 关键主题 |
|------|---------|
| Getting Started | 安装、Quickstart、概念入门 |
| Core Features | CLAUDE.md、自动补全、检查点、Plan Mode |
| Configuration | 权限、Hooks、设置、模型配置 |
| Platforms & Integrations | VS Code、JetBrains、Slack、CI/CD |
| Extensibility | MCP、Skills、Subagents、Plugins |
| Automation & Scheduling | Cloud Tasks、Desktop Tasks、/loop |
| Enterprise | SSO、SCIM、Bedrock、Vertex、Foundry |
| Security & Legal | 安全策略、数据隐私 |

---

## 核心概念与架构

<p class="heading-note">Core Concepts & Architecture</p>

### 工作原理：Agentic Loop

Claude Code 的核心是**代理循环**——不断重复"收集上下文 → 执行操作 → 验证结果"：

```
Gather Context → Take Action → Verify Results → Repeat
```

不是一次性生成代码就完事，而是像一个真的开发者一样：先看、再改、再验证。

### 内置工具（5 大类）

| 类别 | 工具 | 干什么的 |
|------|------|---------|
| 文件操作 | Read, Write, Edit | 读、创建、改文件 |
| 搜索 | Glob, Grep | 按文件名找、按内容搜 |
| 执行 | Bash | 跑终端命令 |
| 网络 | WebSearch, WebFetch | 搜网页、抓页面内容 |
| 代码智能 | 上下文推断 | 理解代码结构和关系 |

### 支持的模型

- **Claude Sonnet** — 默认，快且够用
- **Claude Opus** — 最强，适合复杂任务
- 也支持通过 Amazon Bedrock、Google Vertex AI 接入自定义模型

### 权限模式

| 模式 | 说明 |
|------|------|
| Default | 每次改文件都要确认 |
| Auto-accept edits | 自动接受文件编辑 |
| Plan Mode | 只读，只做计划不执行 |
| Auto Mode | 自动选合适的模式 |

---

## 安装与平台支持

<p class="heading-note">Installation & Platform Support</p>

| 平台 | 安装命令 |
|------|---------|
| macOS / Linux | `curl -fsSL https://claude.ai/install.sh \| bash` |
| Windows | `winget install Anthropic.ClaudeCode` |
| Homebrew | `brew install claude-code` |

支持 5 种使用方式：

| 平台 | 特点 |
|------|------|
| Terminal (CLI) | 原始体验，功能最全 |
| VS Code | 侧边栏集成，内联 diff 预览 |
| JetBrains | IDE 深度集成 |
| Desktop App | 独立应用 |
| Web App | 浏览器直接用 |

**常见用法：**
- 理解不熟悉的新代码库
- 描述问题，AI 自动诊断修复
- 自然语言指令重构代码
- 自动生成单元测试和集成测试
- 自动准备 Pull Request
- 为代码库生成文档

---

## 扩展能力

<p class="heading-note">Extensibility</p>

### CLAUDE.md — 项目记忆文件

在项目根目录放一个 `CLAUDE.md`，Claude Code 每次启动都会读：

```
# Project Context
- This is a Next.js app with TypeScript
- Use pnpm as the package manager
- Follow the existing code style (ESLint + Prettier)
```

相当于给 AI 一份"项目说明书"。

### MCP — 连接外部工具

Model Context Protocol，让 Claude Code 能访问文件系统、数据库、API、浏览器、Figma、Linear、Jira 等外部服务。

### Skills — 自定义技能

放在 `.claude/skills/` 目录，定义可复用的工作流，通过 `/skill-name` 快速调用。

### Hooks — 事件钩子

在特定事件触发时自动执行操作，比如 pre-commit 检查、自动格式化、跑测试。

### Subagents — 子代理

用 Git Worktrees 隔离，并行执行多个独立任务。适合大型重构或多文件修改。

### Plugins — 插件

社区插件市场，扩展 Claude Code 功能。

---

## 自动化与调度

<p class="heading-note">Automation & Scheduling</p>

| 方式 | 平台 | 说明 |
|------|------|------|
| Cloud Tasks | Web | 云端异步执行 |
| Desktop Tasks | 桌面应用 | 本地定时任务 |
| /loop | CLI | 循环迭代执行 |
| Cron Jobs | CLI | 定时调度任务 |

也支持 GitHub Actions 集成，可以在 CI/CD 流程里用 AI 辅助代码审查。

---

## 企业功能

<p class="heading-note">Enterprise Features</p>

- **SSO** — 企业单点登录
- **SCIM** — 自动用户配置
- **审计日志** — 使用情况追踪
- **团队管理** — 权限控制

第三方模型支持：Amazon Bedrock (AWS)、Google Vertex AI (GCP)、Microsoft Azure Foundry。

---

## 学习路线建议

<p class="heading-note">Suggested Learning Roadmap</p>

**第一阶段：基础入门（1-2 天）**
- 装好 Claude Code，选一个你习惯的平台
- 读 Overview 和 Quickstart 文档
- 在一个小项目上试基本操作：读文件、改代码、跑命令

**第二阶段：核心功能（3-5 天）**
- 给项目写一个 CLAUDE.md
- 学会 Plan Mode，让 AI 先规划再执行
- 练习常见工作流：理解代码库、修 bug、重构

**第三阶段：扩展能力（1-2 周）**
- 配置 MCP 服务器，连上你常用的外部工具
- 创建自定义 Skills，自动化重复工作
- 用 Hooks 实现开发流程自动化
- 试 Subagents 并行处理复杂任务

**第四阶段：高级应用（持续）**
- 接入 CI/CD 流程
- 探索企业功能
- 用调度功能自动化日常任务
- 关注官方更新，跟进新功能

---

*Written with Claude Code | 2026-04-04*
