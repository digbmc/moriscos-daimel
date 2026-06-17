---
layout: page
title: This is a test
hide_in_nav: true
---
<div class="row">
    <div class="column">
    <!--
    {% assign img = "Garcia_Maior_2_1r.jpg" %}
    <img src="{{ site.baseurl }}/images/{{ img }}" alt="">
    -->
    </div>
    <div class="column">
    {% assign chapter = site.texts | where: "title", "Folder 2, 1r" | first %}
    {{ chapter.content }}
    </div>
<div>
