---
layout: archive
permalink: /projects/
title: "Projects by tags"
author_profile: true
header: 
  image: "/images/projects.jpg"
---
{% include group-by-array collection=site.posts field="tags" %}
{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ tag | slugify }}" class="archive__subtitle">{{ tag }}</h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}

<div class ="project-deck">
  <div class ="project-image">
  <img class ="project-deck-top" src = "" alt = "University Home Pricing">
        <div class ="project-block">
        <h4 class = "card-title"> University Home Pricing</h4>
             <p class ="project-text"> == $0
               "an"
             </p>
			 </div>
        <div class ="project-buttons">
           <a href="https://veerendrababum.github.io/datapreparation/" target = "_blank" class ="btn btn-primary">previous section</a>
         </div>
        <div class ="project-footer">
             <small class ="text-muted">data analysis, machine learning</small>
          </div>

          </div>
</div>