---
layout: page
title: PowerShell
published: false
---
<h3>All Posts</h3>
<table>
{% for post in site.posts %}
<tr>
    <td> {{ post.date | date: "%m/%d/%Y" }} </td>
    <td><a href="https://itops.pt{{ post.url | remove: 'index.html' }}">{{ post.title }}</td> 
    <td>{% for tag in post.tags %} {{ tag }} {% endfor %}</td>

</tr>    
{% endfor %}
</table>
<h3>All Pages</h3>
{% for page in site.pages %} {% if page.layout != nil %} {% if page.layout != 'feed' %}
<br /> <a href="https://itops.pt{{ post.url | remove: 'index.html' }}">{{ post.title }}</a>
{% endif %} {% endif %} {% endfor %}