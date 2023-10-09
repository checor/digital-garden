---
layout: page
title: Home
id: home
permalink: /
---

# Hi, I'm Sergio! 🌱

Welcome to my public notebook. I write and share notes here.

I'm using a Jekyll template available on GitHub [here](https://github.com/maximevaillancourt/digital-garden-jekyll-template).

My notes may be written in English or Spanish, like below.

# <i>Hola, ¡Soy Sergio!</i> 🌵

<i>Bienvenido a mi cuaderno público. Escribo y comparto notas aquí.</i>

<i>Utilizo una plantilla de Jekyll disponible en GitHub [aquí](https://github.com/maximevaillancourt/digital-garden-jekyll-template).</i>

<i>Mis notas pueden estar escritas en inglés o español, como arriba.</i>

<strong>Recently updated notes - <i>Últimas notas</i></strong>

<ul>
  {% assign recent_notes = site.notes | sort: "last_modified_at_timestamp" | reverse %}
  {% for note in recent_notes limit: 5 %}
    <li>
      {{ note.last_modified_at | date: "%Y-%m-%d" }} — <a class="internal-link" href="{{ note.url }}">{{ note.title }}</a>
    </li>
  {% endfor %}
</ul>

<style>
  .wrapper {
    max-width: 46em;
  }
</style>
