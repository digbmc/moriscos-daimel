---
layout: page
title: Trascripción facsimilar
body_class: wide-page
hide_title: true
nav_order: 4
---
<h1 class="title_columned">Trascripciones facsimilares</h1>
<div id="toc" class="toc">
<h2>Todas las páginas en orden</h2>
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
  <h2>Testigos</h2>
</div>

<div class="row">
  <div id="content">
    {% for text in site.texts %}
      <div id="{{ text.slug }}" class="text-section" style="display:none;">
        <h1 style="margin-left:25%;">{{ text.title }}</h1>
        <div class="column-left" id="folium">
          <img src="{{ site.baseurl}}{{ text.image }}" alt="Image not available" style="max-width:80%;height:auto;margin:10px 0;">
        </div>
        <div class="column-right">
          <!--<h1>{{ text.title }}</h1>-->
          {{ text.content }}
        </div>
      </div>
    {% endfor %}
  </div>
</div>

<div>
  {% for text in site.texts %}
  <button id="{{ text.slug }}" class="next_text" onclick="next(id)" style="display:none;"> Next</button>
  {% endfor %}
</div>

<div class="bottom_buttons">
  <button style="display:none;"> Previous</button>
  <button id="showtoc_bt" onclick="showTOC()" style="margin-top:20px;margin-bottom: 50px;display:none;"> Volver a la tabla de contenidos</button>
  <!--<button id="next" onclick="next()" style="display:none;"> Next</button>-->
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
    /*document.getElementById('next').style.display = 'block';
    document.querySelectorAll('.text-section').forEach(function(el) {
      el.style.display = 'none';
    });*/
    document.getElementById(id).style.display = 'block';

    const items = document.querySelectorAll('.text-selection');
    items.value;

  }
  function showTOC() {
    document.getElementById('toc').style.display = 'block';
    document.getElementById('showtoc_bt').style.display = 'none';
    /*document.getElementById('next').style.display = 'none';*/
    document.querySelectorAll('.text-section').forEach(function(el) {
    el.style.display = 'none';
    });
  }
  function next(id) {
    /*const selections = document.querySelectorAll('.text-selection');
    selections.forEach(selection => {
      const title = selection.querySelector('.order').textContent;
      console.log(title);
    });*/
    document.querySelectorAll('.text-section').forEach(function(el) {
      el.style.display = 'none';
    });
  }
  </script>