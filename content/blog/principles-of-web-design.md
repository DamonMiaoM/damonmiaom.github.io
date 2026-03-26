---
title: "Principles of Web Design — A Practitioner's Field Notes"
date: 2026-03-26T22:00:00
description: "Everything learned from building a website from scratch — distilled into principles of layout, typography, color, spacing, and taste. In the spirit of Ray Dalio: not rules, but hard-won observations."
tags: ["Web Design", "Principles", "Reflection"]
---

*这篇文章写于我从零开始搭建个人网站之后。它不是教程，不是手册，而是原则——从真实的错误、真实的参考、真实的决策中提炼出来的观察。它借鉴了 Ray Dalio 的写作方式：不是"应该这样做"，而是"我发现事情是这样运作的"。*

*This was written after building a personal website from nothing. It is not a tutorial, not a manual, but a set of principles — observations distilled from real mistakes, real references, and real decisions. It borrows from Ray Dalio's approach: not "you should do this," but "here is how I found things to actually work."*

---

## I. 设计即删减 / Design Is Subtraction

The first instinct is always to add. More color. More sections. More animation. More text. The beginner's page is a cluttered room where every object shouts for attention simultaneously, and nothing is heard.

The mature design does the opposite. It asks: what can I remove?

The Japanese call this *ma* (間) — the meaningful pause, the deliberate empty space between things. In web design, it appears as whitespace between sections, a single accent color instead of four, a navigation bar with four links instead of ten.

Every reference site that shaped our work — Natsumi Nishizumi, Plum Village, Naval's archive — shared one characteristic: they had clearly decided what *not* to include. The restraint was not an absence of effort. It was the product of more effort than decoration ever requires.

**原则1：在添加任何东西之前，先问自己能删掉什么。设计的质量，往往体现在它的克制上。**

**Principle 1: Before adding anything, ask what you can remove. The quality of a design is often measured by its restraint.**

---

## II. 色板是灵魂 / The Palette Is the Soul

Color communicates before words do. A visitor *feels* the palette before they read a single sentence. The emotional register of a site — calm or urgent, warm or clinical, intimate or professional — is set almost entirely by its colors.

In building this site, we moved through two palettes:

**Old (warm forest green):**
`#F4F5EF` / `#7D8A35` / `#8FA882` / `#2E4A28` / `#1B2D1A`

**New (cool sage and near-black):**
`#F1F1F1` / `#DCDEDF` / `#96A89C` / `#16452C` / `#000C06`

The shift from warm olive to cool sage changed the entire emotional temperature — from "summer garden" to "still water at dawn." Neither palette is objectively better. But each produces a different *feeling*, and the designer's job is to choose the feeling deliberately.

### The Five Roles / 颜色的五个角色

Choose five colors and assign each a specific role. Discipline here eliminates hundreds of micro-decisions later.

| Role | Function | Our Color |
|---|---|---|
| **Background** | The silence the content sits on | `#F1F1F1` |
| **Surface** | Cards, code blocks, subtle differentiation | `#DCDEDF` |
| **Text** | The primary voice | `#000C06` |
| **Accent** | The brand moment — headings, links, buttons | `#16452C` |
| **Secondary** | Dates, captions, metadata — the quiet parts | `#96A89C` |

**原则2：选五种颜色，给每种颜色分配一个角色。这五个角色之外，不引入额外颜色。**

**Principle 2: Choose five colors and give each a role. Introduce no colors outside these five roles.**

---

## III. 字体：阅读的建筑 / Typography: The Architecture of Reading

Typography is not decoration — it is structure. The reader's eye navigates a page the way a person navigates a building: looking for doors, hallways, and rooms. The type system is that architecture.

### 两种字体，最多 / Two Fonts, Maximum

In this project: **Lora** (a warm, editorial serif) carries the *voice* — body text, article titles, the hero headline. **Poppins** (a geometric sans-serif) handles the *infrastructure* — navigation, dates, labels, metadata.

Why this pairing works: serif creates warmth and authority. Sans-serif creates clarity and precision. Together, they establish a two-register system: "this is the content" versus "this is the interface." The reader never consciously notices the switch — but they always feel it.

**原则3：用衬线体表达内容，用无衬线体构建界面。绝不混用超过两种字体。**

**Principle 3: Use serif for voice, sans-serif for interface. Never mix more than two typefaces.**

### 层级：眼睛不需要思考 / Scale: The Eye Should Not Need to Think

Typography hierarchy means the reader instantly knows what is most important — not by reading, but by *seeing*. The jump between levels must be dramatic enough to be felt without conscious analysis.

| Element | Size | Weight | Font |
|---|---|---|---|
| Hero headline | 4.8rem | 400 italic | Lora |
| Article title | 2.1rem | 700 | Lora |
| Card / list title | 1.5rem | 700 | Lora |
| Section heading (h2) | 1.35rem | 600 | Poppins |
| Body text | 1.0625rem | 400 | Lora |
| Meta / caption | 0.7rem | 400 | Poppins |

**原则4：建立字体层级。层级之间的跳跃要足够显著，让眼睛不需要思考就能识别优先级。**

**Principle 4: Build a type scale. The jump between levels must be large enough that the eye recognizes hierarchy without conscious thought.**

### 行高是对读者的礼貌 / Line-Height Is Hospitality

A line-height of 1.4 is a crowded subway car. A line-height of 1.8 is a quiet room where thinking is possible. In long-form reading — especially bilingual text where Chinese and English alternate — generous line-height is not a luxury. It is the minimum courtesy extended to the reader.

**原则5：正文行高至少1.8；标题行高1.2。给读者足够的空气。**

**Principle 5: Body text deserves `line-height: 1.8`. Headlines deserve `line-height: 1.2`. Give the reader air.**

### 字距是工具，不是装饰 / Letter-Spacing Is a Tool, Not Decoration

Uppercase labels, navigation links, and small-cap metadata benefit from increased letter-spacing (`0.1em` to `0.2em`). It creates breathing room within compressed text and signals to the reader: "this is a label, not content." Never add letter-spacing to body copy — it disrupts the reading rhythm that took centuries of typographic practice to perfect.

**原则6：大写标签加字距。正文绝不加字距。**

**Principle 6: Add letter-spacing to uppercase labels. Never add it to body copy.**

---

## IV. 留白：休息的哲学 / Whitespace: The Philosophy of Rest

We instinctively fear empty space. It feels unfinished, wasteful, lazy. This instinct is almost always wrong.

The reference sites that shaped our aesthetic — Natsumi Nishizumi, Plum Village, Naval — share one defining characteristic: space. Section padding of 80 to 120 pixels. Gaps between post entries large enough to breathe between thoughts. A hero that occupies 70–80% of the viewport before anything else appears.

Whitespace does several things simultaneously:
- It **separates** — creating clear distinctions between sections
- It **emphasizes** — an element surrounded by space is automatically important
- It **rests** — it gives the eye a pause between demands
- It **signals quality** — scarcity, not abundance, conveys value

**原则7：留白不是空洞，而是节奏、休息和强调。不确定时，把内边距翻倍。**

**Principle 7: Whitespace is not emptiness — it is rhythm, rest, and emphasis. When uncertain, double your padding.**

---

## V. 布局：容器的逻辑 / Layout: The Logic of Containers

### 容器与自由 / The Container and the Wild

Most content lives inside a constrained max-width container — ours is `1100px`. This maintains readable line lengths and consistent alignment. But certain elements should break free entirely:

- **The hero section** spans 100% of the viewport — it is a *moment*, not content
- **The closing CTA section** uses full-bleed background — signaling "this is a different kind of statement"
- **Code blocks and blockquotes** sometimes extend slightly wider than body text — giving them visual distinction

**原则8：建立内容容器，然后在关键时刻有意识地打破它。**

**Principle 8: Establish a content container. Then deliberately break it at high-impact moments.**

### 卡片 vs 编辑式排列 / Cards vs Editorial Strips

The fundamental layout decision for a content-driven site: **cards** (bordered boxes in a grid) or **editorial strips** (rows divided by horizontal rules)?

- **Cards** belong to apps. They frame self-contained interactive objects. They suggest: "each item is a product."
- **Editorial strips** belong to magazines. They create reading rhythm. They suggest: "these items are part of a sequence worth following."

A personal blog is closer to a magazine than an app. When we replaced bordered card boxes with open editorial strips — separated only by a thin horizontal rule — the entire page felt more literary, more considered, more honest.

**原则9：让布局匹配内容的本质。博客和新闻适合编辑式排列；产品和作品集适合卡片布局。**

**Principle 9: Match your layout to the nature of your content. Blogs and news are editorial. Products and portfolios are card-based.**

### 每个页面只做一件事 / Each Page Has One Job

Every page on a site plays a specific role in the reader's journey:

| Page | Role | Analogy |
|---|---|---|
| **Homepage** | Magazine cover | Draws you in with the best recent work |
| **Blog list** | Library index | All content, browsable, filterable by tag |
| **Single post** | The article | Focused, distraction-free reading |
| **About** | The author's voice | Personal, narrative, trust-building |

When pages try to serve two roles simultaneously — a homepage that is also a full blog archive — they serve neither well. The homepage shows *glimpses*: title and date, nothing more. The blog list shows *context*: description and tags. The single post shows *everything*.

**原则10：每个页面只做一件事。设计的每个元素都应服务于那一件事，而非其他。**

**Principle 10: Give each page one job. Every design element should serve that job and nothing else.**

---

## VI. 首屏：一个承诺 / The Hero: A Promise

The hero section is the first thing a visitor sees. It has one job: make a promise. It says — wordlessly, through image and scale and typography — "here is what this place is about, here is the mood, here is whether you belong here."

Key decisions inside a hero:

**1. 图片遮罩 / Overlay opacity**: The dark overlay over a hero image serves two purposes — ensuring text legibility, and shifting the emotional register. `rgba(0, 12, 6, 0.48)` was our formula. Too dark loses the photograph; too light loses the words.

**2. 背景定位 / Background-position**: A portrait photo's meaningful content is rarely at the geometric center. `background-position: center 55%` showed the lotus flower rather than the blurry sky above it. Always ask: what is the *subject* of this image, and where is it in the frame?

**3. 高度 / Height**: A hero at `80vh` is an entrance — you are immersed before you scroll. At `50vh`, it is merely a header. The difference is psychological, not mathematical.

**4. 文字层级 / Text hierarchy**: One large italic serif headline. One small uppercase subtitle. Nothing else. The hero is not a place for information — it is a place for feeling.

**原则11：首屏是一个承诺。图片、遮罩、文字、高度——每一个决定都应服务于那个承诺，而不是展示技巧。**

**Principle 11: The hero is a promise. Image, overlay, text, scale — every decision must serve that promise, not demonstrate skill.**

---

## VII. 导航：安静即力量 / Navigation: Quiet Is Strength

Both Natsumi Nishizumi and Naval share a navigation philosophy: almost invisible, completely functional. No dropdowns. No mega-menus. No icons. Just text links, quietly arranged.

Our navigation pattern — brand name centered, links split left and right — is a classic editorial structure. It signals: "the content is the point." The navigation is infrastructure, not a feature.

Key navigation principles:
- **Flat**: Never more than one level. Dropdowns add complexity that rarely earns its cost.
- **Sparse**: Fewer links means each link carries more weight. If everything is reachable, nothing feels important.
- **Uppercase + letter-spacing**: Makes links feel like wayfinding signs, not content.
- **Sticky**: Stays visible as the user scrolls. Orientation is a courtesy.

**原则12：导航应该是安静的向导，而不是一个值得注意的功能。如果你的导航感觉很显眼，它就太显眼了。**

**Principle 12: Navigation should be a quiet guide, not a notable feature. If your navigation feels prominent, it is too prominent.**

---

## VIII. 以声明结束 / End with a Statement

Natsumi Nishizumi's closing section — "Start a Conversation" — is a masterclass in what happens at the bottom of a page. It is:

- **Full-bleed**: breaks the container, claiming the full width of the screen
- **Large type**: a single serif headline at `4rem`, taking space without apology
- **One idea**: not a footer menu — one thought, one action
- **An invitation**: email address in quiet uppercase below the headline

Most beginner sites let their pages trail off into a footer: copyright notice, small links, silence. This is a missed opportunity. The bottom of a page is the second most important design moment — after the opening. A visitor who reaches the bottom has given you their time and attention. Meet them there with something worth finding.

**原则13：用一个声明结束每一个重要页面。全宽、大字、一个想法、一个行动。不要让页面悄悄消失在版权声明里。**

**Principle 13: End every key page with a statement. Full-bleed, large type, one idea, one action. Do not let the page quietly disappear into a copyright notice.**

---

## IX. 从参考中学习 / Learning from References

The most efficient design education is not reading about design. It is studying specific sites that produce a specific feeling — and then asking, with precision: *why* does this work?

The three references that shaped this site:

**Natsumi Nishizumi** — taught us alternating section backgrounds, mixing italic and roman type within a single headline, and the principle of the statement closing. What feeling? *Refined simplicity.* What decision produced it? Sections separated by background color instead of borders — the page breathes without hard edges.

**Plum Village** — taught us image-led editorial grids, warm off-white backgrounds, and the power of a full-bleed dark closing section. What feeling? *Warm, trustworthy, unhurried.* What decision produced it? Images that fill their grid cell completely, with no border or shadow — the content defines its own boundary.

**Naval** — taught us that a title needs nothing more than itself. What feeling? *Confident quietness.* What decision produced it? The complete removal of description, date formatting, borders, and hover effects. What remains is the title. Nothing fights with it.

**How to study a reference site:**
1. Name the feeling it creates, precisely
2. Identify the single design decision most responsible for that feeling
3. Understand *why* it works — psychological, typographic, or structural
4. Adapt the *principle*, not the surface

**原则14：建立参考库。带着"为什么有效？"这个问题研究它们。学习原则，而不是模仿表面。**

**Principle 14: Build a reference library. Study each site with the question: *why* does this work? Learn the principle, not the surface.**

---

## X. 错误即课程 / Mistakes Are the Curriculum

Web design is not planned — it is discovered. The principles above were not deduced in advance. They emerged from failures:

- Posts **disappeared** after dates were changed → learned that Hugo skips future-dated content in UTC, and `--buildFuture` was the fix
- The blog list **showed full article content** → learned that Hugo's `.Summary` pulls too much from HTML-heavy bilingual text; explicit `description` frontmatter is the clean solution
- The hero **showed blurry sky** instead of the lotus → learned that `background-position: center 35%` shows the top of a portrait image; `center 55%` shows the flower
- Two GitHub Actions workflows **competed** and overwrote each other → learned that concurrent deployments race, and the last to finish wins
- **Underlines appeared everywhere** → learned that wrapping an `<ol>` list item's entire content in `<a>` applies browser-default link underlines to everything; the fix is to only wrap the clickable text
- **Card borders felt app-like** → learned that editorial strips (top-border only, no box) create a literary feeling that bordered cards cannot

Each failure revealed something true about how the web actually works. The mistakes were not detours from the learning — they *were* the learning.

**原则15：设计是迭代的。错误不是弯路，而是路本身。快速构建，看到渲染结果，然后调整。从未犯错的设计师，要么从未构建，要么从未看见。**

**Principle 15: Design is iterative. Mistakes are not detours — they are the road. Build fast, see it rendered, and adjust. A designer who has never made these mistakes has either never built anything or never looked.**

---

## 尾声：设计究竟是什么 / Closing: What Design Actually Is

After going through this process — choosing colors, fixing bugs, studying reference sites, debating whether a card border should exist, moving pixels by fractions of a rem — a definition of design becomes clear:

**Design is the arrangement of decisions that shape how a person experiences something.**

It is not about beauty, though beauty is a byproduct when it is done well. It is not about aesthetics, though taste is the instrument. It is about shaping *experience* — directing attention, creating rhythm, building trust, making promises, and fulfilling them quietly.

The best design disappears. The visitor never thinks "what a thoughtful font choice" — they simply find reading easy. They never consciously appreciate the spacing — they simply feel that the page is calm. They never analyze the palette — they simply feel at home.

*好的设计会消失。访客不会想"这字体选得真好"——他们只是觉得阅读很自然。他们不会注意到间距——他们只是觉得页面很安静。他们不会欣赏色板——他们只是感到如归自家。*

That is the goal. Not to be noticed. To be *felt*.

---

*这些原则来自一个没有设计背景的寿险顾问，在搭建个人网站的过程中，通过真实的错误和真实的参考，一条一条发现的。它们不是普世真理——而是在特定条件下发现的，有效的东西。*

*These principles were discovered by a life insurance agent with no design background, building a personal website through real mistakes and real references, one at a time. They are not universal truths — they are things found to work, under specific conditions, earned through the process of actually making something.*

<p class="post-revised">Published: Mar 26, 2026, 22:00</p>
