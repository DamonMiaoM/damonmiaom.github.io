---
title: "Building a Word-to-PPT Transformer: A Developer's Journal"
date: 2026-03-26T18:00:00
---

**Project:** AI-powered Web Tool — Convert Word exam papers to PowerPoint slides
**Live URL:** [damonmiaom.github.io/Word_To_PPT](https://damonmiaom.github.io/Word_To_PPT)
**GitHub:** [github.com/DamonMiaoM/Word_To_PPT](https://github.com/DamonMiaoM/Word_To_PPT)

---

## Background

As a life insurance agent who also tutors children in mathematics, I often work with exam papers in Word format. Presenting questions to students one-by-one on screen — rather than scrolling through a dense document — makes lessons much more engaging. I wanted a tool that could automatically convert a Word exam paper into a clean, presentation-ready PowerPoint file.

The original project was a single `index.html` file using three JavaScript libraries: **JSZip** (to unpack the `.docx` format), **Mammoth** (for Word parsing), and **PptxGenJS** (to generate `.pptx` output). It worked, but barely. This journal documents the full journey of turning that rough prototype into something I'm genuinely proud of.

---

## Phase 1 — Getting the Project Online

### Problem: The project only existed as a local `.zip` file

The starting point was a WeChat-received file: `wordToPPT.zip`. It had never been versioned or published anywhere.

### What I did

1. Extracted the zip, copied all files into `~/Projects/wordToPPT/`
2. Created a `.gitignore` to exclude macOS system files (`.DS_Store`, `__MACOSX/`)
3. Initialized a Git repository and made the first commit
4. Created the GitHub repository `DamonMiaoM/Word_To_PPT` and pushed

### Enabling GitHub Pages

After pushing, I enabled GitHub Pages (Settings → Pages → Source: `main` branch). Within two minutes, the tool was live at a public URL — no server, no cost, accessible from any device.

**Key lesson:** A pure front-end tool (HTML + JS, no backend) is perfect for GitHub Pages. Anyone can use it by just opening a link.

---

## Phase 2 — Bug Fixes

### Bug 1: Drag-and-drop downloaded the file instead of uploading it

**Symptom:** When a user dragged a `.docx` file onto the upload zone, the browser would download it instead of processing it.

**Root cause:** The upload zone had an `onclick` handler but zero drag-and-drop event listeners. Browsers handle dragged files with a default behavior: navigate to or download the file. Without `preventDefault()`, that default behavior wins.

**Fix:** Added three event listeners to the upload zone:
- `dragover` — calls `preventDefault()` to block the browser default; also highlights the zone visually
- `dragleave` — resets the visual highlight
- `drop` — calls `preventDefault()` and passes the dropped file to the processing function

Also extracted the file-handling logic into a shared `handleFile(file)` function so both click-upload and drag-and-drop use the same code path.

---

## Phase 3 — Layout and Visual Redesign

### Problem: Exported slides had poor typography and broken layouts

The original output used a dark blue header bar, 28pt bold text for everything, and a fixed-height title box (1.2 inches). Long question titles would overflow their box and visually collide with the answer options below.

### Iteration 1 — Basic layout fixes

- Increased title box height from 1.2" to 3.2"
- Reduced font size dynamically based on title length (20pt for short titles, down to 16pt for very long ones)
- Enabled `autoFit: true` so PptxGenJS would shrink text that still overflowed
- Moved the answer options block downward to prevent overlap

### Iteration 2 — Redesign to match a reference layout

The user shared a screenshot of a previous version they loved. Key design features observed:

- **Light blue header bar** instead of dark blue
- **"第 X 题" format** in the header (instead of the raw "1、" prefix from the document)
- **Category label** in the top-right corner (e.g., "— 选择题")
- **Thin separator line** under the header
- **Slide number** in the bottom-left corner

Implemented all of these. Also added logic to extract the question number from the title text so the header always displays "第 1 题", "第 2 题", etc. in a consistent format.

### Iteration 3 — User-specified typography

The user specified: **Microsoft YaHei (微软雅黑), 28pt, bold for question stems; same font and size but not bold for answer options.**

Implemented using PptxGenJS's rich text array format:
```javascript
[
  { text: title, options: { fontFace: '微软雅黑', fontSize: 28, bold: true } },
  { text: options, options: { fontFace: '微软雅黑', fontSize: 28, bold: false } }
]
```

For slides without images, combined the title and options into a **single text block** so options flow naturally directly below the question — no awkward white gap in the middle of the slide.

For short answer options (A/B/C/D all under ~52 characters total), options are displayed **horizontally on one line**: `A.266    B.304    C.342    D.380` — matching how they appear in the original Word document.

---

## Phase 4 — Question Parsing Accuracy

This was the hardest phase. Getting "one question = one slide" right required solving several interconnected problems.

### Problem 1: Two questions merged onto one slide

**Symptom:** Slide 3 showed both question 3 and question 4 squeezed together.

**Root cause (A):** The original regex only recognized questions starting with a number prefix (`1、`, `2.`, etc.). Question 4 in the source document used Word's **auto-numbered list** feature, meaning the "4" was generated by Word's list engine and **does not appear in the raw XML text**. The paragraph text started directly with "在6×6的方格表中摆放..." — no number, invisible to the regex.

**Root cause (B):** The auto-split fallback (designed to detect question boundaries by looking for completed A/B/C/D options) was checking for `D.` at the **start of a line**. But in this document, all four options were on a single line: `A.15,5,25  B.15,25,5  C.5,15,25  D.25,15,5`. So `D.` was in the middle of a line — the check always failed.

**Fix:** Changed the `D.` detection to search anywhere in the body text (not just at line start):
```javascript
const seenAllOpts = currentSlide && /D[\.、]/.test(bodyStr);
```
After detecting that all options are present, the next substantive paragraph (text or inline image) automatically triggers a new slide.

Also added a **title-promotion mechanism**: if a new slide is created without a title (because the question has no number prefix), the first non-option paragraph is promoted to become the title.

### Problem 2: Empty slides for section headers

**Symptom:** "二、计算。" and "三、填空。" each generated their own blank slide.

**Root cause:** The question regex matched both Arabic numerals (`1、`) AND Chinese numerals (`二、`). Section headers like "二、计算题" use Chinese numerals — they were being treated as questions.

**Fix:** Split into two separate regexes:
- `sectionRegex` — matches Chinese numeral headers (`一、`, `二、`, `三、`…) → updates the section label, does NOT create a slide
- `questionRegex` — matches Arabic numeral questions (`1.`, `2、`…) → creates a slide

The section label is parsed and stored: "计算" → "计算题", "填空" → "填空题", "选择" → "选择题", etc.

### Problem 3: Static "选择题" label on all slides

**Symptom:** Every slide showed "— 选择题" in the top-right, even calculation and fill-in-the-blank questions.

**Root cause:** The label was hardcoded as a string literal in the rendering code.

**Fix:** Each slide now stores a `section` property set during parsing. The renderer reads `s.section` dynamically:
```javascript
slide.addText(`— ${s.section || '解答题'}`, { ... });
```

---

## Phase 5 — Complex Multi-Part Questions

### Problem: A question with 3 images and 2 sub-questions was unreadable

**Symptom:** Question 6 (a multi-part geometry problem with a watch diagram, three reference figures, and two sub-questions) was crammed onto one slide. Only the last image was shown; the text was partially cut off.

**Analysis:** This question had a fundamentally different structure from simple multiple-choice questions:
- A shared stem with multiple reference images (图1, 图2, 图3)
- Sub-question (1): a fill-in-the-blank
- Sub-question (2): two further sub-parts (① and ②)

No single slide can reasonably present all of this.

**Solution — Two parts:**

**Part A: Multi-image grid layout**

Previously only `s.images[0]` was ever rendered. Changed to support up to 4 images:
- 1 image: full right column
- 2 images: right column split into two rows
- 3–4 images: 2×2 grid in the right column

**Part B: Automatic sub-question splitting**

Added a post-processing step after parsing. When a question has ≥ 2 images AND contains `(1)` / `(2)` sub-question markers in its body text:

1. A **stem slide** is created with all images and the shared question text
2. Each sub-question `(1)`, `(2)` gets its own **full-width slide**, labeled "第X题 · 续"

```javascript
finalSlides = finalSlides.flatMap(s => {
    if (s.images.length < 2) return [s];
    const subStarts = /* find (1), (2) markers */;
    if (subStarts.length < 2) return [s];
    return [stemSlide, ...subQuestionSlides];
});
```

---

## Phase 6 — Image Quality (The Hardest Bug)

### Problem: Images were visibly stretched and blurry

**Symptom:** Despite using `sizing: { type: 'contain' }` in PptxGenJS, images still appeared stretched — the aspect ratio was clearly wrong.

**Investigation:** PptxGenJS's `contain` mode is designed to scale an image to fit within a bounding box while preserving the aspect ratio. But for base64-encoded images (which is how embedded Word images arrive), PptxGenJS **cannot determine the original pixel dimensions**. Without knowing the original width and height, it cannot calculate the correct scaling — so it silently falls back to stretching the image to fill the box.

**Fix:** Replaced `sizing: contain` entirely with a manual calculation:

```javascript
// Read actual pixel dimensions using the browser's Image API
const getAspect = (dataUrl) => new Promise(res => {
    const img = new Image();
    img.onload  = () => res(img.naturalWidth / img.naturalHeight);
    img.onerror = () => res(1);
    img.src = dataUrl;
});

// Calculate contain dimensions manually
const addImg = async (slide, data, bx, by, bw, bh) => {
    const ar = await getAspect(data);
    let w, h;
    if (ar >= bw / bh) { w = bw; h = bw / ar; }
    else               { h = bh; w = bh * ar; }
    // Center within the bounding box
    slide.addImage({ data, x: bx + (bw - w) / 2, y: by + (bh - h) / 2, w, h });
};
```

The export handler was converted from synchronous to `async/await` to support this pattern. The result: images now display at their true aspect ratio with no distortion.

**Remaining limitation:** If the source image embedded in the Word document is low-resolution (e.g., 150×150px), displaying it at 4–5 inches on screen will appear blurry regardless of aspect ratio handling. This is a source-data limitation, not something the tool can fix.

---

## Summary of All Changes

| # | Problem | Root Cause | Fix |
|---|---------|-----------|-----|
| 1 | Drag-and-drop downloaded file | No `dragover`/`drop` event listeners | Added event listeners with `preventDefault()` |
| 2 | Layout overflow, title overlaps options | Fixed 1.2" title box, 28pt font | Larger box, dynamic font size, `autoFit` |
| 3 | Visual design didn't match reference | Dark header, no question number format | Light blue header, "第X题" format, dynamic label |
| 4 | Two questions on one slide | Word auto-numbering hides "4、" from XML; `D.` check required line-start | Auto-split after detecting `D.` anywhere in body |
| 5 | Empty slides for section headers | Chinese numeral regex same as Arabic | Separate `sectionRegex` (no slide) vs `questionRegex` (slide) |
| 6 | "选择题" label static on all slides | Hardcoded string | `s.section` property set during parsing, read during render |
| 7 | Complex multi-part question unreadable | Single slide, only first image shown | Post-processing splits on `(1)`/`(2)`; multi-image grid |
| 8 | Images stretched/blurry | PptxGenJS `contain` can't read base64 dimensions | Manual aspect-ratio calculation via `Image.naturalWidth/Height` |

---

## Technical Architecture

The final tool is a **single HTML file** with zero build steps and zero server requirements:

```
index.html
├── jszip.min.js          — Unpack .docx (which is a ZIP archive)
├── mammoth.browser.min.js — (included but unused in final version)
└── pptxgen.bundle.js     — Generate .pptx output
```

**Parsing pipeline:**
1. Load `.docx` as ArrayBuffer via FileReader
2. Unzip with JSZip → extract `word/document.xml` and `word/media/*`
3. Build image map: filename → base64 data URL
4. Build relationship map: image ID → filename
5. Walk `<w:p>` paragraphs → classify as section header / question / body / option / image
6. Post-process: split complex multi-part questions

**Export pipeline:**
1. For each slide, build header elements (shape + text)
2. Build rich text parts (title bold + options normal)
3. For each image: read pixel dimensions → calculate contain fit → place centered
4. Write `.pptx` via PptxGenJS async API

---

## What I Learned

**As someone new to coding**, this project taught me several things I didn't expect:

1. **File formats are just ZIP files.** A `.docx` is literally a ZIP archive containing XML files. Once you unzip it, the document is just text you can parse.

2. **Browser APIs are powerful.** Loading images, reading pixel dimensions, dragging and dropping files — all of this can be done in pure JavaScript running in a browser tab, with no installation required.

3. **"It works on my machine" is just the beginning.** The tool looked fine in my test, but real exam papers exposed edge cases I never anticipated: questions with no number prefix, options on one line, multi-part structures, low-resolution embedded images.

4. **Iterating is how software gets good.** Each screenshot I shared revealed a new problem. Each problem had a specific root cause. Fixing root causes — not symptoms — is what makes the improvements stick.

5. **AI as a collaborator.** I built this entire tool through conversation with Claude Code. I described what I saw, shared screenshots, explained what I wanted — and we debugged and improved it together. The AI wrote the code; I supplied the domain knowledge (what a good exam slide should look like) and the test cases (real exam papers).

---

## What's Next

- Support for `.doc` (older Word format) — currently only `.docx` works
- Better handling of questions where the number is embedded as a Word list style
- Option to customize color scheme and font before export
- Mobile-friendly upload interface

---

*This journal was written in March 2026. The tool is free and open source.*
*GitHub: [github.com/DamonMiaoM/Word_To_PPT](https://github.com/DamonMiaoM/Word_To_PPT)*
