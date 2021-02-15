---
layout: default
---

# $ cat about.txt
{:id="about"}

This is a theme intended to use to [lampiaosec](https://lampiaosec.github.io). So, it's our taste, but because we like free culture, it's free to.

The categories to post was to support us, but you can change as you need.

# $ cat contact.txt
{:id="contact"}

I think that all about this theme is intuitive, but if you want help, please, contact me: [gjuniioor](https://github.com/gjuniioor).

# $ cat projects.txt
{:id="projects"}

<ul>
{% for project in site.categories.projects %}
<li><a href="{{ project.link }}">{{ project.title }}</a> - {{ project.description }}</li>
{% endfor %}
</ul>

# $ cat posts.txt
{:id="posts"}

<ul>
{% for post in site.categories.posts %}

{% if post.en %}
<li>{{ post.title }} :: <a href="{{ post.url }}" title="{{ post.description }}">en</a> :: <a href="{{ post.pt }}" title="{{ post.description_pt }}">pt_br</a></li>
{% endif %}

{% endfor %}
</ul>
