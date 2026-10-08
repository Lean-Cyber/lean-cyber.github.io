---
title: "Lean Cyber"
layout: default
---

<div class="home-intro">
  <h1 class="home-title">Lean Cyber</h1>
  <p class="home-tagline">
    A discipline for building security architectures that reduce real risk without unnecessary complexity.
  </p>
  <p class="home-description">
    Lean Cyber focuses on right‑sized controls, operational clarity, and architectural simplicity.  
    Security should be proportionate, intentional, and sustainable.
  </p>
</div>

<div class="home-section">
  <h2 class="section-heading">Essays</h2>

  <div class="essay-list">
    {% for essay in site.essays %}
      <div class="essay-item">
        <h3 class="essay-title">
          <a href="{{ essay.url | relative_url }}">{{ essay.title }}</a>
        </h3>

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
</div>
