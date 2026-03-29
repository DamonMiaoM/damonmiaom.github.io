# Lotus Gallery Expansion — Design Spec

**Date:** 2026-03-29
**Status:** Approved
**Approach:** C — Full Enrichment

---

## Overview

Expand the existing Gallery page from 3 images to 17 lotus/water lily images. All images displayed in one unified masonry column grid. Each image has bilingual (EN/ZH) poetic captions and proper accessibility alt text. New images added with clean descriptive filenames and lazy loading for performance.

---

## Decisions

| Decision | Choice | Reason |
|---|---|---|
| Layout | All 17 in one unified grid | Single cohesive collection feel |
| Grid style | Masonry columns (existing CSS) | Organic, natural — suits lotus photography |
| Captions | Bilingual poetic (EN + ZH) | Matches site's bilingual identity |
| Filenames | Clean descriptive names | Maintainability over Unsplash defaults |
| Performance | `loading="lazy"` on all images | 17 images need lazy loading |
| Page description | Refreshed to reflect lotus theme | Current text too generic for expanded collection |
| Alt text | Descriptive per image | Accessibility audit |

---

## File Operations

### Images to copy
Source: `/Users/Damon/Personal Website/Images/`
Destination: `/Users/Damon/Projects/damonmiaom.github.io/static/images/`

### Existing images — rename
| Old Name | New Name |
|---|---|
| `lotus.jpg` | `lotus-pink-saffu.jpg` |
| `waterlily.jpg` | `lotus-white-nong.jpg` |
| `ripple.jpg` | `lotus-ripple-vitalyk.jpg` |

### New images — copy with clean names
| Source (Unsplash filename) | Destination |
|---|---|
| `zoltan-tasi-yanhwFwyoaU-unsplash.jpg` | `lotus-red-zoltan.jpg` |
| `cheng-feng-BP-Q-Z0Ua9Y-unsplash.jpg` | `lotus-white-pads-cheng.jpg` |
| `yoksel-zok-bh0ai10Cxc4-unsplash.jpg` | `lotus-lavender-yoksel.jpg` |
| `ohi-ozoya-pQD0K7EOoQA-unsplash.jpg` | `lotus-cream-star-ohi.jpg` |
| `zoltan-tasi-vHnVtLK8rCc-unsplash.jpg` | `lotus-bokeh-zoltan.jpg` |
| `chris-wong-GEdrQkuF7iE-unsplash.jpg` | `lotus-blush-chris.jpg` |
| `jason-leung-B14YYbBZnT4-unsplash.jpg` | `lotus-soft-pink-jason.jpg` |
| `ed-stone-olusucJ05eM-unsplash.jpg` | `lotus-white-sun-ed.jpg` |
| `wang-binghua-qfE8Qqbkmn0-unsplash.jpg` | `lotus-pink-reflect-wang.jpg` |
| `arto-suraj-LpGFOOln8tk-unsplash.jpg` | `lotus-open-arto.jpg` |
| `shana-van-roosbroek-xeH1hzOyoT0-unsplash.jpg` | `lotus-white-mirror-shana.jpg` |
| `skyler-ewing-9A-Z4nHrIZk-unsplash.jpg` | `lotus-blue-drop-skyler.jpg` |
| `shreyaan-vashishtha-tFWKWMs-95E-unsplash.jpg` | `lotus-peach-shreyaan.jpg` |
| `nong-Hqa-Ymae0k0-unsplash.jpg` | `lotus-white-shadow-nong.jpg` |

---

## gallery.md Changes

### Front matter — refreshed description
```yaml
eyebrow_en: "Visual Pause"
eyebrow_zh: "视觉停驻"
desc_en: "Seventeen lotus — water, light, and the quiet between petals"
desc_zh: "十七朵莲：水、光，与花瓣之间的静默"
```

### Image list — all 17 with captions and alt text

| # | File | Alt Text | EN Caption | ZH Caption |
|---|---|---|---|---|
| 1 | lotus-pink-saffu.jpg | Pink water lily floating on still dark water | Pink lotus · Still water · Photo: Saffu / Unsplash | 粉莲·静水·摄影：Saffu / Unsplash |
| 2 | lotus-white-nong.jpg | White water lily on dark still water | White lotus · Dark mirror · Photo: Nong / Unsplash | 白莲·暗镜·摄影：Nong / Unsplash |
| 3 | lotus-ripple-vitalyk.jpg | Concentric ripples spreading across a calm dark lake | Ripples · A stone's memory · Photo: Vitaly K / Unsplash | 涟漪·石落之念·摄影：Vitaly K / Unsplash |
| 4 | lotus-red-zoltan.jpg | Deep red-pink water lily rising from dark water, lily pads behind | Crimson bloom · Rising alone · Photo: Zoltan Tasi / Unsplash | 深红独绽·自水中来·摄影：Zoltan Tasi / Unsplash |
| 5 | lotus-white-pads-cheng.jpg | Single small white water lily surrounded by many dark green lily pads | One white among many · Quietly itself · Photo: Cheng Feng / Unsplash | 千叶之中一白·静守本心·摄影：Cheng Feng / Unsplash |
| 6 | lotus-lavender-yoksel.jpg | Lavender pink water lily with yellow centre and water droplets, reflected in blue water | Lavender after rain · Each drop a world · Photo: Yoksel Zok / Unsplash | 雨后薰衣·每滴皆是一个世界·摄影：Yoksel Zok / Unsplash |
| 7 | lotus-cream-star-ohi.jpg | Star-shaped cream water lily with yellow stamens against a soft grey background | Star-shaped · Reaching toward grey light · Photo: Ohi Ozoya / Unsplash | 星形花瓣·向灰光伸展·摄影：Ohi Ozoya / Unsplash |
| 8 | lotus-bokeh-zoltan.jpg | Ethereal white water lily glowing in bokeh light against a dark dreamy background | Light through water · A dream half-remembered · Photo: Zoltan Tasi / Unsplash | 光穿水面·如半梦初醒·摄影：Zoltan Tasi / Unsplash |
| 9 | lotus-blush-chris.jpg | Pale blush pink water lily nestled among deep green lily pads in low light | Blush in the dark · Finding its own warmth · Photo: Chris Wong / Unsplash | 暗处微红·自寻温暖·摄影：Chris Wong / Unsplash |
| 10 | lotus-soft-pink-jason.jpg | Soft pink water lily with yellow centre opening over dark water and pads | Softest pink · The colour of early morning · Photo: Jason Leung / Unsplash | 最柔的粉·清晨的颜色·摄影：Jason Leung / Unsplash |
| 11 | lotus-white-sun-ed.jpg | White water lily fully open in sunlight with golden centre and water droplets | Sunlit white · Wide open · Photo: Ed Stone / Unsplash | 阳光白莲·全然敞开·摄影：Ed Stone / Unsplash |
| 12 | lotus-pink-reflect-wang.jpg | Pink water lily with its mirror reflection on dark still water | Pink and its shadow · Two blooms for one stem · Photo: Wang Binghua / Unsplash | 粉莲与影·一茎两花·摄影：Wang Binghua / Unsplash |
| 13 | lotus-open-arto.jpg | Pink-tipped white lotus flower opening its petals against large dark green leaves | Opening · Petal by petal · Photo: Arto Suraj / Unsplash | 次第开放·一瓣一瓣地·摄影：Arto Suraj / Unsplash |
| 14 | lotus-white-mirror-shana.jpg | White water lily and its reflection in dark moody water surrounded by lily pads | White and its reflection · Inseparable · Photo: Shana van Roosbroek / Unsplash | 白莲与倒影·不可分离·摄影：Shana van Roosbroek / Unsplash |
| 15 | lotus-blue-drop-skyler.jpg | White and lavender water lily with water droplets reflected in still blue water | Blue-edged petals · After the rain stayed · Photo: Skyler Ewing / Unsplash | 蓝边花瓣·雨后留痕·摄影：Skyler Ewing / Unsplash |
| 16 | lotus-peach-shreyaan.jpg | Peach and cream water lily with orange centre resting on dark green lily pads | Peach and gold · Warmth made visible · Photo: Shreyaan Vashishtha / Unsplash | 桃金相映·温柔可见·摄影：Shreyaan Vashishtha / Unsplash |
| 17 | lotus-white-shadow-nong.jpg | White water lily alone on grey water casting a soft shadow beneath | White alone on grey · Nothing is missing · Photo: Nong / Unsplash | 灰水独白·什么都不缺·摄影：Nong / Unsplash |

---

## Technical Notes

- All `<img>` tags get `loading="lazy"` attribute
- Caption format: `EN caption · Photo: Name / Unsplash` on first line, ZH on second line (matching existing pattern via `<br>`)
- No CSS changes needed — existing `.gallery-grid` and `.gallery-item` styles handle 17 images fine
- Lightbox script requires no changes
- After implementation: `git add . && git commit && git push` to deploy

---

## Out of Scope

- No new CSS changes
- No filtering or categorisation by colour/mood
- No new shortcodes
- No changes to other pages
