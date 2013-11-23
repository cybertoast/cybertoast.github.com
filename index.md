---
layout: page
title: 
---
<ul class="posts">
  {% for post in site.posts %}
    <li>
        <h2 class="title"><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h2>
        <span class="date">{{ post.date }}</span> 
        <p class="content">
        {{ post.content }}
        </p>
    </li>
  {% endfor %}
</ul>


