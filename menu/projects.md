---
layout: page
title: Pet projects
---
<ul class="posts">
    {% for project in site.projects %}

        <div class="posts">
            <h1>
                <a href="{{ site.github.url }}{{ project.url }}">{{ project.title }}</a>
            </h1>
            {% if project.image %}
                <div class="thumbnail-container">
                    <a href="{{ site.github.url }}{{ project.url }}"><img src="{{ site.github.url }}/assets/img/projects/{{ project.image }}"></a>
                </div>
            {% endif %}
            <p>
                {{ project.content | strip_html | truncate: 350 }} <a href="{{ site.github.url }}{{ project.url }}">Read more</a>
            </p>
        </div>

    {% endfor %}
</ul>
