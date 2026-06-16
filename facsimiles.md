---
layout: page
title: Facsimiles
#hide_in_nav: true
---

<div class="container">
    {% for facs in site.images %}
    
    <div class="images">

            <a href="{{ facs.image }}">
                <facs src="{{ facs.image }}" alt="{{ facs.title }}">
            </a>
    </div>
    {% endfor %}
</div>