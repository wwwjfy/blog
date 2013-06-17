---
layout: page
title: Initial Commit
---
{% include JB/setup %}

I'm a developer in Beijing. I use Mac, am interested in #python, #golang, #c, #objc.

I write posts here infrequently.

#### Recent Posts

<ul class="posts">
  {% for post in site.posts limit:5 %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

[Archive >>]({{ site.JB.archive_path }})
