---
title: "Understanding GitHub Tokens — A Beginner's Guide"
date: 2026-03-26T16:00:00
---

*从零开始理解 GitHub Token：它是什么，为什么需要它，为什么必须保密。*

*A plain-language guide to GitHub tokens — what they are, why they exist, and why they must be kept secret.*

---

## 1. 什么是 Token，为什么 GitHub 需要它？
<p class="heading-note">1. What Is a Token and Why Does GitHub Need It?</p>

### 生活比喻
<p class="heading-note">Real-Life Analogy</p>

想象您要进入一栋写字楼，有两种方式进入：

*Imagine entering an office building — two ways in:*

- **用户名+密码** = 前台人工核验身份证，慢，但适合人类 / Front desk manually checks your ID — slow, but works for humans
- **Token** = 门禁卡，刷一下就进，快速自动化，适合机器 / Key card — swipe and enter, fast and automated, designed for machines

### 为什么不直接用密码？
<p class="heading-note">Why Not Just Use a Password?</p>

当机器（如 Claude Cowork）要推送代码时，GitHub 不允许使用真实密码，原因有三：

*When a machine pushes code, GitHub refuses real passwords for three reasons:*

1. **安全** — 密码权限太大，泄露后攻击者可做任何事 / **Security** — a password has full access; if leaked, anything can be done
2. **可控** — Token 可限制权限范围，密码做不到 / **Control** — a token can be scoped to limited permissions; a password cannot
3. **可撤销** — Token 可随时单独删除，不影响密码 / **Revocability** — a token can be deleted at any time without changing your password

### Token 的本质
<p class="heading-note">What a Token Actually Is</p>

Token 就是一串随机字符，GitHub 服务器认识它、知道它的权限、随时可让它失效。

*A token is a random string — GitHub recognizes it, knows its permissions, and can invalidate it instantly.*

```
ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

---

## 2. `repo` 和 `workflow` 权限
<p class="heading-note">The `repo` and `workflow` Scopes</p>

创建 Token 时，GitHub 让您勾选"scope（权限范围）"——决定这把门禁卡能开哪些门。

*When creating a token, GitHub asks you to choose "scopes" — which doors this key card can open.*

### `repo` 权限
<p class="heading-note">The repo Scope</p>

| ✅ 能做什么 | ❌ 不能做什么 |
|---|---|
| 读取和推送代码 | 管理账户设置 |
| 创建和删除分支 | 删除账户 |
| 创建 Pull Request | 访问他人仓库 |

**比喻：** 办公室钥匙，能进办公室改文件，但进不了财务室。

*Analogy: an office key — enter and edit files, but not the finance room.*

### `workflow` 权限
<p class="heading-note">The workflow Scope</p>

`.github/workflows/` 是特殊文件——它控制自动化流程，风险更高，GitHub 单独保护它。

*Workflow files control automation — higher risk, so GitHub protects them separately.*

**比喻：** 普通员工能搬货（`repo`），但不是谁都能改工厂流水线程序（`workflow`）。

*Analogy: staff can move goods (`repo`), but not everyone can reprogram the assembly line (`workflow`).*

### 为什么两个都需要？
<p class="heading-note">Why Both?</p>

- 无 `repo` → 无法推送任何文件 ❌ / Without `repo` → cannot push any files ❌
- 有 `repo` 无 `workflow` → 推送含工作流的代码被拒绝 ❌ / `repo` only → workflow files rejected ❌
- 两个都有 → 完整推送成功 ✅ / Both → full push succeeds ✅

---

## 3. 为什么上一个 Token 报错？
<p class="heading-note">Why Did the Previous Token Fail?</p>

报错信息：

*The error:*
```
remote: Invalid username or token.
Password authentication is not supported for Git operations.
```

这个报错有误导性——真正含义是 **"Token 无效或权限不足"**，而不是"密码方式不支持"。

*This error is misleading — it means **"token invalid or missing permissions"**, not literally "password unsupported."*

原因：第一个 Token 没有勾选 `workflow`，GitHub 检测到 `.github/workflows/` 文件，发现权限不够，拒绝了整个推送。

*Cause: the first token lacked `workflow` scope. GitHub detected the workflows folder and rejected the push.*

> **记住：** 只要仓库里有 `.github/workflows/`，Token 就必须同时勾选 `repo` + `workflow`。
>
> *Remember: if your repo has `.github/workflows/`, always check both `repo` + `workflow`.*

---

## 4. 为什么 Token 泄露很危险？
<p class="heading-note">Why Is a Leaked Token Dangerous?</p>

### 今天发生了什么
<p class="heading-note">What Happened Today</p>

Token 被粘贴进了聊天窗口，等于把门禁卡照片发给了所有人。

*Pasting a token into chat is like photographing your key card and posting it publicly.*

持有者在 Token 被撤销之前，可以：

*Anyone who sees it can, before revocation:*

| 行动 | 后果 |
|---|---|
| 推送恶意代码 | 网站被篡改 |
| 删除所有分支 | 工作全部丢失 |
| 读取私有仓库 | 私人数据泄露 |

### GitHub 的自动保护
<p class="heading-note">GitHub's Auto-Protection</p>

GitHub 会自动扫描公开内容，一旦发现自己颁发的 Token，立即撤销——这也是为什么今天的 Token 推送失败了。

*GitHub auto-scans public content and immediately revokes any token it finds — which is why today's exposed tokens stopped working.*

### 安全使用规则
<p class="heading-note">Safe Usage Rules</p>

| ✅ 应该 | ❌ 不要 |
|---|---|
| 只在 Terminal 里输入 | 粘贴进聊天 |
| 用 Mac 钥匙串存储 | 存在截图或记事本 |
| 用完即撤销 | 长期保留不用的 Token |

一劳永逸的方案

*The permanent solution:*
```bash
git config --global credential.helper osxkeychain
```

---

## 总结
<p class="heading-note">Summary</p>

| 问题 | 答案 |
|---|---|
| Token 是什么？ | 给机器用的"门禁卡" |
| 为什么需要 `repo`？ | 推送代码的基本权限 |
| 为什么需要 `workflow`？ | 仓库有 Actions 文件，需额外权限 |
| 上个 Token 为何报错？ | 缺少 `workflow` 权限 |
| Token 泄露为何危险？ | 持有者可完全控制仓库 |
| 如何安全使用？ | 只在 Terminal 输入，用钥匙串存储 |

---

*"了解工具背后的逻辑，比记住操作步骤更重要。"*

*"Understanding the logic behind tools matters more than memorizing the steps."*
