---
layout: page
title: Edición moderna
nav_order: 4
---
<!--<h1 class="title_columned">Transcripciones facsimilares</h1>-->
<div id="toc" class="toc">
  <ul>
  {% assign sorted_texts = site.textos_modernos | sort: "order" %}
  {% for text in sorted_texts %}
    <li>
      <a href="#"
        onclick="showText('{{ text.order }}'); return false;">
        {{ text.title }}
      </a>
    </li>
  {% endfor %}
  </ul>
</div>

{% for text in site.textos_modernos %}
<div id="{{ text.order }}" class="text-section" style="display:none;">
  {{ text.content }}
</div>
{% endfor %}
<div class="bottom_buttons">
  <button id="prev" onclick="prev()" style="display:none;"> Anterior</button>
  <button id="showtoc_bt" onclick="showTOC()" style="display:none;"> Volver a la tabla de contenidos</button>
  <button id="next" onclick="next()" style="display:none;"> Siguiente</button>
</div>

<div class="jump_container" id="cambio" style="display:none;">
  <select id="jump_pg" onclick="goToPage()">
    <option value="">Elegir la página</option>
    {% assign sorted_texts = site.textos_modernos | sort: "order" %}
    {% for text in sorted_texts %}
    <option value= "{{ text.order }}">
      {{ text.title }}
    </option>
    {% endfor %}
  </select>
</div>
<button id="show_facs" onclick="goToFacsimile()" style="display:none;">Ver el facsímil y la transcripción</button>

<br>

<div id="criterios">
  <h2>Los criterios de edición</h2>
  <p>Se resuelve lo siguiente sin otra indicación:</p> 
  <ul>
    <li>se usa la ortografía moderna con acentuación estandardizada.</li>
    <ul>
      <li>i/y, u/v/b, c/qu-, ç/c/s/z, g/j, b/p etc., -h (añadimos los que faltan y quitamos los que sobran)</li>
      <li>sancto->santo</li>
      <li>officio -> oficio</li>
      <li>inq\[uisiçi\](on) -> inquisición</li>
      <li>uso de mayúsculas para nombres títulos y el comercia de una oración</li>
      <li>corrección de pluralización</li>
      <li>Nombres de lugares, y personas:</li>
      <ul>
        <li>Daimiel</li>
        <li>Valtodano</li>
        <li>María la Canbila</li>
        <li>Agustin Illán</li>
        <li>Alonso Magán Alcaide</li>
        <li>Alonso Parron</li>
        <li>Julián de Alprahe</li>
        <li>Pedro de Farmas</li>
        <li>Alonso de Nodarilagos</li>
        <li>Benito Fernández</li>
        <li>Benito Hernández</li>
        <li>Juan García</li>
        <li>Juana Goncales</li>
        <li>Juan López Enrreda</li>
        <li>Francisco Agraz</li>
        <li>Leonor</li>
        <li>Carabaco Ganadero</li>
        <li>Mari Hernandes</li>
        <li>Goncalo Peral</li>
        <li>Francisco Peral</li>
        <li>Diego Hernandes</li>
        <li>Juana García</li>
        <li>Lucia</li>
        <li>Sebastián</li>
      </ul>
    </ul>
    <li>se resuelven las abreviaciones q-->que inq(ores)-->inquisidores, etc.</li>
    <ul>
      <li>variaciónes de xpo por cristo (i.e. xpiana-->cristiana etc.)
        <br>
        -xpiana
      </li>
    </ul>
    <li>se separan las palabras conjuntas i.e. dellas--> de ellas</li>
    <li>puntuación.</li>
    <ul>
      <li>donde se usa / se introduce la puntuación moderna; i.e. «» .</li>
      <li>"/o" y "/o/" --> o</li>
    </ul>
    <li>preservamos los espacios entre párrafos.<br>
      Las variaciones que preservamos, señalamos en las notas.
    </li>
    <li>Se mantiene vocabulario antiguo con una nota con su definición \[footnotes\]<br>
      Dejamos la sintaxis antigua, con una modernización en las notas
    </li>
    <ul>
      <li>el uso de pronombres</li>
      <ul>
        <li>le prender el cuerpo</li>
      </ul>
    </ul>
  </ul>
  <p>Para preservar la autenticidad de un manuscrito, no corregimos los errores gramaticales (concordancia) pero indicamos la corrección en las notas.</p>
</div>

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
    document.getElementById('show_facs').style.display = 'block';
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
    document.getElementById('show_facs').style.display = 'none';
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