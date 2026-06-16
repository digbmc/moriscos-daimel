---
layout: page
title: Facsimile Transcription
nav_order: 4
#hide_in_nav: true
---
<!-- If we wanted a download button:
<a class="download_bt" download=""><button type="button">Download PDF</button></a>
-->
<div class="toc">
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
