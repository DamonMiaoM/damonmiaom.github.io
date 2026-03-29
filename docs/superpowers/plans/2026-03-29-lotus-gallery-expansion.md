# Lotus Gallery Expansion Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Expand the Gallery page from 3 images to 17 lotus/water lily images with clean filenames, bilingual poetic captions, lazy loading, and a refreshed page description.

**Architecture:** Rename 3 existing static images, copy 14 new images with clean names, then rewrite `content/gallery.md` with all 17 entries — updated front matter, `loading="lazy"` on every `<img>`, descriptive alt text, and bilingual (EN/ZH) captions.

**Tech Stack:** Hugo static site, plain HTML in Markdown content file, bash for file operations.

**Spec:** `docs/superpowers/specs/2026-03-29-lotus-gallery-design.md`

---

## File Map

| Action | Path | Purpose |
|---|---|---|
| Rename | `static/images/lotus.jpg` → `lotus-pink-saffu.jpg` | Clean name for existing image |
| Rename | `static/images/waterlily.jpg` → `lotus-white-nong.jpg` | Clean name for existing image |
| Rename | `static/images/ripple.jpg` → `lotus-ripple-vitalyk.jpg` | Clean name for existing image |
| Copy ×14 | `/Users/Damon/Personal Website/Images/` → `static/images/` | New images with clean names |
| Modify | `content/gallery.md` | Updated front matter + full 17-image grid |

---

## Task 1: Rename 3 Existing Images

**Files:**
- Modify: `static/images/lotus.jpg` → `static/images/lotus-pink-saffu.jpg`
- Modify: `static/images/waterlily.jpg` → `static/images/lotus-white-nong.jpg`
- Modify: `static/images/ripple.jpg` → `static/images/lotus-ripple-vitalyk.jpg`

- [ ] **Step 1: Rename all three files**

```bash
cd /Users/Damon/Projects/damonmiaom.github.io/static/images
mv lotus.jpg lotus-pink-saffu.jpg
mv waterlily.jpg lotus-white-nong.jpg
mv ripple.jpg lotus-ripple-vitalyk.jpg
```

- [ ] **Step 2: Verify the renames**

```bash
ls /Users/Damon/Projects/damonmiaom.github.io/static/images/lotus*.jpg
```

Expected output:
```
lotus-blue-drop-skyler.jpg  (not yet — only the renamed 3 exist)
lotus-pink-saffu.jpg
lotus-ripple-vitalyk.jpg
lotus-white-nong.jpg
```
(Exact output will show just those 3 at this point — no error messages.)

- [ ] **Step 3: Commit**

```bash
cd /Users/Damon/Projects/damonmiaom.github.io
git add static/images/
git commit -m "refactor: rename 3 existing gallery images to clean descriptive names"
```

---

## Task 2: Copy 14 New Images

**Files:**
- Create: `static/images/lotus-red-zoltan.jpg`
- Create: `static/images/lotus-white-pads-cheng.jpg`
- Create: `static/images/lotus-lavender-yoksel.jpg`
- Create: `static/images/lotus-cream-star-ohi.jpg`
- Create: `static/images/lotus-bokeh-zoltan.jpg`
- Create: `static/images/lotus-blush-chris.jpg`
- Create: `static/images/lotus-soft-pink-jason.jpg`
- Create: `static/images/lotus-white-sun-ed.jpg`
- Create: `static/images/lotus-pink-reflect-wang.jpg`
- Create: `static/images/lotus-open-arto.jpg`
- Create: `static/images/lotus-white-mirror-shana.jpg`
- Create: `static/images/lotus-blue-drop-skyler.jpg`
- Create: `static/images/lotus-peach-shreyaan.jpg`
- Create: `static/images/lotus-white-shadow-nong.jpg`

- [ ] **Step 1: Copy all 14 images with clean names**

```bash
SRC="/Users/Damon/Personal Website/Images"
DEST="/Users/Damon/Projects/damonmiaom.github.io/static/images"

cp "$SRC/zoltan-tasi-yanhwFwyoaU-unsplash.jpg"        "$DEST/lotus-red-zoltan.jpg"
cp "$SRC/cheng-feng-BP-Q-Z0Ua9Y-unsplash.jpg"         "$DEST/lotus-white-pads-cheng.jpg"
cp "$SRC/yoksel-zok-bh0ai10Cxc4-unsplash.jpg"         "$DEST/lotus-lavender-yoksel.jpg"
cp "$SRC/ohi-ozoya-pQD0K7EOoQA-unsplash.jpg"          "$DEST/lotus-cream-star-ohi.jpg"
cp "$SRC/zoltan-tasi-vHnVtLK8rCc-unsplash.jpg"        "$DEST/lotus-bokeh-zoltan.jpg"
cp "$SRC/chris-wong-GEdrQkuF7iE-unsplash.jpg"         "$DEST/lotus-blush-chris.jpg"
cp "$SRC/jason-leung-B14YYbBZnT4-unsplash.jpg"        "$DEST/lotus-soft-pink-jason.jpg"
cp "$SRC/ed-stone-olusucJ05eM-unsplash.jpg"           "$DEST/lotus-white-sun-ed.jpg"
cp "$SRC/wang-binghua-qfE8Qqbkmn0-unsplash.jpg"       "$DEST/lotus-pink-reflect-wang.jpg"
cp "$SRC/arto-suraj-LpGFOOln8tk-unsplash.jpg"         "$DEST/lotus-open-arto.jpg"
cp "$SRC/shana-van-roosbroek-xeH1hzOyoT0-unsplash.jpg" "$DEST/lotus-white-mirror-shana.jpg"
cp "$SRC/skyler-ewing-9A-Z4nHrIZk-unsplash.jpg"       "$DEST/lotus-blue-drop-skyler.jpg"
cp "$SRC/shreyaan-vashishtha-tFWKWMs-95E-unsplash.jpg" "$DEST/lotus-peach-shreyaan.jpg"
cp "$SRC/nong-Hqa-Ymae0k0-unsplash.jpg"               "$DEST/lotus-white-shadow-nong.jpg"
```

- [ ] **Step 2: Verify all 17 lotus images are present**

```bash
ls /Users/Damon/Projects/damonmiaom.github.io/static/images/lotus-*.jpg | wc -l
```

Expected output: `17`

- [ ] **Step 3: Commit**

```bash
cd /Users/Damon/Projects/damonmiaom.github.io
git add static/images/
git commit -m "feat: add 14 new lotus images with clean descriptive names"
```

---

## Task 3: Update gallery.md — Front Matter & Full Image Grid

**Files:**
- Modify: `content/gallery.md`

- [ ] **Step 1: Replace the entire content of `content/gallery.md`**

Replace the full file with the following:

```markdown
---
title: "Gallery"
eyebrow_en: "Visual Pause"
eyebrow_zh: "视觉停驻"
title_zh: "相册"
desc_en: "Seventeen lotus — water, light, and the quiet between petals"
desc_zh: "十七朵莲：水、光，与花瓣之间的静默"
---

<div class="gallery-grid">

<figure class="gallery-item">
  <img src="/images/lotus-pink-saffu.jpg" loading="lazy" alt="Pink water lily floating on still dark water" />
  <figcaption>Pink lotus · Still water · Photo: Saffu / Unsplash<br>粉莲·静水·摄影：Saffu / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-white-nong.jpg" loading="lazy" alt="White water lily on dark still water" />
  <figcaption>White lotus · Dark mirror · Photo: Nong / Unsplash<br>白莲·暗镜·摄影：Nong / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-ripple-vitalyk.jpg" loading="lazy" alt="Concentric ripples spreading across a calm dark lake" />
  <figcaption>Ripples · A stone's memory · Photo: Vitaly K / Unsplash<br>涟漪·石落之念·摄影：Vitaly K / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-red-zoltan.jpg" loading="lazy" alt="Deep red-pink water lily rising from dark water, lily pads behind" />
  <figcaption>Crimson bloom · Rising alone · Photo: Zoltan Tasi / Unsplash<br>深红独绽·自水中来·摄影：Zoltan Tasi / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-white-pads-cheng.jpg" loading="lazy" alt="Single small white water lily surrounded by many dark green lily pads" />
  <figcaption>One white among many · Quietly itself · Photo: Cheng Feng / Unsplash<br>千叶之中一白·静守本心·摄影：Cheng Feng / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-lavender-yoksel.jpg" loading="lazy" alt="Lavender pink water lily with yellow centre and water droplets, reflected in blue water" />
  <figcaption>Lavender after rain · Each drop a world · Photo: Yoksel Zok / Unsplash<br>雨后薰衣·每滴皆是一个世界·摄影：Yoksel Zok / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-cream-star-ohi.jpg" loading="lazy" alt="Star-shaped cream water lily with yellow stamens against a soft grey background" />
  <figcaption>Star-shaped · Reaching toward grey light · Photo: Ohi Ozoya / Unsplash<br>星形花瓣·向灰光伸展·摄影：Ohi Ozoya / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-bokeh-zoltan.jpg" loading="lazy" alt="Ethereal white water lily glowing in bokeh light against a dark dreamy background" />
  <figcaption>Light through water · A dream half-remembered · Photo: Zoltan Tasi / Unsplash<br>光穿水面·如半梦初醒·摄影：Zoltan Tasi / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-blush-chris.jpg" loading="lazy" alt="Pale blush pink water lily nestled among deep green lily pads in low light" />
  <figcaption>Blush in the dark · Finding its own warmth · Photo: Chris Wong / Unsplash<br>暗处微红·自寻温暖·摄影：Chris Wong / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-soft-pink-jason.jpg" loading="lazy" alt="Soft pink water lily with yellow centre opening over dark water and pads" />
  <figcaption>Softest pink · The colour of early morning · Photo: Jason Leung / Unsplash<br>最柔的粉·清晨的颜色·摄影：Jason Leung / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-white-sun-ed.jpg" loading="lazy" alt="White water lily fully open in sunlight with golden centre and water droplets" />
  <figcaption>Sunlit white · Wide open · Photo: Ed Stone / Unsplash<br>阳光白莲·全然敞开·摄影：Ed Stone / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-pink-reflect-wang.jpg" loading="lazy" alt="Pink water lily with its mirror reflection on dark still water" />
  <figcaption>Pink and its shadow · Two blooms for one stem · Photo: Wang Binghua / Unsplash<br>粉莲与影·一茎两花·摄影：Wang Binghua / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-open-arto.jpg" loading="lazy" alt="Pink-tipped white lotus flower opening its petals against large dark green leaves" />
  <figcaption>Opening · Petal by petal · Photo: Arto Suraj / Unsplash<br>次第开放·一瓣一瓣地·摄影：Arto Suraj / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-white-mirror-shana.jpg" loading="lazy" alt="White water lily and its reflection in dark moody water surrounded by lily pads" />
  <figcaption>White and its reflection · Inseparable · Photo: Shana van Roosbroek / Unsplash<br>白莲与倒影·不可分离·摄影：Shana van Roosbroek / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-blue-drop-skyler.jpg" loading="lazy" alt="White and lavender water lily with water droplets reflected in still blue water" />
  <figcaption>Blue-edged petals · After the rain stayed · Photo: Skyler Ewing / Unsplash<br>蓝边花瓣·雨后留痕·摄影：Skyler Ewing / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-peach-shreyaan.jpg" loading="lazy" alt="Peach and cream water lily with orange centre resting on dark green lily pads" />
  <figcaption>Peach and gold · Warmth made visible · Photo: Shreyaan Vashishtha / Unsplash<br>桃金相映·温柔可见·摄影：Shreyaan Vashishtha / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/lotus-white-shadow-nong.jpg" loading="lazy" alt="White water lily alone on grey water casting a soft shadow beneath" />
  <figcaption>White alone on grey · Nothing is missing · Photo: Nong / Unsplash<br>灰水独白·什么都不缺·摄影：Nong / Unsplash</figcaption>
</figure>

</div>

<div id="lightbox" class="lightbox" onclick="this.classList.remove('active')">
  <img id="lightbox-img" src="" alt="" />
</div>

<script>
document.querySelectorAll('.gallery-item img').forEach(function(img) {
  img.style.cursor = 'zoom-in';
  img.addEventListener('click', function() {
    document.getElementById('lightbox-img').src = this.src;
    document.getElementById('lightbox-img').alt = this.alt;
    document.getElementById('lightbox').classList.add('active');
  });
});
document.addEventListener('keydown', function(e) {
  if (e.key === 'Escape') document.getElementById('lightbox').classList.remove('active');
});
</script>
```

- [ ] **Step 2: Verify the file looks correct**

```bash
head -10 /Users/Damon/Projects/damonmiaom.github.io/content/gallery.md
```

Expected — first lines should show:
```
---
title: "Gallery"
eyebrow_en: "Visual Pause"
eyebrow_zh: "视觉停驻"
title_zh: "相册"
desc_en: "Seventeen lotus — water, light, and the quiet between petals"
```

- [ ] **Step 3: Count the image entries to confirm all 17 are present**

```bash
grep -c 'gallery-item' /Users/Damon/Projects/damonmiaom.github.io/content/gallery.md
```

Expected output: `17`

- [ ] **Step 4: Confirm all image src paths reference renamed/new files (no old names)**

```bash
grep 'src="/images/' /Users/Damon/Projects/damonmiaom.github.io/content/gallery.md
```

Expected: 17 lines, all starting with `/images/lotus-`. None should say `lotus.jpg`, `waterlily.jpg`, or `ripple.jpg`.

- [ ] **Step 5: Commit**

```bash
cd /Users/Damon/Projects/damonmiaom.github.io
git add content/gallery.md
git commit -m "feat: expand gallery to 17 lotus images with bilingual captions and lazy loading"
```

---

## Task 4: Local Verification

- [ ] **Step 1: Start Hugo dev server**

```bash
cd /Users/Damon/Projects/damonmiaom.github.io
hugo server -D
```

Expected: Server starts at `http://localhost:1313` with no errors.

- [ ] **Step 2: Open gallery page in browser**

Navigate to: `http://localhost:1313/gallery/`

Check:
- All 17 images load (scroll to verify)
- Bilingual captions appear under each image (EN line + ZH line)
- Clicking any image opens the lightbox
- Pressing Escape closes the lightbox
- Page header shows new description: "Seventeen lotus — water, light, and the quiet between petals"

- [ ] **Step 3: Stop server**

Press `Ctrl+C` in the terminal running Hugo.

---

## Task 5: Deploy

- [ ] **Step 1: Push to GitHub**

```bash
cd /Users/Damon/Projects/damonmiaom.github.io
git push
```

Expected: All 3 commits push successfully. Netlify (or GitHub Pages) will auto-deploy within ~1 minute.

- [ ] **Step 2: Verify live site**

Open `https://damonmiaom.github.io/gallery/` in browser.

Check: 17 images display, captions are bilingual, lightbox works.
