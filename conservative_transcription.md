---
layout: page
title: Conservative Transcription
hide_in_nav: true
---

<a class="download_bt" download=""><button type="button">Download PDF</button></a>

<div class="toc">
  <!--<h2>sample texts</h2>-->
  <ul class="texts">
  {% for item in site.texts %}

    <li class="text-title">
      <a href="{{ site.baseurl }}{{ item.url }}">
        {{ item.title }}
      </a>
    </li>
  {% endfor %}
  </ul>
</div>
