---
title: "Gallery"
---

<p class="gallery-intro">A collection of images I find worth pausing for — nature, stillness, and the spaces between things.</p>

<div class="gallery-grid">

<figure class="gallery-item">
  <img src="/images/lotus.jpg" alt="Pink water lily floating on still dark water" />
  <figcaption>Water lily · Still water · Photo: Saffu / Unsplash</figcaption>
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
