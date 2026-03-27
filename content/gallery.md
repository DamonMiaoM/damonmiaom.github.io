---
title: "Gallery"
eyebrow_en: "Visual Pause"
eyebrow_zh: "视觉停驻"
title_zh: "相册"
desc_en: "Images worth pausing for — nature, stillness, the spaces between things"
desc_zh: "值得驻足的图像：自然、静谧、万物之间的留白"
---

<div class="gallery-grid">

<figure class="gallery-item">
  <img src="/images/lotus.jpg" alt="Pink water lily floating on still dark water" />
  <figcaption>Water lily · Still water · Photo: Saffu / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/waterlily.jpg" alt="White water lily floating on dark still water" />
  <figcaption>White lotus · Still water · Photo: Nong / Unsplash</figcaption>
</figure>

<figure class="gallery-item">
  <img src="/images/ripple.jpg" alt="Concentric ripples on a dark calm lake surface" />
  <figcaption>Ripples · Still lake · Photo: Vitaly K / Unsplash</figcaption>
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
