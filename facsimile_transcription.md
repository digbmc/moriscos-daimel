---
layout: page
title: Facsimile Transcription
body_class: wide-page
hide_title: true
nav_order: 4
---
<!-- If we wanted a download button:
<a class="download_bt" download=""><button type="button">Download PDF</button></a>
-->
<!-- The code here has the user taken to a SEPARATE page for each page
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
<h1 class="title_columned">Facsimile Transcriptions</h1>
<div id="toc" class="toc">
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
      <div id="{{ text.slug }}" class="text-section" style="display:none;">
        <div class="column" id="folium">
          <img src="{{ site.baseurl}}{{ text.image }}" alt="" style="max-width:80%;height:auto;margin:10px 0;">
        </div>
        <div class="column">
          <h1>{{ text.title }}</h1>
          {{ text.content }}
        </div>
      </div>
    {% endfor %}
  </div>
</div>

<div class="bottom_buttons">
  <button style="display:none;"> Previous</button>
  <button id="showtoc_bt" onclick="showTOC()" style="margin-top:20px;margin-bottom: 50px;display:none;"> Return to Table of Contents</button>
  <button style="display:none;"> Next</button>
</div>
<!--
<div class="jump_container" onclick="goToPage()">
  <select id="jump_pg">
    <option value="">Jump to Page</option>
    {% for item in site.texts %}
    <option value= "{{ site.baseurl }}{{ item.url }}">
      {{ item.title }}
    </option>
    {% endfor %}
  </select>
</div>
-->
<script>
  function showText(id) {
    document.getElementById('toc').style.display = 'none';
    document.getElementById('showtoc_bt').style.display = 'block';
    document.querySelectorAll('.text-section').forEach(function(el) {
      el.style.display = 'none';
    });
    document.getElementById(id).style.display = 'block';
  }
  function showTOC() {
    document.getElementById('toc').style.display = 'block';
    document.getElementById('showtoc_bt').style.display = 'none';
    document.querySelectorAll('.text-section').forEach(function(el) {
    el.style.display = 'none';
    });
  }
  </script>