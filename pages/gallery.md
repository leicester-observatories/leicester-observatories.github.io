---
layout: page
title: Gallery
permalink: /gallery/
---

This is a selection of images taken using the Observatory by our undergraduate and postgraduate students. You can tap/click on each image to see it in full, along with a description

<style>
.gallery-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
}
.gallery-item img {
  width: 100%;
  cursor: pointer;
}

.gallery-item img {
  width: 100%;
  cursor: pointer;
  transition: transform 0.25s ease; /* smooth scaling */
}

.gallery-item img:hover {
  transform: scale(1.05); /* small expand on hover */
}

#lightbox-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.85);
  display: none;   /* <-- FIX: hide until an image is clicked */
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 1rem;
  z-index: 9999;
  color: #fff;
  overflow: auto;
}

#lightbox-img {
  max-width: 90%;
  max-height: 80vh; /* ensures the image never exceeds viewport height */
  box-shadow: 0 0 20px #000;
}

#lightbox-caption {
  margin-top: 1rem;
  font-size: 1.1rem;
  white-space: pre-wrap; /* preserves line breaks */
  max-height: 50vh; /* allows caption to take up remaining space */
  overflow: auto;   /* scroll if caption is too long */
  padding: 0.5rem;  /* small padding inside caption box */
  width: 90%;       /* match image width for alignment */
  text-align: center;
}


</style>

<div class="gallery-grid">
{% assign gallery = site.static_files
   | where_exp: "file", "file.path contains '/images/gallery/'"
   | sort: 'modified_time' %}

{% for image in gallery %}
  {% assign caption = site.data.gallery[image.basename] %}
  <div class="gallery-item" data-fullsrc="{{ image.path }}">
    <img src="{{ image.path }}" alt="Image {{ forloop.index }}">
    
    {% if caption %}
      <pre class="hidden-caption" style="display:none;">{{ caption }}</pre>
    {% endif %}
  </div>
{% endfor %}
</div>

<div id="lightbox-overlay" onclick="closeLightbox()">
  <img id="lightbox-img" src="">
  
  <div id="lightbox-caption"></div>
</div>

<script>
document.addEventListener('DOMContentLoaded', () => {
  const items = document.querySelectorAll('.gallery-item');
  const overlay = document.getElementById('lightbox-overlay');
  const lightboxImg = document.getElementById('lightbox-img');
  const captionDiv = document.getElementById('lightbox-caption');

  items.forEach(item => {
    item.addEventListener('click', () => {
      lightboxImg.src = item.dataset.fullsrc;
      const hidden = item.querySelector('.hidden-caption');
      if (hidden) {
        captionDiv.innerHTML = hidden.innerHTML; // preserves line breaks
      } else {
        captionDiv.innerHTML = '';
      }
      overlay.style.display = 'flex';
    });
  });
});

function closeLightbox() {
  const overlay = document.getElementById('lightbox-overlay');
  overlay.style.display = 'none';
  document.getElementById('lightbox-img').src = '';
  document.getElementById('lightbox-caption').innerHTML = '';
}
</script>
