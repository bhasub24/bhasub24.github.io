---
layout: archive
title: "Thoughts"
permalink: /thoughts/
author_profile: true
---
<!-- Managed by apply_blog_changes.py; do not remove if rerunning. -->

Reflections on life, philosophy, books, consciousness, and the questions that stay with me.

{% assign selected_posts = site.blog | where: "section", "thoughts" | sort: "date" | reverse %}

{% if selected_posts.size > 0 %}
<div class="entries-list">
  {% for post in selected_posts %}
    {% include archive-single.html type="list" %}
  {% endfor %}
</div>
{% else %}
No articles yet. New writing will appear here.
{% endif %}
