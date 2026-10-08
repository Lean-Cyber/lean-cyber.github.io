---
title: "Essays"
layout: default
---

<div class="essay-list">
{% for essay in site.essays %}
  <div class="essay-item">
    <h2 class="essay-title">
      <a href="{{ essay.url | relative_url }}">{{ essay.title }}</a>
    </h2>

    <p class="essay-meta">
      {{ essay.date | date: "%B %d, %Y" }} ·
      {% assign words = essay.content | number_of_words %}
      {% assign reading_time = words | divided_by:200 | ceil %}
      {{ reading_time }} min read
    </p>

    <p class="essay-description">{{ essay.excerpt }}</p>
  </div>
{% endfor %}
</div>

