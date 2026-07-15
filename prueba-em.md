---
layout: page
title: prueba
body_class: wide-page
hide_title: true
---
<article>
    {% assign txts_ordenados = site.textos-modernos | sort: "order" %}
    {% for texto in txts_ordenados %}
        <div id="{{ texto.order }}" class="contenido">
            <h1 style="margin-left: 8rem;">{{ texto.title }}</h1><br>
            {% assign secciones = texto.content | split: "<!-- SPLIT -->" %}
            {% for seccion in secciones %}
                {% assign pagina = nil %}
                {% assign sum_notas = 0 %}
                {% assign num_sec = forloop.index %}
                {% assign parrafos = seccion | split: "</p>" %}
                {% for bloque in parrafos %}
                    {% assign parrafo = bloque | strip_html | strip %}
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
                                        <p id="{{ texto.order}}_{{ num_sec }}_nota_{{ sum_notas }}">note</p>
                                    {% endfor %}
                                {% endif %}
                            </div>
                            <div class="parrafo">
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