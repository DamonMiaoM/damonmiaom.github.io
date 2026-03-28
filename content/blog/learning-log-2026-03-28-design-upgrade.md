---
title: "How I Upgraded My Site's Design in One Session — 10 Changes Explained"
date: 2026-03-28
author: Damon Miao
description: "A complete account of 10 design improvements made to this site in a single session — grain textures, scroll animations, dark mode, a table of contents, and more. Explained in plain English."
tags: [learning-log, hugo, css, design, beginners, web-design]
slug: design-upgrade-10-improvements
---

# Learning Log — March 28, 2026
# 学习日志 — 2026年3月28日

**Topic:** 10 Design Upgrades — From Flat to Felt

**主题：** 10 项设计升级——从平淡到有质感

---

## What Happened Today / 今天发生了什么

Today I worked with Claude to do a complete design audit of this site and implement 10 improvements — all in one session. We started by fetching the live site, reading the source code, and thinking like a senior web designer across every dimension: structure, aesthetics, motion, accessibility, and interactivity.

今天我和 Claude 对这个网站做了一次全面的设计审查，并在一次会话里落地了 10 项改进。我们先抓取了线上页面，读了源码，然后像一位资深网页设计师那样，从结构、美学、动效、可访问性、交互性等维度逐一审视。

None of these required a complete rebuild. Every change was surgical — precise edits to the CSS and Hugo templates that already existed.

这些改进没有一项需要重建网站。每一项都是精准的"手术式"修改——只针对已有的 CSS 和 Hugo 模板做定点调整。

---

## The 10 Changes / 10 项改进

### Change #1: Scroll-triggered Stagger on Homepage Posts
### 变化 #1：首页文章列表的滚动渐入动画

**What it does:** When you load the homepage, the blog posts don't all appear at once — they fade up into view one by one, each with a slight delay.

**它做了什么：** 加载首页时，博客文章不再同时出现，而是一篇一篇依次向上淡入，每篇之间有轻微的时间差。

**How it works:**

The browser has a built-in tool called `IntersectionObserver` — think of it as a "watchman" that notices when something enters the visible area of the screen. When each post enters view, the watchman triggers a CSS animation.

浏览器有一个内置工具叫 `IntersectionObserver`——把它想象成一个"守望者"，当某样东西进入屏幕可见区域时，它就会注意到。每篇文章进入视野时，守望者就触发一段 CSS 动画。

```javascript
var observer = new IntersectionObserver(function (entries) {
  entries.forEach(function (entry) {
    if (entry.isIntersecting) {
      entry.target.classList.add('is-visible');
    }
  });
}, { threshold: 0.08 });
```

Each item starts invisible and shifted 22px downward. When the class `is-visible` is added, it transitions to full opacity at its natural position. Items stagger 90ms apart so they ripple in like a wave.

每个项目一开始是不可见的，并向下偏移了 22px。当 `is-visible` 类被添加时，它就过渡到完全不透明且回到正常位置。各项目之间错开 90ms，像波浪一样依次出现。

---

### Change #2: Noise Grain Texture on the Background
### 变化 #2：背景的噪点颗粒感

**What it does:** The background no longer feels like a flat digital screen. It has a subtle paper-like texture — almost invisible, but it makes the whole site feel warmer and more physical.

**它做了什么：** 背景不再像一块平坦的数字屏幕。它多了一层微妙的纸张质感——几乎看不见，但让整个网站感觉更温暖、更有触感。

**How it works:**

A tiny SVG image is created entirely in code — no image file needed. It uses a filter called `feTurbulence` which generates random visual noise mathematically. This is overlaid on the entire page at just 4% opacity using a CSS pseudo-element.

一张微型 SVG 图片完全用代码生成——不需要任何图片文件。它使用一个叫 `feTurbulence` 的滤镜，通过数学方式生成随机视觉噪点。然后用 CSS 伪元素以 4% 的透明度叠加在整个页面上。

```css
body::after {
  content: '';
  position: fixed;
  inset: 0;
  pointer-events: none;  /* clicks pass through it */
  opacity: 0.04;
  background-image: url("data:image/svg+xml,...feTurbulence...");
}
```

`pointer-events: none` means the texture layer is purely visual — you can still click and scroll through it normally.

`pointer-events: none` 表示这个纹理层纯粹是视觉的——你仍然可以正常点击和滚动。

---

### Change #3: Reading Time + Progress Bar on Articles
### 变化 #3：文章的阅读时间与进度条

**What it does:** Every blog post now shows an estimated reading time (like "9 min read") next to the date. A thin green bar at the very top of the page grows from left to right as you scroll through the article.

**它做了什么：** 每篇博客文章的日期旁边现在显示预计阅读时间（如"9 min read"）。页面顶部有一条细细的绿色进度条，随着你向下滚动文章而从左向右增长。

**How it works:**

Hugo (the tool that builds this site) automatically calculates reading time based on word count. Accessing it is a single line in the template:

Hugo（构建这个网站的工具）会自动根据字数计算阅读时间。在模板里访问它只需一行：

```html
{{ .ReadingTime }} min read
```

The progress bar is a `div` fixed to the top of the page. A small JavaScript function calculates how far you've scrolled through the article and updates the bar's width as a percentage.

进度条是一个固定在页面顶部的 `div`。一个小型 JavaScript 函数计算你在文章中滚动了多远，并将进度条的宽度以百分比形式更新。

---

### Change #4: JetBrains Mono for Code
### 变化 #4：代码字体换成 JetBrains Mono

**What it does:** Every code block on the site now uses JetBrains Mono instead of the default Courier New. The difference is like switching from a generic pen to a fine-tipped calligraphy pen.

**它做了什么：** 网站上所有代码块现在使用 JetBrains Mono 而不是默认的 Courier New。区别就像从普通圆珠笔换成了精细的书法笔。

**How it works:**

The font was added to the Google Fonts import in the base template, then the CSS was updated to use it for all code elements.

字体被添加到基础模板的 Google Fonts 导入中，然后 CSS 被更新为所有代码元素使用它。

```css
.post-content code,
.post-content pre {
  font-family: 'JetBrains Mono', 'Courier New', monospace;
}
```

The second and third fonts are "fallbacks" — if JetBrains Mono fails to load for any reason, the browser uses Courier New, then the system default monospace font.

第二和第三个字体是"备用字体"——如果 JetBrains Mono 因任何原因加载失败，浏览器会使用 Courier New，然后是系统默认等宽字体。

---

### Change #5: Hero Entrance Animation
### 变化 #5：首页 Hero 区域的入场动画

**What it does:** When you visit the homepage, the tagline *"Notes on AI, technology, and thinking."* doesn't just appear — it glides up and fades in over about one second.

**它做了什么：** 当你访问首页时，标语 *"Notes on AI, technology, and thinking."* 不再突然出现，而是在大约一秒内向上滑入并淡入。

**How it works:**

A CSS `@keyframes` animation defines the motion path — starting 18px below with 0 opacity, ending at natural position with full opacity. A spring-like easing curve (`cubic-bezier(0.16, 1, 0.3, 1)`) gives it a natural, organic feel.

一个 CSS `@keyframes` 动画定义了运动轨迹——从低 18px、透明度为 0 开始，结束于自然位置、完全不透明。弹簧式缓动曲线（`cubic-bezier(0.16, 1, 0.3, 1)`）赋予它自然、有机的感觉。

```css
@keyframes hero-enter {
  from { opacity: 0; transform: translateY(18px); }
  to   { opacity: 1; transform: translateY(0); }
}
```

---

### Change #6: Active Navigation State
### 变化 #6：导航栏的当前页高亮

**What it does:** When you're on the Blog page, "BLOG" in the navigation shows a green underline. Same for all other sections — you always know where you are.

**它做了什么：** 当你在博客页面时，导航栏中的"BLOG"会显示绿色下划线。所有其他栏目也一样——你随时都知道自己在哪里。

**How it works:**

Hugo knows the URL of the current page. In the header template, we check if the current URL starts with each section's path. If it matches, we add the class `nav-active`.

Hugo 知道当前页面的 URL。在 header 模板中，我们检查当前 URL 是否以每个栏目的路径开头。如果匹配，就添加 `nav-active` 类。

```html
<a href="/blog/"{{ if hasPrefix .RelPermalink "/blog" }} class="nav-active"{{ end }}>Blog</a>
```

**One complication I learned:** Blog and Journal are folders in the content directory, so Hugo gives them a "section" label. But About, Projects, Gallery, and Changelog are single files — Hugo doesn't label them as sections. That's why the first version only worked for Blog and Journal. The fix was to compare by URL path instead, which works for everything.

**我学到的一个坑：** Blog 和 Journal 是内容目录里的文件夹，所以 Hugo 给它们一个"section"标签。但 About、Projects、Gallery 和 Changelog 是单独的文件——Hugo 不把它们标记为 section。这就是第一个版本只对 Blog 和 Journal 有效的原因。修复方法是改用 URL 路径比较，这对所有页面都有效。

---

### Change #7: Table of Contents for Long Posts
### 变化 #7：长文章的目录侧边栏

**What it does:** On wide screens (1300px and above), blog posts with section headings show a sticky "Contents" sidebar on the left. The current section highlights in green as you scroll. On small screens it hides automatically.

**它做了什么：** 在宽屏（1300px 及以上）上，有章节标题的博客文章左侧会显示一个固定的"Contents"侧边栏。当前所在章节会随着滚动高亮显示为绿色。在小屏幕上自动隐藏。

**How it works:**

Hugo automatically generates a table of contents from your headings with a single template variable:

Hugo 用一个模板变量自动从标题生成目录：

```html
{{ .TableOfContents }}
```

A CSS grid splits the article area into two columns — one for the TOC, one for the content. `position: sticky` makes the TOC stay in view as you scroll down.

CSS 网格将文章区域分成两列——一列给目录，一列给内容。`position: sticky` 让目录在你向下滚动时保持在视野中。

The active heading highlight uses the same `IntersectionObserver` technique from Change #1 — watching headings as they scroll into view and updating the corresponding TOC link.

活跃标题高亮使用了与变化 #1 相同的 `IntersectionObserver` 技术——监视标题滚动进入视野时更新对应的目录链接。

---

### Change #8: Dark Mode
### 变化 #8：深色模式

**What it does:** When your Mac (or phone, or PC) is set to Dark Mode, the website automatically switches to a deep forest-green dark theme. No button to press — it follows your system preference.

**它做了什么：** 当你的 Mac（或手机、PC）设置为深色模式时，网站会自动切换到深色森林绿主题。不需要按任何按钮——它跟随你的系统偏好。

**How it works:**

CSS has a built-in way to detect the user's system preference called `prefers-color-scheme`. We wrap dark-mode styles in this query, and browsers apply them automatically.

CSS 有一种内置方式来检测用户的系统偏好，叫做 `prefers-color-scheme`。我们把深色模式的样式包在这个查询里，浏览器会自动应用它们。

```css
@media (prefers-color-scheme: dark) {
  :root {
    --bg: #0D1210;       /* very dark green-black */
    --surface: #182018;
    --border: #263026;
  }
  body { color: #CDD9D0; }  /* near-white text */
}
```

Because the entire site uses CSS variables (like `--bg` and `--forest`) instead of hardcoded colours, we only need to redefine those variables in one place.

因为整个网站使用 CSS 变量（如 `--bg` 和 `--forest`）而不是硬编码颜色，我们只需要在一个地方重新定义这些变量。

**Note I learned:** Chrome has its own appearance setting that can override your Mac's system preference. If dark mode doesn't appear in Chrome, check Chrome Settings → Appearance and switch it to "Follow system."

**我学到的注意事项：** Chrome 有自己的外观设置，可能会覆盖 Mac 的系统偏好。如果深色模式在 Chrome 中不显示，检查 Chrome 设置→外观，将其切换为"跟随系统"。

---

### Change #9: Related Posts at Article End
### 变化 #9：文章末尾的相关推荐

**What it does:** At the bottom of every blog post (above the comments), three other recent articles are suggested. Each shows the title on the left and the date on the right.

**它做了什么：** 每篇博客文章的底部（评论区上方）会推荐三篇其他近期文章。每篇左边显示标题，右边显示日期。

**How it works:**

Hugo can access all pages on the site from any template. We filter to blog posts only, exclude the current post, and take the first three:

Hugo 可以从任何模板访问网站上的所有页面。我们过滤只显示博客文章，排除当前文章，取前三篇：

```html
{{ $related := first 3 (where (where .Site.RegularPages "Section" "blog") "RelPermalink" "ne" .RelPermalink) }}
```

Read this in plain English: *"Take all pages on the site. Keep only those in the blog section. Remove the current post. Show the first three."*

用中文直白地读：*"取网站上所有页面。只保留博客栏目的。移除当前文章。显示前三篇。"*

---

### Change #10: Drop Cap on Long-Form Posts
### 变化 #10：长文章的首字下沉

**What it does:** The first letter of the first paragraph in every blog post is enlarged and styled in forest green — like the opening of a printed book or magazine.

**它做了什么：** 每篇博客文章第一段的第一个字母都会被放大并以森林绿样式显示——就像印刷书籍或杂志的开篇。

**How it works:**

Pure CSS — no JavaScript needed. The `::first-letter` pseudo-element targets just the first character of an element:

纯 CSS——不需要 JavaScript。`::first-letter` 伪元素只针对元素的第一个字符：

```css
.has-dropcap > p:first-of-type::first-letter {
  font-size: 3.6em;
  font-weight: 700;
  float: left;
  line-height: 0.78;
  color: var(--forest);
}
```

`float: left` is the key — it lets the letter be large while the rest of the text wraps around it naturally.

`float: left` 是关键——它让字母可以很大，同时其余文字自然地环绕在它周围。

---

## Files Changed / 修改了哪些文件

| File | What Changed |
|------|-------------|
| `themes/forest/static/css/main.css` | Added all visual styles for every change |
| `themes/forest/layouts/_default/baseof.html` | Added JetBrains Mono to Google Fonts import |
| `themes/forest/layouts/_default/single.html` | Added TOC sidebar, reading time, progress bar, drop cap class, related posts |
| `themes/forest/layouts/index.html` | Added scroll stagger JavaScript |
| `themes/forest/layouts/partials/header.html` | Added active nav state logic |
| `hugo.toml` | Added table of contents configuration (h2 and h3 only) |

---

## New Vocabulary / 今天学到的新词汇

### IntersectionObserver / 交叉观察者
A browser API that watches for when elements enter or leave the visible area of the screen. Used here for both the homepage scroll stagger and the TOC active highlighting. No scroll event listeners needed — more efficient.

浏览器 API，用于监视元素何时进入或离开屏幕可见区域。这里用于首页滚动错位动画和目录活跃高亮。不需要滚动事件监听器——更高效。

### CSS Pseudo-element / CSS 伪元素
A virtual element created by CSS that doesn't exist in the HTML. `::before` and `::after` create invisible elements before/after a real element. `::first-letter` targets just the first character. The grain texture overlay uses `body::after`.

CSS 创建的虚拟元素，在 HTML 中不存在。`::before` 和 `::after` 在真实元素前后创建不可见元素。`::first-letter` 只针对第一个字符。颗粒纹理叠加层使用了 `body::after`。

### CSS Variables / CSS 变量
Values defined once with `--name: value` and reused throughout a stylesheet with `var(--name)`. This site uses them for all colours. Dark mode works by redefining just the variable values — every place that uses `var(--bg)` updates automatically.

用 `--name: value` 定义一次并在整个样式表中用 `var(--name)` 复用的值。这个网站用它来管理所有颜色。深色模式通过重新定义变量值来工作——每个使用 `var(--bg)` 的地方都会自动更新。

### CSS Grid / CSS 网格布局
A two-dimensional layout system. Used here for the TOC sidebar: one column for the table of contents (200px), one column for the article content (flexible). Only activates on wide screens — invisible on mobile.

二维布局系统。这里用于目录侧边栏：一列给目录（200px），一列给文章内容（弹性）。只在宽屏上激活——在移动端不可见。

---

## Summary / 今日总结

| # | Change | Where to see it |
|---|--------|----------------|
| 1 | Scroll stagger | Homepage — refresh and watch posts appear |
| 2 | Grain texture | Everywhere — look closely at the background |
| 3 | Reading time + progress bar | Any blog post — top of page + date line |
| 4 | JetBrains Mono | Any post with code blocks |
| 5 | Hero animation | Homepage — watch tagline on load |
| 6 | Active nav | Click any nav link — it gets underlined |
| 7 | Table of contents | Long blog posts on wide screens |
| 8 | Dark mode | Set Mac/browser to Dark mode |
| 9 | Related posts | Bottom of any blog post |
| 10 | Drop cap | First letter of every blog post |

---

## A Note to My Future Self / 写给未来的自己

Design improvements don't always require starting over. The most impactful session I've had on this site made ten meaningful changes — each one small, targeted, and reversible. The site still uses the same theme, the same fonts, the same structure. It just breathes better now.

设计改进并不总需要推倒重来。我在这个网站上最有成效的一次会话做了十项有意义的改进——每一项都小巧、精准、可撤销。网站仍然使用相同的主题、相同的字体、相同的结构。只是现在它更有生气了。

The lesson: read the existing code first. Understand what's already there. Then find the smallest change that creates the biggest difference.

教训：先读现有代码。理解已有的东西。然后找到能产生最大差异的最小改动。

---

*Written with Claude Code | Ghostty Terminal | MacBook Pro*
*2026-03-28*
