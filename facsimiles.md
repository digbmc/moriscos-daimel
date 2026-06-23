---
layout: page
title: Facsímiles
body_class: wide-page
hide_title: true
nav_order: 3
---
<h1 style="margin-left:2rem;">Facsímiles</h1>
<div class="gallery">
    {% assign sorted_texts = site.texts | sort: "order" %}
    {% for text in sorted_texts %}
    <div class="gallery-item">
        <a href = "{{ site.baseurl}}{{ text.image }}" target="_blank">
            <img src="{{ site.baseurl}}{{ text.image }}" alt="La imagen no está dispoible" class="thumbnail">
        </a>
        <p class="caption">{{ text.title }}</p>
    </div>
    {% endfor %}
</div>