---
title: "My First Website — A Beginner's Journey"
date: 2026-03-26T14:00:00
---

*How a life insurance agent with no coding background built and deployed a real website in one afternoon — every obstacle, every fix, and every lesson.*

---

## What We Built

A personal website using **Hugo** (a tool that turns simple text files into a full website) with a custom "forest" theme — clean, minimal, with:
- A homepage with tagline and recent blog posts
- About, Blog, Projects, and AI Glossary pages
- Automatic deployment every time you push changes to GitHub

---

## The Big Picture — How It All Works

Before diving into the obstacles, here is the mental model you need:

```
You write content (text files)
        ↓
Hugo turns them into HTML (a real website)
        ↓
Git tracks all your changes (like a save history)
        ↓
GitHub stores your code online (cloud backup + sharing)
        ↓
GitHub Actions automatically builds & publishes your site
        ↓
Your site is live at damonmiaom.github.io
```

Think of it like writing a Word document, saving it to iCloud, and having it automatically printed and displayed in a shop window every time you save.

---

## Key Concepts Explained Simply

| Term | Plain English |
|---|---|
| **Git** | A system that tracks every change you make to your files — like "Track Changes" in Word, but for code |
| **GitHub** | A website that stores your Git files online, like iCloud for code |
| **Branch** | A parallel copy of your project where you can experiment safely, without touching the main version |
| **Pull Request (PR)** | A proposal to add your branch's changes into the main version — like saying "please review and approve this draft" |
| **Merge** | Accepting the PR — officially combining the branch into main |
| **GitHub Actions** | Automatic tasks that run whenever you push code — in our case, it builds and publishes your website |
| **Hugo** | A tool that converts simple Markdown text files into a full HTML website |
| **Deploy** | Publishing your site so the world can see it |

---

## The Journey — Step by Step

### Step 0: Getting Started
**What happened:** Claude Code desktop app showed an error:
> "Unable to start session: account information is unavailable because your sign-in has expired."

**Why it happened:** The login session had expired — like being automatically logged out of an app after inactivity.

**How we fixed it:** Clicked the avatar icon (bottom-left) → Sign Out → Sign In again. Simple!

**Lesson:** This is a normal thing that happens with any app. Don't panic — just re-login.

---

### Step 1: Wrong Path — Two Different Worlds
**What happened:** Instructions from Claude.ai Cowork said to run:
```
cd /home/user/damonmiaom.github.io
```
But this path didn't exist on my Mac.

**Why it happened:** There are TWO separate environments involved:
- **Claude.ai Cowork** = a cloud computer running Linux, where paths look like `/home/user/...`
- **Your Mac (Claude Code)** = your local computer, where paths look like `/Users/Damon/...`

They are completely separate machines. Commands meant for one don't work on the other.

**The analogy:** It's like someone giving you directions to a shop in Shanghai when you're actually in Beijing. Both are real cities, but the directions only work if you're in the right place.

**How we fixed it:** We identified which commands belong where:
- Cowork terminal → for cloud environment commands
- Claude Code (your Mac) → for local commands

---

### Step 2: Cloning the Repository
**What we did:**
```bash
cd /tmp && git clone https://github.com/DamonMiaoM/damonmiaom.github.io.git site
cd site
git checkout -b claude/build-hugo-site-Dak4R
```

**What this means:**
- `git clone` = downloaded a copy of your GitHub repository to your Mac (like downloading a folder from iCloud)
- `git checkout -b` = created a new branch (a safe parallel copy to experiment in)

**Why we used a branch:** So the main version of your site stays clean while we build the new Hugo site. If something goes wrong, we just don't merge — main is untouched.

---

### Step 3: GitHub Personal Access Token
**What happened:** To push code from the Cowork environment to GitHub, we needed authentication — proof that you are really you.

**Why it happened:** GitHub requires a token (like a special password) for automated tools to push code. It's a security measure.

**How we fixed it:**
1. Went to github.com/settings/tokens
2. Created a new "classic" token with `repo` scope
3. Saved it (it only shows once!)
4. Used it as the password when prompted during `git push`

**Lesson:** This token is like a key to your GitHub account. Keep it safe and don't share it.

---

### Step 4: Commands Pasted in the Wrong Place
**What happened:** The git push commands got pasted into the Cowork **chat input box** instead of the **terminal**.

**Why it happened:** Claude.ai Cowork has two parts:
- The **chat box** — where you talk to Claude
- The **terminal** — where you run commands

They look similar but do very different things. Easy to confuse as a beginner!

**How we fixed it:** Went back to the existing Cowork conversation and found the terminal panel inside it.

**Lesson:** In coding tools, always check WHERE you are typing before you hit Enter.

---

### Step 5: Creating the Pull Request and Merging
**What we did:** On GitHub, created a PR to merge `claude/build-hugo-site-Dak4R` → `main`, then clicked "Merge pull request."

**What this means:**
- The branch (with all 15 Hugo site files) got officially combined into `main`
- This triggered GitHub Actions to automatically build and deploy the site

**The safety net analogy:**
- Branch = a draft chapter written on the side
- Main = the official published book
- PR = "I'd like to add this draft — please review"
- Merge = "Approved! Add it to the book"
- Even after merging, git keeps full history — you can always roll back

---

### Step 6: The First 404 Error
**What happened:** Visited damonmiaom.github.io and saw a "404 — File not found" page.

**Why it happened:** GitHub Pages hadn't been configured to use GitHub Actions yet. It was still using the old default method (Jekyll), which couldn't build a Hugo site.

**How we fixed it:**
Settings → Pages → Build and deployment → Source → selected **"GitHub Actions"**

**Lesson:** GitHub Pages has two modes. "GitHub Actions" is the modern, flexible mode that lets you use any tool (like Hugo). Always set this when using a custom build tool.

---

### Step 7: Still 404 — The Competing Workflows Problem
**What happened:** Even after setting Pages to GitHub Actions and the Hugo workflow showing ✅ Success, the site was still 404.

**Why it happened (the tricky part):** Two workflows ran at the same time when the PR was merged:
1. **"Deploy Hugo to GitHub Pages"** — our custom workflow, built the Hugo site in 25 seconds ✅
2. **"pages build and deployment"** — GitHub's old legacy workflow, ran in 42 seconds ✅

Because the legacy workflow finished 17 seconds LATER, it **overwrote** the Hugo deployment with an empty/broken site. Last one to finish wins — and the wrong one won.

**How we diagnosed it:**
- Checked Actions tab → saw both workflows ran
- Clicked into the Hugo workflow → Build logs showed "Pages: 12" (Hugo DID build correctly)
- Artifact was only 4.02 KB → suspiciously small, confirming the legacy workflow had overwritten it

**How we fixed it:**
Manually triggered a new run of the Hugo workflow. This time, no legacy workflow competed — Hugo's deployment stuck.

**Lesson:** When two processes write to the same place, the last one wins. Always check if there are competing workflows when debugging deployment issues.

---

### Step 8: SUCCESS! 🎉
The site loaded perfectly at https://damonmiaom.github.io

The forest theme was live with:
- Clean navigation: About | Blog | Projects | AI Glossary
- Hero tagline: "Notes on AI, technology, and thinking."
- First blog post: "Hello, World"

**Total time from zero to live website:** ~2 hours (including all the troubleshooting!)

---

## What I Learned

1. **Two environments exist** — cloud (Cowork/Linux) and local (Mac/Claude Code). Know which one you're in.
2. **Git branches are safety nets** — experiment freely, main stays clean
3. **PR + Merge = the safe way** to publish changes
4. **GitHub Actions = automation** — it builds and deploys your site for you
5. **404 doesn't always mean failure** — it often just means "not deployed yet" or "wrong configuration"
6. **Competing workflows can overwrite each other** — always check the Actions tab when debugging
7. **GitHub Personal Access Token** = your secure key for pushing code programmatically

---

## The Tech Stack

| Tool | Role | Analogy |
|---|---|---|
| **Hugo** | Builds the website from text files | A printing press |
| **Git** | Tracks all changes | "Track Changes" in Word |
| **GitHub** | Stores code online | iCloud for code |
| **GitHub Actions** | Automates build & deploy | A robot publisher |
| **GitHub Pages** | Hosts the website for free | A free shop window |
| **Markdown** | Simple text format for content | A simplified Word format |

---

*"The best way to learn technology is to build something real."*

---

<p class="post-revised">Published: Mar 26, 2026, 14:00</p>
