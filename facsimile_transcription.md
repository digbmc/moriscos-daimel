---
layout: page
title: Facsimile Transcription
nav_order: 4
#hide_in_nav: true
---
<!-- If we wanted a download button:
<a class="download_bt" download=""><button type="button">Download PDF</button></a>
-->
<!--
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
-->

<hr>

<div id="toc">
  <ul>
  {% for text in site.texts %}
    <li>
      <a href="#"
        onclick="showText('{{ text.slug }}'); return false;">
        {{ text.title }}
      </a>
    </li>
  {% endfor %}
  </ul>
</div>

<div class="row">
  <div id="content">
    {% for text in site.texts %}
    <!--{% assign chapter = site.texts | where: "title", "Folder 2, 1r" | first %}-->
      <div id="{{ text.slug }}" class="text-section" style="display:none;">
        <div class="column">
          <img src="{{ site.baseurl}}{{ text.image }}" alt="" style="max-width:100%;height:auto;margin:10px 0;">
        </div>
        <div class="column">
          <h1>{{ text.title }}</h1>
          {{ text.content }}
        </div>
        <button onclick="showTOC()"> Return to Table of Contents</button>
      </div>
    {% endfor %}
  </div>
</div>

<script>
  function showText(id) {
    document.getElementById('toc').style.display = 'none';
    document.querySelectorAll('.text-section').forEach(function(el) {
      el.style.display = 'none';
    });
    document.getElementById(id).style.display = 'block';
  }
  function showTOC() {
    document.getElementById('toc').style.display = 'block';
    document.querySelectorAll('.text-section').forEach(function(el) {
    el.style.display = 'none';
    });
  }
  </script>