---
title: "实在受不了 Agent 间没完没了的复制粘贴，所以我让它们自己写信"
date: 2026-06-07T20:00:00
author: Damon Miao
description: "从手动 copy+paste 到 inbox/outbox 文件系统通信架构——一个保险经纪人摸索多 Agent 协作的实践记录。"
tags: [Claude Code, Multi-Agent, Hermes, AI工作流, 实践]
slug: multi-agent-inbox-outbox
---

自从学会使用 Claude Code 和 Hermes，terminals 和 sessions 并行就成了工作常态。

那会儿读一些技术文章，碰上 orchestrator 还不知道咋回事。很快，实操就让我感受到了这个词儿的内涵。

我给架在腾讯云上的 Hermes 取名叫 Warren，本地的 Hermes agent 叫 Charlie。Claude Code（Sonnet 4.6），取名叫 Peter。这个架构的原因其实很简单，作为 Claude Pro 的用户，那点儿 usage quota 必须物尽其用，好钢用在刀刃儿上。Peter 是 manager，Charlie 是 chief executor，Warren 是我在户外时的贴身 assistant。

不过，我很快意识到 Peter 在一些问题上的看法不够成熟，且跟 Charlie 之间的能力没有明显的差别（除了识图）。加上 Opus 4.8 上线，我毫不犹豫，给加载了新模型的 Claude Code 取名 Marcus Aurelius。

而后，接触到 OpenCode，这一阵子可以免费接入 DeepSeek-v4-flash。

这个团队的架构：

```
Damon（Orchestrator）
  ├── Charlie  ← Hermes（Mac）· 执行
  ├── Peter    ← Claude Code / Sonnet · 管理
  ├── Marcus   ← Claude Code / Opus · 架构
  ├── Warren   ← Hermes（Server）· 收集
  └── Allen    ← OpenCode / DeepSeek · 研究
```

Hermes 我用的是 MiMo-v2.5-pro；DeepSeek 的 API 接入到了 OpenCode。

## 人肉总线

随之而来的问题是，原来在两个 agents 之间，把它们完成任务后的 output 手动 copy + paste 的办法，实在太费劲了。两个 Agent 之间只有一条通道，四个 Agent 就变成了六条。

Agent 数量上去之后，我的工作不是变少了，是变碎了。

Charlie 跑完一个任务，产出个 md 文件，我把它拖到 Peter 的对话框里。Peter 跑完，又拖回来。每个 handoff 只要几秒钟，但我一天要 handoff 十几次。

几秒钟的事情累积久了，就不是时间的问题了——是思路总被打断。我正在想下一个任务给谁，先得把上一个任务的产出搬过来。我成了系统里最慢的那个环节。

更糟糕的是，Agent 数量从 2 个变 3 个、变 4 个、变 5 个之后，我的大脑成了唯一的上下文总线。所有人都把输出发给我，我再决定发给谁。这个架构有一个好听的名字，叫 orchestrator。但说实话，它有一个不太好听的现实——**你设计了一个 multi-agent 架构，但它的上下文同步层是手工的、由你一个人承担的。**

于是，一个纯天然的问题自然而然地冒出来：

> 如何让这些不同 terminals 里的 agents 可以彼此通信，互通有无？

省得我来回「喂饭」。

## 灵光一闪

我的搜索路径并不是很精准。首先找到的解决方案是 Coze 3.0（在花叔公众号上看到的），恰好是我最迫切想解决这个问题的时候。

苦在 Coze 3.0 要订阅；好在我忍住没有剁手——我觉察了，我管住了，阿弥陀佛。

Coze 提供的解决方案是在 GUI 里；我已经习惯了在 terminal 里用 Claude Code，这个方案并不能缓解我的痛点，反而会加重荷包的负担。

这条路不行。我之前也搜索过解决方案，不是导到花叔那边去了么，那条路没走对，问题出在搜索的方式上。后来通过 WorkBuddy，交代清楚 context，很快锁定了强相关的 references。这正好成了 AI 时代搜索的一个好案例——你得先把背景说清楚，搜索引擎才能帮你找到对的东西。

某次，Charlie 完成了它的 task 之后，我让它写了一个 md file，然后让它把该文件的保存路径给我，我直接让 Peter 自己去读。

就是如此简单的一步，我就已经彻底告别 copy and paste。

由此，我就想到了 inbox 体系。想象一个 agents 们居住的小区，每个人都有自己的邮箱，Charlie 干完了啥，直接往 Peter 的 inbox 写一份 handoff 文档不就好了吗？

于是 Charlie、Peter、Warren 和 Marcus 就各自有了自己的邮箱。

一句题外话，OpenCode 这个 agent 是后加入的，刚开始我给他取名 Ben。不过很快我发现它干活儿不利索，总出错——我居然会像老板一样，面对下属发火。果断 /rename this agent as Allen。

在 agents 交互系统有了雏形之后，我跟 Marcus 还有 Peter 又做了几轮对话，把这个项目的文件夹体系搭建了起来。我还想到了 bulletin，common info 让 agents 们自己去上面读。

```
channel/
├── AGENTS.md          ← 总纲：Agent 名册 + 通信规范
├── bulletin/          ← 全员公告（任何人发，所有人看）
├── allen/             ← Allen（OpenCode / DeepSeek v4）
│   ├── inbox/         ← 收件箱
│   ├── outbox/        ← 发件箱
│   └── archive/       ← 归档
├── charlie/           ← Charlie（Hermes / Mac 本地执行）
├── marcus/            ← Marcus（Claude Code / Opus）
├── peter/             ← Peter（Claude Code / Sonnet）
└── warren/            ← Warren（Hermes / Server）
```

通信机制：

- 点对点：A 的 outbox/ → B 的 inbox/（handoff 格式：日期-from-to-主题.md）
- 广播：任何人 → bulletin/（全员可见）
- 归档：inbox/ 处理完 → archive/（保留历史）

总之，这个小机制部署好之后，我彻底告别了手动 copy + paste，从「人肉搬运」变成了「人肉路由」。不好听归不好听，但工作体验确实优化了很多。

## 意外的共鸣

最近更新了 WorkBuddy 之后，发现它上线了「专家团」的新功能。今天我再 login 的时候，突然想到：WorkBuddy 是怎么解决专家团里不同 agents 的沟通的？

于是它自己给我解释了一番。我看它回答得十分专业，便让它看看我在本地的架构。另外，我又想到最开始搜索 reference 的时候没抓对 keywords，所以通过 WorkBuddy 重新描述了 channel 架构的背景，请它帮我搜相关性更强的参考文献。

然后我发现——WorkBuddy 的专家团本质上也是在做一个「消息路由」：团长拆解任务，分发给团员，团员把结果写回团长的收件箱。跟我做的 inbox/outbox 是同一件事，只是它跑在 GUI 里，我跑在文件系统里。

我顺便问了它：如果换做你，会怎么解决 agents 之间的通信问题？

WorkBuddy 给了我四个方向：

**方向 1：共享状态文件**——每个 Agent 工作结束时把关键状态写入约定格式的文件，其他 Agent 启动时先读。零依赖，今天就能跑。

**方向 2：MCP 作为沟通总线**——跑一个轻量 MCP server 做上下文路由。比文件更结构化，支持实时推送，但有学习成本。

**方向 3：Git 作为隐式状态总线**——Agent 把产出物 commit 到约定分支，下一个 Agent 通过 git log / git diff 感知变化。零额外基础设施，但不适合实时场景。

**方向 4：Terminal multiplexer + 命名管道**——用 tmux + named pipe 做 Agent 间通信。极轻量，但消息风暴时不好管理。

WorkBuddy 的建议：先走方向 1，等觉得不够用了，再上方向 2。

这话让我愣了一下——我凭直觉摸索出的 channel/ 方案，本质上就是方向 1 的一个变体，而且已经跑起来了。

WorkBuddy 还帮我搜到了一些相关的研究和开源项目，我还没来得及细读，列在这里供感兴趣的朋友参考：

> 1. The Inbox/Outbox Pattern — How AI Agents Coordinate Without Stepping on Each Other（dev.to, 2026）
> 2. MCP Agent Mail — "It's Like Gmail for Your Coding Agents"（GitHub, ~2000 stars）
> 3. CA-MCP: Enhancing MCP with Context-Aware Coordination via Shared Context Store（arXiv, 2026）
> 4. 多 Agent 架构的 6 种通信模式对比分析（CSDN, 2026）
> 5. WezTerm Chat MCP — Cross-Pane Communication Between AI CLI Agents（GitHub, 2026）
> 6. Agent Orchestration — MCP Server for Shared Memory Across Agents（GitHub, 2026）

## 摸索出来的道理

说真的，我对自己摸索出这套架构很满意。虽然它并不复杂，但自己题解的过程，会有非常大的成就感。后续我会沿着 WorkBuddy 提供的参考文献做进一步研究，把异步问题解决，实现真正的同步和对齐。

不过在这个过程里，有几件事超出了我的预期。

**零依赖是最大的依赖。** channel/ 就是文件系统本身。不需要跑额外的服务，不需要约定通信协议，不需要安装任何中间件。每个 Agent 只要能读能写本地文件，就能加入。缺点是 Agent 需要自己定期检查 inbox/，但优点是——它永远不会挂。

**命名即协议。** channel/ 通信规范里最重要的一条，其实是文件名格式：`YYMMDD-HHMMSS_<from>-to-<to>_<slug>.md`。这个文件名本身就是一个结构化消息——不需要解析正文，光看文件名就知道是谁、什么时候、发给谁、关于什么。好的约定不是写在文档里的，是写在文件名里的。

协调成本是真正的瓶颈。加 Agent 很容易，但每加一个，协调成本接近平方增长。channel/ 把这个成本从「手动 copy+paste」转移到了「手动路由」，已经省去很多重复性劳动。

在写这篇文章的过程中，我让 Buddy 帮我搜了一下，发现确实有人已经做过类似的事——有开源项目，有学术论文，有成熟的框架。

但这不重要。

有些观点我以为是自己的，后来在很久之前的书里读到，发现人家早就想过这个问题了。但这并不能否定我通过自己的思维触及到了这个想法——恰恰相反，它证明了我的直觉是对的。

别人做到了什么，别人取得了什么成就，不妨碍我凭着自己的灵感和实践摸索出同样的东西。这个过程对我来说非常受益——它让我感觉自己在进步，在拓展边界。

而这个过程本身，恰恰就是我和 AI Agents 互动的产物。如果没有 Charlie 挖掘早期记录，如果没有 Buddy 搜索参考文献，如果没有 Ada 扩写初稿，这篇文章不会以今天的形态出现。

**Welcome to the multi-agents world.**

*Damon · 2026-06-07 · Multi_Agent_World*
