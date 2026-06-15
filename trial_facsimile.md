---
layout: page
title: Images
hide_in_nav: true
---

<div class="facsimile">
    {% for img in site.images %}
        <a href="{{ img.image }}">
            <img src="{{ img.image }}" alt="{{ img.title }}">
        </a>
    {% endfor %}
</div>