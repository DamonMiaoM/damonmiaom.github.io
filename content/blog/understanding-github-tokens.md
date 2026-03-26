---
title: "Understanding GitHub Tokens — A Beginner's Guide"
date: 2026-03-26
---

*A plain-language explanation of what tokens are, why GitHub needs them, and why they must be kept secret — written for someone building their first website.*

---

## 1. 什么是 Token，为什么 GitHub 需要它？
## 1. What Is a Token and Why Does GitHub Need It?

### 用一个生活比喻来理解 / A Real-Life Analogy

想象您要进入一栋写字楼。有两种方式：

Imagine you want to enter an office building. There are two ways:

- **用户名 + 密码** = 前台人工验证。你报上姓名，前台核对身份证，放你进去。慢，但适合人类。
- **Token** = 门禁卡。一张小卡片，刷一下就进。快速、自动化，适合机器。

- **Username + Password** = the front desk manually checks your ID. Slow, but works for humans.
- **Token** = a key card. Swipe and enter. Fast, automated — designed for machines.

### 为什么不直接用密码？/ Why Not Just Use a Password?

当 Claude Cowork（云端机器）要推送代码到 GitHub 时，它是一台**机器**，不是人。GitHub 不允许机器用您的真实密码操作，原因是：

When Claude Cowork (a cloud machine) pushes code to GitHub, it's a **machine**, not a human. GitHub doesn't allow machines to use your real password because:

1. **安全** — 密码权限太大。一旦泄露，攻击者可以做任何事，包括删号。
   **Security** — a password has full access. If leaked, an attacker can do anything, including deleting your account.

2. **可控** — Token 可以限制权限范围（只读？只写？只操作某个仓库？），密码做不到。
   **Control** — a token can be scoped to limited permissions. A password cannot.

3. **可撤销** — Token 可以随时单独撤销，不影响密码。密码被盗只能全面修改。
   **Revocability** — a token can be deleted at any time without changing your password.

### Token 的本质 / What a Token Actually Is

Token 就是一串随机字符，例如：
A token is simply a random string of characters, like:

```
ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

GitHub 服务器认识这串字符，知道它属于您，知道它有哪些权限，并且随时可以让它失效。

GitHub's servers recognize this string, know it belongs to you, know what permissions it has, and can invalidate it at any time.

---

## 2. `repo` 和 `workflow` 权限分别控制什么？
## 2. What Do `repo` and `workflow` Permissions Control?

当您创建 Token 时，GitHub 让您勾选"scope（范围）"。这是一个权限清单，决定这把"门禁卡"能开哪些门。

When you create a token, GitHub asks you to choose "scopes" — a permission list that decides which doors this key card can open.

### `repo` 权限 — 仓库的读写钥匙

| 能做什么 | 不能做什么 |
|---|---|
| 读取代码 | 管理账户设置 |
| 推送代码（git push）| 删除账户 |
| 创建/删除分支 | 管理其他人的仓库 |
| 创建 Pull Request | 访问其他 GitHub 功能 |

**比喻：** `repo` 权限 = 办公室的钥匙。能进办公室、搬文件、改文件，但不能动财务室、不能改公司章程。

**Analogy:** `repo` = office key. You can enter, move files, edit files — but not access the finance room or change company rules.

### `workflow` 权限 — GitHub Actions 的修改权

GitHub Actions 工作流文件（`.github/workflows/deploy.yml`）是特殊文件。GitHub 单独保护它，因为工作流可以**自动执行代码**，安全风险更高。

GitHub Actions workflow files (`.github/workflows/deploy.yml`) are special. GitHub protects them separately because workflows can **automatically execute code** — higher security risk.

| 能做什么 | 不能做什么 |
|---|---|
| 推送包含 `.github/workflows/` 的代码 | 触发或停止 Actions 运行 |
| 修改工作流文件内容 | 管理 Secrets |

**比喻：** `workflow` 权限 = 修改工厂自动化流水线的权限。普通员工能搬货（repo），但不是谁都能改流水线程序（workflow）。

**Analogy:** `workflow` = permission to modify the factory's automated assembly line. Regular staff can move goods (`repo`), but not everyone can reprogram the conveyor belt (`workflow`).

### 为什么两个都需要？/ Why Do We Need Both?

我们的仓库里有 `.github/workflows/deploy.yml`。当我们 `git push` 时：
Our repo contains `.github/workflows/deploy.yml`. When we `git push`:

- 没有 `repo` → 无法推送任何文件 ❌
- 有 `repo` 但没有 `workflow` → 可以推送普通文件，但推送包含工作流文件的代码会被拒绝 ❌
- 两个都有 → 完整推送成功 ✅

- Without `repo` → cannot push any files ❌
- `repo` but no `workflow` → can push normal files, but pushing workflow files is rejected ❌
- Both → full push succeeds ✅

---

## 3. 为什么上一个 Token 报错（权限不够）？
## 3. Why Did the Previous Token Fail?

报错信息是：
The error message was:
```
remote: Invalid username or token.
Password authentication is not supported for Git operations.
```

### 真正的原因 / The Real Cause

这个报错其实有误导性——它不是说"密码方式不支持"，而是说**"这个 Token 无效或权限不足"**。

This error is misleading — it doesn't mean "password method unsupported." It means **"this token is invalid or lacks permission."**

最可能的原因是第一个 Token 创建时只勾选了部分权限，**没有勾选 `workflow`**。因为我们的仓库里有 `.github/workflows/deploy.yml`，GitHub 检测到这个文件，发现 Token 没有 `workflow` 权限，于是拒绝整个推送。

The most likely cause: the first token was created without checking `workflow`. Because our repo contains `.github/workflows/deploy.yml`, GitHub detected this file, saw the token lacked `workflow` permission, and rejected the entire push.

### 一个简单的记忆规则 / A Simple Rule to Remember

> 只要您的仓库里有 `.github/workflows/` 文件夹，推送时用的 Token 就**必须同时勾选 `repo` 和 `workflow`**。

> Whenever your repo has a `.github/workflows/` folder, the token used for pushing **must have both `repo` and `workflow`**.

---

## 4. 为什么 Token 泄露很危险？
## 4. Why Is a Leaked Token Dangerous?

### 今天发生了什么 / What Happened Today

今天您两次把完整的 Token 粘贴进了聊天窗口：
Today you pasted your full token into the chat twice:
```
ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx  ← 第一个（已泄露）
ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx  ← 第二个（已泄露）
```

### 泄露之后会发生什么？/ What Can Happen After a Leak?

一旦有人拿到您的 Token，在它被撤销之前，他们可以：

Once someone has your token, before you revoke it, they can:

| 行动 | 后果 |
|---|---|
| `git push` 恶意代码 | 您的网站被篡改，显示钓鱼页面或广告 |
| 删除所有分支和代码 | 您的工作全部丢失 |
| 读取私有仓库 | 泄露您的私人项目和数据 |
| 修改工作流 | 在您的仓库里运行任意代码 |

### GitHub 的自动检测 / GitHub's Automatic Detection

有趣的是，GitHub 会**自动扫描公开对话和代码库**，一旦发现自己颁发的 Token 出现在公开位置，会立即自动撤销它。这也是为什么第一个 Token 在使用时可能已经失效。

Interestingly, GitHub **automatically scans public conversations and repositories**. If it detects one of its own tokens appearing publicly, it immediately invalidates it. This is likely why the first token was already invalid when we tried to use it.

### 正确的 Token 使用习惯 / Safe Token Habits

| ✅ 应该这样做 | ❌ 不要这样做 |
|---|---|
| 只在自己的 Terminal 里输入 | 粘贴进聊天窗口 |
| 用完立即撤销 | 长期保留不用的 Token |
| 存入 Mac 钥匙串（自动管理）| 存在记事本或截图里 |
| 为不同用途创建不同 Token | 一个 Token 到处用 |
| 给 Token 起有意义的名字 | 用完全相同的名字创建多个 |

### 最安全的长期方案 / The Safest Long-Term Solution

在 Terminal 里运行一次：
Run this once in Terminal:

```bash
git config --global credential.helper osxkeychain
```

之后第一次推送时输入 Token，Mac 的钥匙串会安全保存它，以后再也不需要手动输入。Token 不会出现在任何聊天记录或命令历史里。

After this, enter your token once on first push. Mac's Keychain securely stores it — you'll never need to type it again. The token won't appear in any chat history or command history.

---

## 总结 / Summary

| 问题 | 答案 |
|---|---|
| Token 是什么？ | 给机器用的"门禁卡"，代替密码 |
| 为什么需要 `repo`？ | 推送代码的基本权限 |
| 为什么需要 `workflow`？ | 仓库里有 Actions 文件，需要额外权限 |
| 上个 Token 为何报错？ | 缺少 `workflow` 权限，被 GitHub 拒绝 |
| Token 泄露为何危险？ | 持有者可以完全控制您的仓库 |
| 如何安全使用？ | 只在 Terminal 输入，用 Mac 钥匙串存储 |

---

*"了解工具背后的逻辑，比记住操作步骤更重要。"*
*"Understanding the logic behind tools matters more than memorizing the steps."*

*Document compiled: March 26, 2026*
