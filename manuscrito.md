---
layout: page
title: El manuscrito
body_class: wide-page
hide_title: true
nav_order: 3
---
<h1 id="titulo" style="margin-left:1.5rem;">El proceso de MG: transcripción del manuscrito</h1>
<div id="toc" class="gallery">
    {% assign sorted_texts = site.texts | sort: "order" %}
    {% for text in sorted_texts %}
    <div class="gallery-item">
      <!--
      This, in the original code, activates the JavaScript function that displays the manuscript image and
      transcription content
      Esto, en el código previo, activa la función de JavaScript que muestra la imagen del manuscrito y su
      trascripción
        <a href="#"
            onclick="showText('{{ text.order }}'); return false;">
      -->
        <a href="{{ site.baseurl }}{{ text.url }}">
            <img src="{{ site.baseurl}}{{ text.image }}" alt="Manuscript page: {{ text.title }}" class="thumbnail">
            {{ text.title }}
        </a>
    </div>
    {% endfor %}
</div>
<br>
<div style="margin-left:1.5rem">
  <a href="{{ site.baseurl }}/criterios-transcripcion/">Los criterios de la transcripción</a>
</div>
<div class="citation">
  <br>
  <hr>
  <p>Cómo citar la transcripción del manuscrito: Sarah Kurth, Alejandre Lamas-Nemec, and Kathryn Phipps. El proceso de Mayor García: la transcripción del manuscrito. Bryn Mawr College, 2026. Web.</p>
<div>


<!-- ----------------------------------------------------------------------------------------------------------------- 

    A TON of original JavaScript-based code blocked out
    This would enable the viewing of each manuscript with its transcription on the same page (i.e., no redirecting to
    some other web page)
    Es: Un montón de código previo de JavaScript bloqueado
    Esto activaría la visita de cada manuscrito con sendas trascripciones en la misma página (i.e., no se dirige a
    otra página de Internet)

<div class="row">
  <div id="content">
    {% for text in site.texts %}
      <div id="{{ text.order }}" class="text-section" style="display:none;">
        <h1 style="margin-left:25%;">{{ text.title }}</h1>
        <div class="column-left" id="folium">
          <img src="{{ site.baseurl}}{{ text.image }}" alt="Manuscript page: {{ text.title }}" style="max-width:80%;height:auto;margin:10px 0;">
        </div>
        <div class="column-right">
          {{ text.content }}
        </div>
      </div>
    {% endfor %}
  </div>
</div>
<div style="display:flex;align-items:center;justify-content:center;">
  <button id="back" onclick="history.back()" style="display:none">
      Volver
  </button>
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
  //These two variables are meant to indicate the current page the user is on and how many pages there are, respectively.
  //Las dos siguientes variables indican, respectivamente, en qué página está el usuario o la usuaria y cuántas páginas hay en total.
  var curr_page;
  let all_pgs = document.querySelectorAll('.text-section').length;
  //Displays the text and facsimile of a certain page.
  //Muestra la transcripción y facsímil de una cierta página.
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
    document.getElementById('back').style.display = 'none';
    document.documentElement.scrollTop = 150;
    document.body.scrollTop = 150;
  }
  //Displays the table of contents.
  //Muestra la tabla de contenidos.
  function showTOC() {
    document.getElementById('toc').style.display = 'flex';
    document.getElementById('showtoc_bt').style.display = 'none';
    document.getElementById("titulo").innerHTML="Facsímiles";
    document.getElementById('next').style.display='none';
    document.getElementById('prev').style.display='none';
    document.getElementById('cambio').style.display = 'none';
    document.getElementById('back').style.display = 'none';
    document.querySelectorAll('.text-section').forEach(function(el) {
      el.style.display = 'none';
    });
    document.documentElement.scrollTop = 0;
    document.body.scrollTop = 0;
  }
  //The following two functions display the next or previous page relative to the current page.
  //Las dos siguientes funciones muestran la página siguiente o anterior respecto a la página actual.
  function next() {
    var next_page = curr_page + 1;
    showText.call(this, next_page);
  }
  function prev() {
    var prev_page = curr_page -1;
    showText.call(this, prev_page);
  }
  //
  function goToPage() {
    const select = document.getElementById("jump_pg");
    const new_pg = select.value;
    if (new_pg == "") {showTOC.call(this)}
    else{
    showText.call(this, new_pg);
    }
  }

  //The following functions are for a button on the 'modern edition' page that is meant to display the facsimile and transcription of the modern transcription they are viewing.
  //Las siguientes funciones sirven para un botón en la página de la edición moderna que cuando se pincha muestra el facsímil y la transcripción de la transcripción moderna que están viendo.
  function immediate_facs(id) {
    curr_page = Number(id);
    document.getElementById("titulo").innerHTML="Facsímil y transcripción";
    document.getElementById('toc').style.display = 'none';
    document.getElementById('showtoc_bt').style.display = 'none';
    document.getElementById('next').style.display='none';
    document.getElementById('prev').style.display ='none';
    document.getElementById('back').style.display='block';
    
    document.querySelectorAll('.text-section').forEach(function(el) {
      el.style.display = 'none';
    });
    document.getElementById('cambio').style.display = 'none';
    document.getElementById(id).style.display = 'block';
    document.documentElement.scrollTop = 150;
    document.body.scrollTop = 150;
  }
  window.addEventListener('DOMContentLoaded', () => {
      const params = new URLSearchParams(window.location.search);

      const id = params.get('id');
      //const difficulty = params.get('difficulty');

      if (id) {
          immediate_facs(id);
          //startQuiz(chapter, difficulty);
      }
  });
</script>
-->