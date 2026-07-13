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
                {% assign parrafos = seccion | split: "</p>" %}
                <div class="margen">
                    {% for bloque in parrafos %}
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
                            <p></p>
                        {% endif %}
                    {% endfor %}
                </div>
                <div class="parrafo">
                    {% for bloque in parrafos %}
                        {% assign parrafo = bloque | strip_html | strip %}
                        {% assign primera = parrafo | slice: 0 %}{% assign ultima = parrafo | slice: -1 %}
                        {% assign agarrar = parrafo | slice: -2 %}
                        {% if primera != "[" and ultima != "]" %}
                            <p>{{ parrafo }}</p>
                        {% elsif primera == "[" and ultima == "]" and agarrar == "." %}
                            <br><br>
                        {% endif %}
                    {% endfor %}
                </div>
            {% endfor %}
        </div>
    <hr>
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