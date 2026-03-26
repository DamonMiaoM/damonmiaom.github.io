---
title: "Building a Word-to-PPT Transformer: A Developer's Journal"
date: 2026-03-26T18:00:00
description: "How a life insurance agent used AI to turn a Word exam paper into a clean PowerPoint deck — six phases, eight bugs, and everything learned along the way."
tags: ["AI", "Tools", "Project"]
---

*一个寿险顾问，如何用 AI 把一张 Word 试卷变成一套漂亮的 PPT——六个阶段、八个 Bug、以及一路走来的收获。*

*How a life insurance agent used AI to turn a Word exam paper into a clean PowerPoint deck — six phases, eight bugs, and everything learned along the way.*

**工具地址 / Live URL:** [damonmiaom.github.io/Word_To_PPT](https://damonmiaom.github.io/Word_To_PPT)
**源代码 / GitHub:** [github.com/DamonMiaoM/Word_To_PPT](https://github.com/DamonMiaoM/Word_To_PPT)

---

## 背景
<p class="heading-note">Background</p>

我是一名寿险顾问，同时也在给孩子们做数学辅导。上课时，我发现把试卷上的题目一道一道地展示在屏幕上，比让学生盯着一整张密密麻麻的 Word 文档要好得多。于是我想：能不能做一个工具，自动把 Word 试卷转换成演示用的 PowerPoint？

*I'm a life insurance agent who also tutors children in mathematics. In class, I found that showing questions one at a time on screen — rather than scrolling through a dense Word document — made lessons far more engaging. So I asked: could I build a tool to automatically convert a Word exam paper into presentation-ready PowerPoint slides?*

起点是一个从微信收到的文件：`wordToPPT.zip`。它包含一个 `index.html`，使用了三个 JavaScript 库：**JSZip**（解压 `.docx` 文件）、**Mammoth**（解析 Word 内容）和 **PptxGenJS**（生成 `.pptx` 输出）。它能跑，但仅此而已。本文记录了把这个粗糙原型打磨成我真正引以为豪的工具的全过程。

*The starting point was a file received via WeChat: `wordToPPT.zip`. It contained a single `index.html` using three JavaScript libraries: **JSZip** (to unpack `.docx` files), **Mammoth** (to parse Word content), and **PptxGenJS** (to generate `.pptx` output). It ran, but barely. This journal documents the full journey of turning that rough prototype into something I'm genuinely proud of.*

---

## 第一阶段——让项目上线
<p class="heading-note">Phase 1 — Getting the Project Online</p>

**问题：** 项目只存在于本地的一个 `.zip` 文件里，从未被版本化或发布过。

*The project only existed as a local `.zip` file and had never been versioned or published anywhere.*

**做了什么：**

*What I did:*

1. 解压文件，复制到 `~/Projects/wordToPPT/`
2. 创建 `.gitignore`，排除 macOS 系统文件（`.DS_Store`、`__MACOSX/`）
3. 初始化 Git 仓库，完成第一次提交
4. 在 GitHub 创建仓库 `DamonMiaoM/Word_To_PPT`，推送代码

*1. Extracted the zip into `~/Projects/wordToPPT/`. 2. Created `.gitignore` to exclude macOS system files. 3. Initialized a Git repository and made the first commit. 4. Created the GitHub repository and pushed.*

开启 GitHub Pages（Settings → Pages → Source: `main` 分支）后，不到两分钟，工具就上线了——无需服务器，零成本，任何设备打开链接即可使用。

*After enabling GitHub Pages, the tool was live in under two minutes — no server, no cost, accessible from any device via a public URL.*

**关键认知：** 纯前端工具（HTML + JS，无后端）天然适合 GitHub Pages。只需分享一个链接，任何人都能用。

***Key lesson:** A pure front-end tool is perfect for GitHub Pages. Share a link — anyone can use it instantly.*

---

## 第二阶段——修复 Bug
<p class="heading-note">Phase 2 — Bug Fixes</p>

### Bug：拖拽文件时浏览器直接下载，而非上传
<p class="heading-note">Bug: Drag-and-drop downloaded the file instead of uploading it</p>

**现象：** 把 `.docx` 文件拖入上传区，浏览器会直接下载它，而不是处理它。

*When dragging a `.docx` file onto the upload zone, the browser would download it instead of processing it.*

**根本原因：** 上传区只有 `onclick` 事件，完全没有拖拽事件监听器。浏览器遇到被拖入的文件，默认行为就是下载或导航到该文件。没有 `preventDefault()`，默认行为就会生效。

*Root cause: The upload zone had an `onclick` handler but zero drag-and-drop event listeners. Without `preventDefault()`, the browser's default behavior — download the file — takes over.*

**修复：** 为上传区新增三个事件监听器：
- `dragover` — 调用 `preventDefault()` 阻止浏览器默认行为，同时高亮显示上传区
- `dragleave` — 取消高亮
- `drop` — 阻止默认行为，将文件交给处理函数

*Fix: Added three event listeners — `dragover` (prevent default + visual highlight), `dragleave` (reset highlight), `drop` (prevent default + pass file to handler). Also extracted file handling into a shared `handleFile()` function used by both click and drag-and-drop.*

---

## 第三阶段——布局与视觉重设计
<p class="heading-note">Phase 3 — Layout and Visual Redesign</p>

**问题：** 导出的幻灯片排版很差，题目标题框只有 1.2 英寸高，28pt 字号太大，长题目会溢出并压住下方的选项。

*The exported slides had poor typography. The fixed 1.2-inch title box combined with 28pt font meant long questions overflowed and collided with the answer options below.*

经过三轮迭代，最终实现了和参考版本一致的设计：

*After three rounds of iteration, the final design matched the reference:*

- **浅蓝色标题栏**，替代原来的深蓝色 / *Light blue header bar instead of dark blue*
- **"第 X 题"格式**，自动从题目文字中提取题号 / *"第 X 题" format, number auto-extracted from title text*
- **右上角题型标签**（选择题 / 计算题 / 填空题…） / *Category label in top-right corner*
- **标题栏下方细分隔线** / *Thin separator line under the header*
- **底部左下角页码** / *Slide number in the bottom-left corner*
- **微软雅黑 28pt**：题干加粗，选项不加粗 / *Microsoft YaHei 28pt: bold for stems, normal for options*
- **短选项水平排列**：`A.266    B.304    C.342    D.380` / *Short options displayed horizontally on one line*

---

## 第四阶段——题目解析准确性
<p class="heading-note">Phase 4 — Question Parsing Accuracy</p>

这是最难的阶段。实现"一题一页"需要解决几个相互关联的问题。

*This was the hardest phase. Getting "one question = one slide" required solving several interconnected problems.*

### 问题一：两道题被合并到同一页
<p class="heading-note">Problem 1: Two questions merged onto one slide</p>

**根本原因 A：** 原来的正则只识别以数字前缀开头的题目（`1、`、`2.` 等）。但第4题在 Word 里使用了**自动编号列表**功能，"4"是由 Word 的列表引擎自动生成的，**并不出现在原始 XML 文字中**。段落文字直接以"在6×6的方格表中摆放..."开头——没有数字，正则看不见。

*Root cause A: The regex only recognized questions with a number prefix. But question 4 used Word's auto-numbered list — the "4" is generated by Word's engine and doesn't appear in the raw XML text.*

**根本原因 B：** 用于检测"ABCD 选项已齐"的逻辑要求 `D.` 出现在行首。但这份文档里四个选项全在同一行：`A.15,5,25  B.15,25,5  C.5,15,25  D.25,15,5`，`D.` 在行的中间——检测永远失败。

*Root cause B: The "options complete" check required `D.` at the start of a line. But all four options were on one line, so `D.` was in the middle — the check always failed.*

**修复：** 将 `D.` 检测改为在 body 文字中任意位置匹配：

*Fix: Changed `D.` detection to match anywhere in the body text:*

```javascript
const seenAllOpts = currentSlide && /D[\.、]/.test(bodyStr);
```

检测到选项齐全后，下一段实质性内容（文字或图片）自动触发新建幻灯片。

*After detecting all options, the next substantive paragraph automatically triggers a new slide.*

### 问题二：大题标题生成了空白幻灯片
<p class="heading-note">Problem 2: Empty slides for section headers</p>

"二、计算。"和"三、填空。"各自生成了一张空白幻灯片，因为题目正则同时匹配了汉字序号和阿拉伯数字。

*"二、计算。" and "三、填空。" each generated a blank slide — the question regex matched both Chinese and Arabic numerals.*

**修复：** 拆成两个正则：`sectionRegex`（汉字序号 → 只更新题型标签，不建幻灯片）和 `questionRegex`（阿拉伯数字 → 建幻灯片）。

*Fix: Split into two regexes — `sectionRegex` (Chinese numerals → update label only, no slide) and `questionRegex` (Arabic numerals → create slide).*

### 问题三：所有幻灯片右上角永远显示"选择题"
<p class="heading-note">Problem 3: Static "选择题" label on every slide</p>

**修复：** 每个幻灯片对象存储 `section` 属性，渲染时动态读取，自动跟随所在大题类型变化。

*Fix: Each slide object stores a `section` property set during parsing. The renderer reads it dynamically — the label automatically matches the current section type.*

---

## 第五阶段——复杂多图多小题
<p class="heading-note">Phase 5 — Complex Multi-Part Questions</p>

第6题是一道带手表图解、三张参考图和两道小题的综合几何题，被全部压缩在一张幻灯片上。之前的代码还只显示第一张图片。

*Question 6 was a complex geometry problem with a watch diagram, three reference figures, and two sub-questions — all crammed onto one slide. The code also only showed the first image.*

**解决方案分两步：**

*Solution in two parts:*

**A. 多图网格布局：**
- 1 张图：右侧全高显示
- 2 张图：右侧上下竖排
- 3-4 张图：右侧 2×2 网格

*A. Multi-image grid: 1 image (full column), 2 images (stacked), 3-4 images (2×2 grid).*

**B. 自动拆分小题：** 当题目有 ≥ 2 张图片且含 `(1)(2)` 小题标记时，自动拆分为：主题干幻灯片（含全部图片）+ 每道小题独立一页（标注"第X题 · 续"）。

*B. Auto sub-question splitting: When a question has ≥ 2 images and contains `(1)(2)` markers, it automatically splits into a stem slide (with all images) plus one slide per sub-question (labeled "第X题 · 续").*

---

## 第六阶段——图片质量（最难的 Bug）
<p class="heading-note">Phase 6 — Image Quality (The Hardest Bug)</p>

**现象：** 尽管使用了 `sizing: { type: 'contain' }`，图片仍然被严重拉伸。

*Despite using `sizing: { type: 'contain' }` in PptxGenJS, images were visibly stretched.*

**调查结论：** PptxGenJS 的 `contain` 模式需要知道图片的原始像素尺寸才能计算缩放比例。但对于 base64 编码的图片（Word 内嵌图片就是这种格式），PptxGenJS **无法获取原始尺寸**，于是静默地退回到直接拉伸填满容器。

*Investigation: PptxGenJS's `contain` mode needs the original pixel dimensions to calculate scaling. But for base64-encoded images — which is how Word embeds them — PptxGenJS cannot determine the original size, so it silently falls back to stretching the image to fill the box.*

**修复：** 彻底放弃 `sizing: contain`，改用 JavaScript 的 `Image` API 手动读取原始尺寸，自行计算等比缩放：

*Fix: Replaced `sizing: contain` entirely with a manual calculation using the browser's `Image` API:*

```javascript
const getAspect = (dataUrl) => new Promise(res => {
    const img = new Image();
    img.onload = () => res(img.naturalWidth / img.naturalHeight);
    img.src = dataUrl;
});

const addImg = async (slide, data, bx, by, bw, bh) => {
    const ar = await getAspect(data);
    let w, h;
    if (ar >= bw / bh) { w = bw; h = bw / ar; }
    else               { h = bh; w = bh * ar; }
    slide.addImage({ data, x: bx + (bw - w) / 2, y: by + (bh - h) / 2, w, h });
};
```

导出函数同步改为 `async/await`，图片不再拉伸。

*The export handler was converted to `async/await`. Images now display at their true aspect ratio with no distortion.*

---

## 全部改动汇总
<p class="heading-note">Summary of All Changes</p>

| # | 问题 | 根本原因 | 修复方法 |
|---|------|---------|---------|
| 1 | 拖拽文件被下载 | 缺少 dragover/drop 事件 | 添加事件监听 + `preventDefault()` |
| 2 | 标题溢出压住选项 | 1.2" 固定高度 + 28pt 字号 | 更大的文字框 + 动态字号 + `autoFit` |
| 3 | 视觉设计不达标 | 深蓝色标题，无题号格式 | 浅蓝标题栏、第X题格式、动态标签 |
| 4 | 两题合并一页 | Word 自动编号不入 XML；D. 检测要求行首 | 在 body 任意位置检测 `D.` |
| 5 | 大题标题生成空白页 | 汉字正则与阿拉伯数字正则混用 | 拆成两个独立正则 |
| 6 | 题型标签静态 | 硬编码字符串 | `s.section` 属性动态渲染 |
| 7 | 复杂多图题无法阅读 | 只显示第一张图；全压一页 | 多图网格 + 自动拆分小题 |
| 8 | 图片拉伸模糊 | pptxgen contain 无法读 base64 尺寸 | 手动用 `Image.naturalWidth/Height` 计算 |

---

## 技术架构
<p class="heading-note">Technical Architecture</p>

最终工具是**一个 HTML 文件**，无需构建步骤，无需服务器：

*The final tool is a single HTML file — zero build steps, zero server requirements:*

```
index.html
├── jszip.min.js       — 解压 .docx（本质是 ZIP 压缩包）
├── pptxgen.bundle.js  — 生成 .pptx 输出
```

**解析流程：** 加载 `.docx` → JSZip 解压 → 提取 XML 和图片 → 逐段识别大题/小题/选项/图片 → 后处理（拆分复杂题）

**导出流程：** 逐页生成幻灯片 → 构建标题栏、富文本、图片 → 手动等比缩放图片 → PptxGenJS 异步写出 `.pptx`

*Parse pipeline: load `.docx` → unzip → extract XML and images → classify each paragraph → post-process complex questions. Export pipeline: build each slide → header, rich text, images with manual aspect-ratio calculation → write `.pptx` via async PptxGenJS API.*

---

## 我学到了什么
<p class="heading-note">What I Learned</p>

**作为一个编程新手**，这个项目教会了我几件没有预料到的事：

*As someone new to coding, this project taught me several things I didn't expect:*

1. **文件格式就是 ZIP 文件。** `.docx` 本质上是一个 ZIP 压缩包，里面装着 XML 文件。解压之后，文档就是可以解析的普通文本。 / *A `.docx` is literally a ZIP archive containing XML files. Once unzipped, the document is just text you can parse.*

2. **浏览器 API 很强大。** 加载图片、读取像素尺寸、拖放文件——这些全都可以在浏览器里用纯 JavaScript 完成，无需安装任何东西。 / *Loading images, reading pixel dimensions, drag-and-drop — all possible in pure JavaScript, no installation needed.*

3. **"在我机器上能跑"只是开始。** 工具在测试时看起来没问题，但真实试卷暴露出我从未预料到的边界情况：没有题号前缀的题目、选项在同一行、多图多小题结构、低分辨率嵌入图片。 / *"It works on my machine" is just the beginning. Real exam papers exposed edge cases I never anticipated.*

4. **迭代才能让软件变好。** 每张截图都揭示了一个新问题，每个问题都有具体的根本原因。解决根本原因——而不是症状——才能让改进真正持久。 / *Each screenshot revealed a new problem with a specific root cause. Fixing root causes — not symptoms — is what makes improvements stick.*

5. **AI 作为协作者。** 这个工具完全是通过和 Claude Code 对话构建的。我描述我看到的问题，分享截图，解释我想要什么——AI 写代码，我提供领域知识（一张好的试卷幻灯片应该是什么样子）和测试用例（真实的试卷）。 / *I built this entire tool through conversation with Claude Code. The AI wrote the code; I supplied the domain knowledge and the real test cases.*

---

## 下一步
<p class="heading-note">What's Next</p>

- 支持 `.doc`（旧版 Word 格式）—— 目前只支持 `.docx` / *Support for `.doc` (older Word format)*
- 更好地处理 Word 列表样式题号（题号不在 XML 文字中的情况） / *Better handling of Word auto-numbered questions*
- 导出前可自定义配色和字体 / *Option to customize color scheme and font before export*
- 移动端友好的上传界面 / *Mobile-friendly upload interface*

---

*本文写于 2026 年 3 月。工具免费开源。/ This journal was written in March 2026. The tool is free and open source.*
*GitHub: [github.com/DamonMiaoM/Word_To_PPT](https://github.com/DamonMiaoM/Word_To_PPT)*
