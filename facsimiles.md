---
layout: page
title: Facsímiles
nav_order: 3
#hide_in_nav: true
---

<div class="gallery">
    {% for text in site.texts %}
    <a href = "{{ site.baseurl}}{{ text.image }}" target="_blank">
        <img src="{{ site.baseurl}}{{ text.image }}" alt="Image not available" class="thumbnail">
    </a>
    {% endfor %}
</div>