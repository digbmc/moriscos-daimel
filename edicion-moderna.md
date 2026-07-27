---
layout: page
title: La edición moderna
body_class: wide-page
hide_title: true
nav_order: 3
---
{% assign txts_ordenados = site.textos-modernos | sort: "order" %}
<div id="toc" class="toc">
    <h1 id="titulo">El proceso de Mayor García: la edición moderna</h1>
    <p>Tabla de contenidos</p>
    <ul>
        <li>
            <a href="#criterios">Los criterios de la edición moderna</a>
        </li>
        {% for texto in txts_ordenados %}
            <li>
                <a href="#{{ texto.order }}">
                    {{ texto.title }}
                </a>
                {% if texto.subtitulos %}
                    <ul>
                    {% for subtitulo in texto.subtitulos %}
                    {% assign ind = forloop.index | minus: 1 %}
                        {% if subtitulo == "nil" %}
                            {% for fecha in texto.fechas[ind] %}
                                <li>
                                    <a href="#{{ texto.order }}-{{ fecha }}">{{ fecha }}</a>
                                </li>
                            {% endfor %}
                        {% else %}
                            <li>
                                <a href="#{{ texto.order }}-{{ subtitulo }}">{{ subtitulo }}</a>
                                {% if texto.fechas %}
                                    <ul>
                                    {% for fecha in texto.fechas[ind] %}
                                        <li>
                                            <a href="#{{ texto.order }}-{{ fecha }}">{{ fecha }}</a>
                                        </li>
                                    {% endfor %}
                                    </ul>
                                {% endif %}
                            </li>
                        {% endif %}
                    {% endfor %}
                    </ul>
                {% elsif texto.fechas %}
                    <ul>
                    {% for fecha in texto.fechas %}
                        <li>
                            <a href="#{{ texto.order }}-{{ fecha }}">{{ fecha }}</a>
                        </li>
                    {% endfor %}
                    </ul>
                {% endif %}
            </li>
        {% endfor %}
        <li>
            <a href="#citacion">Cómo citar la edición moderna</a>
        </li>
    </ul>
</div>
<article>
    <div id="criterios" style="max-width:48rem;margin-left:auto;margin-right:auto;">
        <h1>Los criterios de la edición moderna</h1>
        <p>
            El propósito de esta edición es promover la lectura fluida del proceso en su totalidad, tanto para los estudiantes como para el campo del estudio sobre los moriscos.  
            <br><br>  
            **ORTOGRAFÍA Y SINTAXIS** <br><br>
            Por lo general, se estandariza la ortografía, la separación de palabras, la acentuación y el uso de mayúsculas según su uso actual.  
            <br><br>
            Se modernizan las conjugaciones antiguas de verbos típicos sin otra indicación, especialmente, las formas, «vido» y «porná», las cuales se modernizan a «vio» y «pondrá» respectivamente.
            <br><br>
            Sin embargo, se respeta el uso de vocabulario anticuado y de tiempos verbales antiguos, como el futuro del subjuntivo (tuviere, supiere, etc.) y el imperfecto del subjuntivo (tuviese, estuviese, etc.). Por lo general, no se edita la sintaxis para preservar las particularidades del estilo inquisitorial y se preservan las colocaciones originales de los pronombres.  
            <br><br>
            Los pocos errores menores del notario, como la repetición de una palabra, se corrigen sin otra indicación. Se mantienen los errores que pueden indicar cambios significativos en el uso del lenguaje, particularmente cuando se trata de la transcripción de un vocablo arábigo en castellano. Por ejemplo, «el alheña» en vez de su forma actual, «la alheña».  
            <br><br>                
            **ENUMERACIÓN Y FORMATO** <br><br>
            En los márgenes se encuentra el número de la imagen digitalizada por Kislak, seguido por el folio del manuscrito. Por ejemplo, [25: 1r] corresponde a la imagen 25 en la colección digital y es el recto del primer folio del proceso.  <br><br>          
            Por lo general, se preservan los párrafos y la (falta de) puntuación con las excepciones siguientes:       
                <ul> 
                    <li>
                Para facilitar el entendimiento de las transiciones entre las voces del texto, se introducen comillas «» cuando es evidente que una voz cita a otra.
                </li>
                    </ul>        
            <ul> 
                <li>
                Se añaden párrafos nuevos cuando la voz es distinta (por ejemplo, para aclarar la diferencia entre la pregunta que propone un inquisidor y la respuesta del testigo).
            </li>
                </ul>        
            <ul> 
                <li>
                Cuando una palabra está dividida entre dos folios, se escribe la palabra completa en el folio en el cual empieza.
            </li>
                </ul> 
            Las notas de pie que siguen una enumeración con numerales arabigos (1, 2, 3 etc.) corresponden a las notas marginales del manuscrito y aparecen en una aproximación de su ubicación en el manuscrito. Las que aparecen con letras (a. b. c. etc.) reflejan nuestros comentarios editoriales para aportar contexto lingüístico y/o histórico. Las traducciones del latín provienen de Nicholas Caraballo. Le agradecemos a María Alejandra Peñuela Hoyos por su apoyo con las transcripciones y traducciones del árabe. 
        </p>
    </div>
    <!--En: First loop goes through all the .md files in textos-modernos (organized by chapter) in order-->
    <!--Es: El primer bucle revisa todos los archivos .md en textos-modernos (organizados por capítulo) en orden-->
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
                    {% if bloque_p contains "</h2>" %}
                        {% assign subtitulos = bloque_p | split: "</h2>" %}
                        {% assign subtitulo = subtitulos[0] | strip_html | strip %}
                        {% assign bloque_p = subtitulos[1] %}
                    {% endif %}
                    {% if bloque_p contains "</h3>" %}
                        {% assign fechas = bloque_p | split: "</h3>" %}
                        {% assign fecha = fechas[0] | strip_html | strip %}
                        <!--EN: bloque_p is in case an <h3> follows; ES: bloque_p es en el case de que <h3> siga-->
                        {% assign bloque_p = fechas[1] %}
                        {% assign parrafo = fechas[1] | strip_html | strip %}
                    {% else %}
                        {% assign parrafo = bloque_p | strip_html | strip %}
                    {% endif %}
                    {% if bloque_p contains "</h4>" %}
                        {% assign testigos = bloque_p | split: "</h4>" %}
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
                        {% if bloque contains "</h2>" or bloque contains "</h3>" %}
                            <div class="row">
                                <div class = "margen">
                                    <p></p>
                                </div>
                                <div class="parrafo">
                                    {% if subtitulo != nil %}
                                        <h2 id="{{ texto.order }}-{{ subtitulo }}">{{ subtitulo }}</h2>
                                        {% assign subtitulo = nil %}
                                    {% endif %}
                                    {% if fecha != nil %}
                                        <h3 id="{{ texto.order }}-{{ fecha }}">{{ fecha }}</h3>
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
                                    <a href="{{ site.baseurl }}{{ transc.url }}" id="{{ num_pagina }}">{{ pagina }}</a>
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
                                    <h4 style="text-align:center;">{{ testigo }}</h4>
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
                                                    <span class="tooltip" tabindex="0">
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
        </div>
    {% endfor %}
</article>
<a href="#" class="back-to-top" aria-label="Volver arriba">
    ↑
</a>
<br>
<hr>
<div style="max-width:38rem;margin-left:auto;margin-right:auto;">
    <p id="citacion">
    <em>Cómo citar la edición moderna:</em> Alejandre Lamas-Nemec and Kathryn Phipps. El proceso de Mayor García: la edición moderna. Bryn Mawr College, 2026. Web.
    </p>
</div>