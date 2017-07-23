---
layout: page
show-avatar: true
title: Default
date: '2017-07-23T18:12:02+00:00'
---


{% for post in paginator.posts %}

{{ post.title }}

{% if post.subtitle %}

{{ post.subtitle }}

{% endif %}

Posted on {{ post.date | date: "%B %-d, %Y" }}

{% if post.image %}

{% endif %}

{{ post.excerpt | strip_html | xml_escape | truncatewords: site.excerpt_length }} {% assign excerpt_word_count = post.excerpt | number_of_words %} {% if post.content != post.excerpt or excerpt_word_count > site.excerpt_length %} [Read More] {% endif %}

{% if post.tags.size > 0 %}

Tags: {% if site.link-tags %} {% for tag in post.tags %} {{ tag }} {% endfor %} {% else %} {{ post.tags | join: ", " }} {% endif %}

{% endif %}

{% endfor %}

{% if paginator.total_pages > 1 %}

{% if paginator.previous_page %}

← Newer Posts

{% endif %} {% if paginator.next_page %}

Older Posts →

{% endif %}

{% endif %}