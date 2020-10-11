---
layout: default
title: Posts by Tag
---
{% assign sortedTags = site.tags | sort %}

<h1>Posts by Tag</h1>
All tags: {% for tag in sortedTags %} {% assign tagName = tag | first %}{% assign postsCount = tag | last | size %}<a href="#{{ tagName }}">{{ tagName }}</a> ({{postsCount}}){% if forloop.last %}{% else %}, {% endif %}{% endfor %}

{% for tag in sortedTags %}
  {% assign tagName = tag | first %}
  {% assign posts = tag | last %}
  {% assign postsCount = posts | size %}
  <a id="{{ tagName }}" />
  <h2 style="display: inline-block;">{{ tagName }}: {{ postsCount }} posts</h2>

<ul>
  {% for post in posts %}
  <li>
    <h2>{{ post.date | date: "%Y-%m-%d" }}: <a href="{{ post.url }}">{{ post.title }}</a></h2>
    {{ post.excerpt | strip_html | normalize_whitespace | truncate: 160 | escape }}
  </li><br>
  {% endfor %}
</ul>
{% endfor %}
