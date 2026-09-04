---
layout: page
permalink: /publications/
title: publications
description: Publications, preprints, and work in preparation.
nav: true
nav_order: 1
---

{% include bib_search.liquid %}

<div class="publications">
  {% capture published_bib %}{% bibliography --query @*[pubstate=published] --group_by none %}{% endcapture %}
  {% if published_bib contains "<li>" %}
    <h2 class="category">Published</h2>
    {{ published_bib }}
  {% endif %}

  <h2 class="category">Preprints</h2>
  {% bibliography --query @*[pubstate=preprint] --group_by none %}

  {% if site.data.in_preparation.size > 0 %}
    <h2 class="category">In preparation</h2>
    {% assign self_name = site.first_name | append: " " | append: site.last_name %}
    <ol class="bibliography">
      {% for item in site.data.in_preparation %}
        <li>
          <div class="row">
            {% if site.enable_publication_thumbnails %}
              <div class="col col-sm-2 abbr">
                {% if item.preview %}
                  {% if item.preview contains "://" %}
                    <img class="preview z-depth-1 rounded" src="{{ item.preview }}" alt="{{ item.preview }}">
                  {% else %}
                    {% assign preview_path = item.preview | prepend: "/assets/img/publication_preview/" %}
                    {% include figure.liquid loading="eager" path=preview_path sizes="200px" class="preview z-depth-1 rounded" zoomable=true avoid_scaling=true alt=item.preview %}
                  {% endif %}
                {% endif %}
              </div>
            {% endif %}
            <div class="{% if site.enable_publication_thumbnails %}col-sm-8{% else %}col-sm-10{% endif %}">
              <div class="title">{{ item.title }}</div>
              <div class="author">
                {% for author in item.authors %}
                  {% if forloop.first == false %}
                    {% if forloop.length == 2 %} and {% else %}, {% if forloop.last %}and {% endif %}{% endif %}
                  {% endif %}
                  {% if author == self_name %}
                    <em>{{ author }}</em>
                  {% else %}
                    {{ author }}
                  {% endif %}
                {% endfor %}
              </div>
              <div class="periodical">
                <em>{{ item.status }}</em>
              </div>
            </div>
          </div>
        </li>
      {% endfor %}
    </ol>
  {% endif %}
</div>
