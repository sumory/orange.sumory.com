---
title: Releases
permalink: /releases/
redirect_from: /releases.html
layout: releases
---

{% for release in site.data.releases %}


<h3 id="{{ release.version }}"><a href="#{{ release.version }}">{{ release.version }}</a> <em>{{ release.created_at | date: '%B %d, %Y' }}</em></h3>


{{ release.body }}

<hr>

{% endfor releases %}
