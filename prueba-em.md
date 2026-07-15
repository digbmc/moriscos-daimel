---
layout: page
title: prueba
body_class: wide-page
hide_title: true
---
<article>
    <!--En: First loop goes through all the .md files in textos-modernos (organized by chapter) in order-->
    <!--Es: El primer bucle revisa todos los archivos .md en textos-modernos (organizados por capítulo) en orden-->
    {% assign txts_ordenados = site.textos-modernos | sort: "order" %}
    {% for texto in txts_ordenados %}
        <!--En: This <div> keeps the data of each file together-->
        <!--Es: ste <div> guarda juntos los datos de cada archivo-->
        <div id="{{ texto.order }}" class="contenido">
            <h1 style="margin-left: 6rem;">{{ texto.title }}</h1><br>
            <!--En: Because chapters are not organized by page, but manuscripts are, a marker is used to divide the chapter into sections-->
            <!--Es: Dado que los capítulos no se organizan por página como los manuscritos, se utiliza un marcador para dividir el capítulo en secciones-->
            {% assign secciones = texto.content | split: "<!-- SPLIT -->" %}
            {% for seccion in secciones %}
                {% assign pagina = nil %} <!--En: Keeps the page number; Es: Guarda el número de página-->
                {% assign subtitulo = nil %}
                {% assign sum_notas = 0 %} <!--En: The number of notes in the section; Es: La suma de notas en la sección-->
                {% assign num_sec = forloop.index %}
                <!--En: Splits the section into paragraphs; Es: Divide la sección en párrafos-->
                {% assign parrafos = seccion | split: "</p>" %}
                {% for bloque in parrafos %}
                    {% if bloque contains "</h2>" %}
                        {% assign subtitulos = bloque | split: "</h2>" %}
                        {% assign subtitulo = subtitulos[0] | strip_html | strip %}
                        {% assign parrafo = subtitulos[1] | strip_html | strip %}
                    {% else %}
                        {% assign parrafo = bloque | strip_html | strip %}
                    {% endif %}
                    {% assign primera = parrafo | slice: 0 %}{% assign ultima = parrafo | slice: -1 %}
                    {% assign agarrar = parrafo | slice: -2 %}
                    {% if primera == "[" and ultima == "]" and agarrar != "." %}
                        {% assign pagina = parrafo %}
                    {% else %}
                        <div class="row">
                            <div class="margen">
                                {% if pagina != nil %}
                                    {% assign partes = pagina | split: ':' %}
                                    {% assign mitad = partes[0] | split: '[' %}
                                    {% assign num_pagina = mitad[1] %}
                                    {% assign orden = num_pagina | minus: 24 %}
                                    {% assign transc = site.texts | where: "order", orden | first %}
                                    <a href="{{ site.baseurl }}{{ transc.url }}">{{ pagina }}</a>
                                    {% assign pagina = nil %}
                                {% endif %}
                                {% if parrafo == "[Folio blanco.]" %}
                                    <p><i>Folio blanco.</i></p>
                                {% elsif parrafo contains "[nota]" %}
                                    {% assign notas = parrafo | split: "[nota]" %}
                                    {% assign suma = notas.size | minus: 1 %}
                                    {% for i in (1..suma) %}
                                        {% assign sum_notas = sum_notas | plus: 1 %}
                                        <p id="{{ texto.order}}_{{ num_sec }}_nota_{{ sum_notas }}">nota</p><br>
                                    {% endfor %}
                                {% endif %}
                            </div>
                            <div class="parrafo">
                                {% if subtitulo != nil %}
                                    <h2>{{ subtitulo }}</h2>
                                    {% assign subtitulo = nil %}
                                {% endif %}
                                {% if parrafo != "[Folio blanco.]" %}
                                    {% if parrafo contains "[nota]" %}
                                        {% assign parrafo = parrafo | remove: "/[nota]/" %}
                                    {% endif %}
                                    {% unless parrafo contains "[nota_" %}
                                        <p>{{ parrafo }}</p>
                                    {% else %}
                                        {% assign nom_nota = parrafo | slice: 2, 6 %}
                                        {% assign texto_a_quitar = "/[" | append: nom_nota | append: "]/" %}
                                        {% assign parrafo = parrafo | remove: texto_a_quitar %}
                                        <script>
                                            function imprimir_nota(id, texto) {
                                                const lugar = document.getElementById(id);
                                                if (lugar) {
                                                    lugar.innerHTML = texto;
                                                }
                                            }
                                            imprimir_nota("{{texto.order}}_{{ num_sec}}_{{ nom_nota }}", {{ parrafo | jsonify }});
                                        </script>
                                    {% endunless %}
                                {% endif %}
                            </div>
                        </div>
                    {% endif %}
                {% endfor %}
                <!--
                <div class="margen">
                    {% for bloque in parrafos %}
                        {% assign parrafo_p = bloque | markdownify %}
                        {% assign parrafo = bloque | strip_html | strip %}
                        {% assign primera = parrafo | slice: 0 %}{% assign ultima = parrafo | slice: -1 %}
                        {% assign agarrar = parrafo | slice: -2 %}
                        {% if primera == "[" and ultima == "]" and agarrar != "." %}
                            {% assign partes = parrafo | split: ':' %}
                            {% assign mitad = partes[0] | split: '[' %}
                            {% assign pagina = mitad[1] %}
                            {% assign orden = pagina | minus: 24 %}
                            {% assign transc = site.texts | where: "order", orden | first %}
                            <a href="{{ site.baseurl }}{{ transc.url }}">{{ parrafo }}</a>
                        {% elsif primera == "[" and ultima == "]" and agarrar == "." %}
                            <p><i>Folio blanco.</i></p>
                        {% else %}
                            {% if parrafo_p contains "[nota]" %}
                                {% assign notas = parrafo_p | split: "[nota]" %}
                                {% assign suma = notas.size | minus: 1 %}
                                {% for i in (1..suma) %}
                                    {% assign sum_notas = sum_notas | plus: 1 %}
                                    <p id="{{ texto.order}}_{{ num_sec }}_nota_{{ sum_notas }}">note</p>
                                {% endfor %}
                            {% else %}
                                <p></p>
                            {% endif %}
                        {% endif %}
                    {% endfor %}
                </div>
                <div class="parrafo">
                    {% for bloque in parrafos %}
                        {% assign parrafo_p = bloque %}
                        {% assign parrafo = bloque | strip_html | strip %}
                        {% assign primera = parrafo | slice: 0 %}{% assign ultima = parrafo | slice: -1 %}
                        {% assign agarrar = parrafo | slice: -2 %}
                        {% assign nota = parrafo | slice: 0, 4 %}
                        {% if primera != "[" and ultima != "]" %}
                            {% if parrafo_p contains "[nota]" %}
                                {% assign parrafo_p = parrafo_p | remove: "/[nota]/" %}
                            {% endif %}
                            {% unless parrafo_p contains "[nota_" %}
                                {{ parrafo_p | markdownify}}
                            {% else %}
                                {% assign nom_nota = parrafo | slice: 2, 6 %}
                                {% assign texto_a_quitar = "/[" | append: nom_nota | append: "]/" %}
                                {% assign parrafo_p = parrafo_p | remove: texto_a_quitar %}
                                <script>
                                    function imprimir_nota(id, texto) {
                                        const lugar = document.getElementById(id);
                                        if (lugar) {
                                            lugar.innerHTML = texto;
                                        }
                                    }
                                    imprimir_nota("{{texto.order}}_{{ num_sec}}_{{ nom_nota }}", {{ parrafo_p | jsonify }});
                                </script>
                            {% endunless %}
                        {% elsif primera == "[" and ultima == "]" and agarrar == "." %}
                            <br><br><br>
                        {% endif %}
                    {% endfor %}
                </div>
                -->
            {% endfor %}
            <!--{{sum_notas}}-->
        </div>
    {% endfor %}
</article>

<script>
    //Código es para esconder el texto de un archivo de prueba.
    document.getElementById("100").style.display = 'none';
</script>

<!--
<article>
    --Poner los archivos en la carpeta «textos-modernos» en orden--
    {% assign txts_ordenados = site.textos-modernos | sort: "order" %}
    {% for texto in txts_ordenados %}
        <div>
            --El "id" del título, Dios mediante, ayudará con los enlaces del tabla de contenidos--
            <h1 id="{{ texto.order }}">{{ texto.title }}</h1>
            --Dividir el texto de sendos documentos en secciones por página--
            {% assign secciones = texto.content | split: '-- SPLIT --' %}
            {% for seccion in secciones %}
                --Para organizar la sección (página) que contiene subtítulo(s), número de folio, y notas de margen junto con el texto principal--
                {% assign parrafos = seccion | split: '</p>' %}
                {% for parrafo in parrafos %}
                    1. {{ parrafo }}
                {% endfor %}
            {% endfor %}
        </div>
    {% endfor %}
</article>
-->