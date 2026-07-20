---
layout: page
title: La edición moderna
hide_title: true
nav_order: 4
---
<!--<h1 class="title_columned">Transcripciones facsimilares</h1>-->
<h1 id="titulo">El proceso de MG: La edición moderna</h1>
<div id="toc" class="toc">
  <ul>
    <li>
      <a href="{{ site.baseurl }}/criterios-em/">Los criterios de la edición</a>
    </li>
  {% assign txts_ordenados = site.textos-modernos | sort: "order" %}
  {% for texto in txts_ordenados %}
    <li>
      <a href="{{ site.baseurl }}/texto-modernizado/#{{ texto.order }}">
        {{ texto.title }}
      </a>
    </li>
  {% endfor %}
  </ul>
</div>

<br>
<hr>
Cómo citar la edición modernizada: Alejandre Lamas-Nemec and Kathryn Phipps. El proceso de Mayor García: la edición moderna. Bryn Mawr College, 2026. Web.

<!--
<script>
  var curr_page;
  let all_pgs = document.querySelectorAll('.text-section').length;
  function showText(id) {
    curr_page = Number(id);
    document.getElementById('toc').style.display = 'none';
    document.getElementById('criterios').style.display = 'none';
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
    //document.getElementById('show_facs').style.display = 'block';
    document.getElementById(id).style.display = 'block';
    document.documentElement.scrollTop = 150;
    document.body.scrollTop = 150;
  }
  function showTOC() {
    document.getElementById('toc').style.display = 'block';
    document.getElementById('criterios').style.display = 'block';
    document.getElementById('showtoc_bt').style.display = 'none';
    document.getElementById('next').style.display='none';
    document.getElementById('prev').style.display='none';
    document.getElementById('cambio').style.display = 'none';
    //document.getElementById('show_facs').style.display = 'none';
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
-->