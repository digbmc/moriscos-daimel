---
layout: page
title: Facsímiles
body_class: wide-page
hide_title: true
nav_order: 3
---
<h1 id="titulo" style="margin-left:2rem;">Facsímiles</h1>
<div id="toc" class="gallery">
    {% assign sorted_texts = site.texts | sort: "order" %}
    {% for text in sorted_texts %}
    <div class="gallery-item">
        <a href="#"
            onclick="showText('{{ text.order }}'); return false;">
            <img src="{{ site.baseurl}}{{ text.image }}" alt="La imagen no está dispoible" class="thumbnail">
            {{ text.title }}
        </a>
        <!--The code here originally opened the image jpg in a seperate window
        <a href = "{{ site.baseurl}}{{ text.image }}" target="_blank">
            <img src="{{ site.baseurl}}{{ text.image }}" alt="La imagen no está dispoible" class="thumbnail">
        </a>
        -->
        <!--<p class="caption">{{ text.title }}</p>-->
    </div>
    {% endfor %}
</div>
<div class="row">
  <div id="content">
    {% for text in site.texts %}
      <div id="{{ text.order }}" class="text-section" style="display:none;">
        <h1 style="margin-left:25%;">{{ text.title }}</h1>
        <div class="column-left" id="folium">
          <img src="{{ site.baseurl}}{{ text.image }}" alt="La imagen no está disponible" style="max-width:80%;height:auto;margin:10px 0;">
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
  <button id="next" onclick="next()" style="display:none;"> Siguiente</button>
</div>

<div class="jump_container" id="cambio" style="display:none;">
  <select id="jump_pg" onclick="goToPage()">
    <option value="">Elegir la página</option>
    {% assign sorted_texts = site.texts | sort: "order" %}
    {% for text in sorted_texts %}
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
    document.getElementById("titulo").innerHTML="Facsímil y transcripción";
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
    document.documentElement.scrollTop = 150;
    document.body.scrollTop = 150;
  }
  function showTOC() {
    document.getElementById('toc').style.display = 'flex';
    document.getElementById('showtoc_bt').style.display = 'none';
    document.getElementById("titulo").innerHTML="Facsímiles";
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
    if (new_pg == "") {showTOC.call(this)}
    else{
    showText.call(this, new_pg);
    }
  }
</script>