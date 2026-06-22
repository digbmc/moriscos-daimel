---
layout: page
title: Edición moderna
nav_order: 5
---
<!--<h1 class="title_columned">Transcripciones facsimilares</h1>-->
<div id="toc" class="toc">
  <!--No creo que fuere preciso que hubiere un título a menos que quisiéremos organizar los textos así como organizamos
  los trascripciones facsimilares
  <h2>Todas las páginas en orden</h2>
  -->
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
<br>
## Los criterios de edición
Se resuelve lo siguiente sin otra indicación: 

-se usa la ortografía moderna con acentuación estandardizada.  

--i/y, u/v/b, c/qu-, ç/c/s/z, g/j, b/p etc., -h (añadimos los que faltan y quitamos los que sobran) 

-sancto->santo 

-officio -> oficio 

-inq\[uisiçi\](on) -> inquisición 

-uso de mayúsculas para nombres títulos y el comercia de una oración 

-corrección de pluralización 

-Nombres de lugares, y personas:  

-Daimiel 

-Valtodano 

-María la Canbila 

- Agustin Illán 

- Alonso Magán Alcaide 

- Alonso Parron 

- Julián de Alprahe 

- Pedro de Farmas 

- Alonso de Nodarilagos 

- Benito Fernández 

- Benito Hernández 

- Juan García 

- Juana Goncales 

- Juan López Enrreda 

- Francisco Agraz 

- Leonor 

- Carabaco Ganadero 

- Mari Hernandes 

- Goncalo Peral 

- Francisco Peral 

- Diego Hernandes 

- Juana García 

- Lucia 

-se resuelven las abreviaciones q-->que inq(ores)-->inquisidores, etc. 

variaciónes de xpo por cristo (i.e. xpiana-->cristiana etc.) 

-xpiana 

-se separan las palabras conjuntas i.e. dellas--> de ellas 

-puntuación. 

	- donde se usa / se introduce la puntuación moderna; i.e. «»  

	- "/o" y "/o/" --> o 

-preservamos los espacios entre párrafos.  

Las variaciones que preservamos, señalamos en las notas.  

-Se mantiene vocabulario antiguo con una nota con su definición \[footnotes\] 

Dejamos la sintaxis antigua, con una modernización en las notas 

	el uso de pronombres 

-le prender el cuerpo 

Para preservar la autenticidad de un manuscrito, no corregimos los errores gramaticales (concordancia) pero indicamos la corrección en las notas.