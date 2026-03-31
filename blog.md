---
layout: archive
title: Blog
permalink: /blog/
---

This blog is intended to document my process learning data science and related subjects, but I might write about other things too.

<div class="entries-list">
  {% for post in site.posts %}
    <article class="list__item">
      <h2 class="archive__item-title">
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </h2>
      <p class="page__meta"><i class="far fa-clock" aria-hidden="true"></i> {{ post.date | date: "%d %B %Y" }}</p>
    </article>
  {% endfor %}
</div>