---
layout: page
title: Texto de la edición moderna
body_class: wide-page
hide_title: true
hide_in_nav: true
---
<article>
    <!--En: First loop goes through all the .md files in textos-modernos (organized by chapter) in order-->
    <!--Es: El primer bucle revisa todos los archivos .md en textos-modernos (organizados por capítulo) en orden-->
    {% assign txts_ordenados = site.textos-modernos | sort: "order" %}
    {% for texto in txts_ordenados %}
        <!--En: This <div> keeps the data of each file together-->
        <!--Es: Este <div> guarda juntos los datos de cada archivo-->
        <div id="{{ texto.order }}" class="contenido">
            <!--<div><h1 style="margin-left:16rem;text-align:center;">{{ texto.title }}</h1></div><br>-->
            <div class="row">
                <div class="margen">
                    <p></p>
                </div>
                <div class="parrafo">
                    <h1 style="text-align: center;">{{ texto.title }}</h1>
                </div>
            </div>
            <!--En: Because chapters are not organized by page, but manuscripts are, a marker is used to divide the chapter into sections-->
            <!--Es: Dado que los capítulos no se organizan por página como los manuscritos, se utiliza un marcador para dividir el capítulo en secciones-->
            {% assign secciones = texto.content | split: "<!-- SPLIT -->" %}
            {% for seccion in secciones %}
                {% assign pagina = nil %} <!--En: Keeps the page number; Es: Guarda el número de página-->
                {% assign subtitulo = nil %}
                {% assign testigo = nil %}
                {% assign fecha = nil %}
                {% assign sum_notas = 0 %} <!--En: The number of notes in the section; Es: La suma de notas en la sección-->
                {% assign sum_enc = 0 %}
                {% assign num_sec = forloop.index %}
                <!--En: Splits the section into paragraphs; Es: Divide la sección en párrafos-->
                {% assign parrafos = seccion | split: "</p>" %}
                {% for bloque in parrafos %}
                    {% assign bloque_p = bloque %}
                    {% if bloque_p contains "</h1>" %}
                        {% assign subtitulos = bloque_p | split: "</h1>" %}
                        {% assign subtitulo = subtitulos[0] | strip_html | strip %}
                        {% assign bloque_p = subtitulos[1] %}
                    {% endif %}
                    {% if bloque_p contains "</h2>" %}
                        {% assign fechas = bloque_p | split: "</h2>" %}
                        {% assign fecha = fechas[0] | strip_html | strip %}
                        <!--EN: bloque_p is in case an <h3> follows; ES: bloque_p es en el case de que <h3> siga-->
                        {% assign bloque_p = fechas[1] %}
                        {% assign parrafo = fechas[1] | strip_html | strip %}
                    {% else %}
                        {% assign parrafo = bloque_p | strip_html | strip %}
                    {% endif %}
                    {% if bloque_p contains "</h3>" %}
                        {% assign testigos = bloque_p | split: "</h3>" %}
                        {% assign testigo = testigos[0] | strip_html | strip %}
                        {% assign parrafo = testigos[1] | strip_html | strip %}
                    {% endif %}
                    {% assign primera = parrafo | slice: 0 %}{% assign ultima = parrafo | slice: -1 %}
                    {% assign agarrar = parrafo | slice: -2 %}
                    {% if primera == "[" and ultima == "]" and agarrar != "." %}
                        {% assign pagina = parrafo %}
                    {% else %}
                        <!--EN: This mini container is meant to give headings their own place and keep certain margin notes from separate-->
                        <!--ES: Este envase es para dar a los encabezados un lugar propio y mantener ciertas notas marginales separados-->
                        {% if bloque contains "</h1>" or bloque contains "</h2>" %}
                            <div class="row">
                                <div class = "margen">
                                    <p></p>
                                </div>
                                <div class="parrafo">
                                    {% if subtitulo != nil %}
                                        <h1>{{ subtitulo }}</h1>
                                        {% assign subtitulo = nil %}
                                    {% endif %}
                                    {% if fecha != nil %}
                                        <h2>{{ fecha }}</h2>
                                        {% assign fecha = nil %}
                                    {% endif %}
                                </div>
                            </div>
                        {% endif %}
                        <!--EN: The container for the main text; ES: El contenedor para el texto principal-->
                        <div class="row">
                            <div class="margen">
                            <!--EN: If the block has headings, it aligns the margin notes with the heading instead of the main text-->
                            <!--ES: Si el bloque tiene encabezados, se alinean las notas del margen con los encabezados en vez del texto principal-->
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
                                    <p><em>Folio blanco.</em></p>
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
                                {% if testigo != nil %}
                                    <h3 style="text-align:center;">{{ testigo }}</h3>
                                    {% assign testigo = nil %}
                                {% endif %}
                                {% if parrafo != "[Folio blanco.]" %}
                                    {% if parrafo contains "[nota]" %}
                                        {% assign parrafo = parrafo | remove: "/[nota]/" %}
                                    {% endif %}
                                    {% unless parrafo contains "[nota_" %}
                                        {% if parrafo contains "[enc]" %}
                                            {% assign partes = parrafo | split: "/[enc]/" %}
                                            {% for parte in partes %}
                                                {% assign remainder = forloop.index0 | modulo: 2 %}
                                                {% if remainder == 1 %}
                                                    {% assign sum_enc = sum_enc | plus: 1 %}
                                                    <span class="tooltip">
                                                        {{ parte | strip }}
                                                        <span class="tooltiptext" id="{{ texto.order }}_{{ num_sec }}_enc_{{ sum_enc }}">
                                                            ENC.
                                                        </span>
                                                    </span>
                                                {% else %}
                                                    <span>{{ parte }}</span>
                                                {% endif %}
                                            {% endfor %}
                                            <p></p>
                                        {% endif %}
                                    {% endunless %}
                                    {% unless parrafo contains "[nota_" or parrafo contains "[enc_" or parrafo contains "[enc]" %}
                                        <p>{{ parrafo }}</p>
                                    {% endunless %}
                                    {% if parrafo contains "[nota_" %}
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
                                            if ({{ parrafo | jsonify }}.includes ("[enc]")) {
                                                document.getElementById("{{texto.order}}_{{ num_sec}}_{{ nom_nota }}").innerHTML = "";
                                            }
                                            else {
                                                imprimir_nota("{{texto.order}}_{{ num_sec}}_{{ nom_nota }}", {{ parrafo | jsonify }});
                                            }
                                            /*
                                            if ({{ parrafo | jsonify }}.includes("[enc]")) {
                                                const p = document.getElementById("{{texto.order}}_{{ num_sec}}_{{ nom_nota }}");
                                                const enc_span = document.createElement("span");
                                                enc_span.textContent = "yes";
                                                p.append(enc_span);
                                            }
                                            */
                                        </script>
                                        {% if parrafo contains "[enc]" %}
                                            {% assign partes = parrafo | split: "/[enc]/" %}
                                            {% for parte in partes %}
                                                {% assign remainder = forloop.index0 | modulo: 2 %}
                                                {% if remainder == 1 %}
                                                    {% assign sum_enc = sum_enc | plus: 1 %}
                                                    <script>
                                                            /*
                                                            const p = document.getElementById("{{text.order}}_{{ num_sec }}_{{ nom_nota }}");
                                                            const enc_span = document.createElement("span");
                                                            */
                                                        const tooltip = document.createElement("span");
                                                        tooltip.className = "tooltip";
                                                        tooltip.textContent = "{{ parte }}";
                                                        const tooltipText = document.createElement("span");
                                                        tooltipText.className = "tooltiptext";
                                                        tooltipText.id = "{{ texto.order }}_{{ num_sec }}_enc_{{ sum_enc }}";
                                                        tooltipText.textContent = "ENC.";
                                                        tooltip.appendChild(tooltipText);
                                                        document.getElementById("{{texto.order}}_{{ num_sec}}_{{ nom_nota }}").appendChild(tooltip);
                                                    </script>
                                                {% else %}
                                                    <script>
                                                        const otr_txt = document.createElement("span");
                                                        otr_txt = {{ parte }};
                                                        document.getElementById("{{texto.order}}_{{ num_sec}}_{{ nom_nota }}").appendChild(otr_txt);
                                                    </script>
                                                {% endif %}
                                            {% endfor %}
                                        {% endif %}    
                                    {% endif %}
                                    {% if parrafo contains "[enc_" %}
                                        {% assign nom_enc = parrafo | slice: 2, 5 %}
                                        {% assign texto_a_quitar = "/[" | append: nom_enc | append: "]/" %}
                                        {% assign parrafo = parrafo | remove: texto_a_quitar %}
                                        <script>
                                            function dale_enc(id, texto) {
                                                const lugar = document.getElementById(id);
                                                if (lugar) {
                                                    lugar.innerHTML = texto;
                                                }
                                            }
                                            dale_enc("{{texto.order}}_{{ num_sec}}_{{ nom_enc }}", {{ parrafo | jsonify }});
                                        </script>
                                    {% endif %}
                                {% endif %}
                            </div>
                        </div>
                    {% endif %}
                {% endfor %}
            {% endfor %}
            <!--{{sum_notas}}-->
        </div>
    {% endfor %}
</article>

<script>
    //Código es para esconder el texto de un archivo de prueba.
    document.getElementById("100").style.display = 'none';
</script>
