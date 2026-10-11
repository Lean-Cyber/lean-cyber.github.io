---

title: "Essays"
layout: default
---

{%- comment -%} Collect every unique tag used across essays {%- endcomment -%}
{%- assign tag\_string = "" -%}
{%- for essay in site.essays -%}
{%- for tag in essay.tags -%}
{%- assign tag\_string = tag\_string | append: tag | append: "|" -%}
{%- endfor -%}
{%- endfor -%}
{%- assign all\_tags = tag\_string | split: "|" | uniq | sort\_natural -%}

{% if all\_tags.size > 0 %}

<div class="tag-filter" id="tag-filter" role="group" aria-label="Filter essays by tag" hidden>
  <button type="button" class="filter-btn is-active" data-filter="all" aria-pressed="true">All</button>
  {% for tag in all\_tags %}
  <button type="button" class="filter-btn" data-filter="{{ tag | slugify }}" aria-pressed="false">{{ tag }}</button>
  {% endfor %}
</div>
{% endif %}

<div class="essay-list" id="essay-list">
{% for essay in site.essays %}
  <div class="essay-item" data-tags="{% for tag in essay.tags %}{{ tag | slugify }}{% unless forloop.last %} {% endunless %}{% endfor %}">
    <h2 class="essay-title">
      <a href="{{ essay.url | relative\_url }}">{{ essay.title }}</a>
      {% include tags.html tags=essay.tags %}
    </h2>

&#x20;   <p class="essay-meta">
      {{ essay.date | date: "%B %d, %Y" }} ·
      {% assign words = essay.content | number\_of\_words %}
      {% assign reading\_time = words | divided\_by:200 | ceil %}
      {{ reading\_time }} min read
    </p>

    <p class="essay-description">{{ essay.excerpt }}</p>

</div>
{% endfor %}
</div>

<p class="filter-empty" id="filter-empty" hidden>No essays with this tag yet.</p>

<script>
(function () {
  var bar = document.getElementById('tag-filter');
  if (!bar) return;
  var buttons = bar.querySelectorAll('.filter-btn');
  var items = document.querySelectorAll('#essay-list .essay-item');
  var empty = document.getElementById('filter-empty');
  bar.hidden = false; // only show filter controls when JS is available

  function apply(filter, updateHash) {
    var shown = 0;
    items.forEach(function (item) {
      var tags = (item.getAttribute('data-tags') || '').split(' ');
      var match = filter === 'all' || tags.indexOf(filter) !== -1;
      item.hidden = !match;
      if (match) shown++;
    });
    buttons.forEach(function (btn) {
      var on = btn.getAttribute('data-filter') === filter;
      btn.classList.toggle('is-active', on);
      btn.setAttribute('aria-pressed', on ? 'true' : 'false');
    });
    empty.hidden = shown > 0;
    if (updateHash) {
      var url = filter === 'all' ? location.pathname + location.search : '#' + filter;
      history.replaceState(null, '', url);
    }
  }

  buttons.forEach(function (btn) {
    btn.addEventListener('click', function () {
      apply(btn.getAttribute('data-filter'), true);
    });
  });

  // Support shareable links like /essays/#startup, including back/forward
  function fromHash() {
    var wanted = decodeURIComponent(location.hash.replace('#', '')) || 'all';
    var valid = Array.prototype.some.call(buttons, function (b) {
      return b.getAttribute('data-filter') === wanted;
    });
    apply(valid ? wanted : 'all', false);
  }
  window.addEventListener('hashchange', fromHash);
  fromHash();
})();

