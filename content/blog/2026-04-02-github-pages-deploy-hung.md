---
title: "GitHub Pages 样式不更新？Deploy Job 卡死了 12 小时"
date: 2026-04-02T20:00:00
author: Damon Miao
description: "改了三次 CSS，网站还是旧的。最终发现 GitHub Actions 的 deploy job 已经挂了超过 12 个小时——而原因只是少了一行配置。"
tags: [Hugo, GitHub Pages, GitHub Actions, 调试, 个人网站]
slug: github-pages-deploy-hung
---

## 背景

<p class="heading-note">Context</p>

在开发这个网站（Hugo + forest 主题）时，我对页面标题区域的 CSS 样式做了修改，多次 `commit & push` 之后，线上页面始终没有更新。

---

## 排查过程

<p class="heading-note">Debugging Process</p>

**第一步：确认 CSS 文件是否已更新**

```bash
curl -s "https://damonmiaom.github.io/css/main.css?v=6" | grep "ph-title"
# 941:.ph-title {
# 952:.ph-title-zh {
```

CSS 文件本身已经更新，样式是真实存在的。

**第二步：对比两个页面的 HTML 输出**

| 页面 | 内联 `<style>` 块 | 状态 |
|------|-----------------|------|
| `/changelog/` | ✅ 有 | 正常显示 |
| `/about/` | ❌ 无 | 样式不生效 |

找一个「能用的页面」做对比，是调试时快速缩小范围的好方法。

**第三步：检查 GitHub Actions**

打开 Actions 日志，发现第 83 个 workflow：

- `build` 步骤 ✅ 成功
- `deploy` 步骤 🟠 卡住——已经运行了 **12 小时 27 分钟**

```
deploy
Started 12h 27m 36s ago
Evaluating deploy.if
Evaluating: success()
Result: true
```

deploy job 永久停留在评估阶段，后续所有提交的部署请求全部被堵死。

---

## 根本原因

<p class="heading-note">Root Causes</p>

发现两个叠加的问题：

**1. Deploy Job 没有设置超时时间**

GitHub Actions 的 job 默认没有超时限制。某次 deploy 因为某种原因卡住（可能是等待 GitHub Pages 环境权限响应），就这样永远挂着，后续所有部署都无法执行。

**2. GitHub Pages Source 设置需要确认**

`Settings → Pages → Build and deployment → Source` 必须设置为 **GitHub Actions**，而不是 branch 模式。两者不匹配时，workflow 跑多少次都不会真正部署。

---

## 修复方案

<p class="heading-note">Fix</p>

在 `.github/workflows/deploy.yml` 的 deploy job 下加一行：

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    needs: build
    timeout-minutes: 10   # ← 加这一行
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        ...
```

加了 timeout 之后，推代码，deploy job 两分钟内顺利完成，样式恢复正常。

---

## 经验教训

<p class="heading-note">Lessons Learned</p>

1. **CSS 没更新 ≠ CSS 写错了**：先检查部署链路，再检查代码本身。
2. **GitHub Actions 的橙色不代表「在跑」**：可能已经卡死，看运行时间才知道。
3. **Deploy job 必须设 `timeout-minutes`**：无超时的 job 会无声挂起，让所有后续部署静默失败。
4. **找一个「能用的页面」做对比**：快速定位问题在哪一层，比逐一排查省时间。

---

## 排查清单

<p class="heading-note">Checklist</p>

下次遇到 GitHub Pages 内容不更新，按顺序查：

- [ ] CSS 文件是否已更新（`curl` 验证）
- [ ] GitHub Actions 所有 job 是否全绿（特别看 deploy）
- [ ] Deploy job 是否有 `timeout-minutes`
- [ ] `Settings → Pages → Source` 是否为 **GitHub Actions**
- [ ] 是否存在浏览器/CDN 缓存（用 `?v=N` 测试）
