---
layout: page
title: Cyber Crumbs
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
    <li>
        <span class="date">{{ post.date | date_to_string }}</span> 
        <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>
        <p class="content">
        {{ post.content }}
        </p>
    </li>
  {% endfor %}
</ul>


