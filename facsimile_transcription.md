---
layout: page
title: Transcripción facsimilar
body_class: wide-page
hide_title: true
nav_order: 4
---
<h1 class="title_columned">Transcripciones facsimilares</h1>
<div id="toc" class="toc">
  <h2>Todas las páginas en orden</h2>
  <ul>
  {% for text in site.texts %}
    <li>
      <a href="#"
        onclick="showText('{{ text.order }}'); return false;">
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
      <div id="{{ text.order }}" class="text-section" style="display:none;">
        <h1 style="margin-left:25%;">{{ text.title }}</h1>
        <div class="column-left" id="folium">
          <img src="{{ site.baseurl}}{{ text.image }}" alt="Image not available" style="max-width:80%;height:auto;margin:10px 0;">
        </div>
        <div class="column-right">
          {{ text.content }}
        </div>
      </div>
    {% endfor %}
  </div>
</div>
<div class="bottom_buttons">
  <button id="prev" onclick="prev()" style="display:none;"> Anterior</button>
  <button id="showtoc_bt" onclick="showTOC()" style="display:none;"> Volver a la tabla de contenidos</button>
  <button id="next" onclick="next()" style="display:none;"> Próxima</button>
</div>

<div class="jump_container" id="cambio" onclick="goToPage()" style="display:none;">
  <select id="jump_pg">
    <option value="">Elegir la página</option>
    {% for text in site.texts %}
    <option value= "{{ text.order }}">
      {{ text.title }}
    </option>
    {% endfor %}
  </select>
</div>

<script>
  var curr_page;
  let all_pgs = document.querySelectorAll('.text-section').length;
  function showText(id) {
    curr_page = Number(id);
    document.getElementById('toc').style.display = 'none';
    document.getElementById('showtoc_bt').style.display = 'block';
    document.getElementById('next').style.display='block';
    document.getElementById('prev').style.display ='block';
    if (curr_page >= all_pgs) {
      document.getElementById('next').style.visibility='hidden';
    } else {
      document.getElementById('next').style.visibility='visible';
    }
    if (curr_page <= 1) {
      document.getElementById('prev').style.visibility = 'hidden';
    } else {
      document.getElementById('prev').style.visibility='visible';
    }
    document.querySelectorAll('.text-section').forEach(function(el) {
      el.style.display = 'none';
    });
    document.getElementById('cambio').style.display = 'block';
    document.getElementById(id).style.display = 'block';
    document.documentElement.scrollTop = 0;
    document.body.scrollTop = 0;
  }
  function showTOC() {
    document.getElementById('toc').style.display = 'block';
    document.getElementById('showtoc_bt').style.display = 'none';
    document.getElementById('next').style.display='none';
    document.getElementById('prev').style.display='none';
    document.getElementById('cambio').style.display = 'none';
    document.querySelectorAll('.text-section').forEach(function(el) {
      el.style.display = 'none';
    });
    document.documentElement.scrollTop = 0;
    document.body.scrollTop = 0;
  }
  function next() {
    var next_page = curr_page + 1;
    showText.call(this, next_page);
  }
  function prev() {
    var prev_page = curr_page -1;
    showText.call(this, prev_page);
  }
  function goToPage() {
    const select = document.getElementById("jump_pg");
    const new_pg = select.value;
    showText.call(this, new_pg);
  }
</script>