---
layout: archive
title: "Research"
permalink: /research/
author_profile: true
---
<!-- Managed by apply_blog_changes.py; do not remove if rerunning. -->

Research notes, explanations, and explorations in mathematics, machine learning, and computational imaging.

{% assign selected_posts = site.blog | where: "section", "research" | sort: "date" | reverse %}

{% if selected_posts.size > 0 %}
<div class="entries-list">
  {% for post in selected_posts %}
    {% include archive-single.html type="list" %}
  {% endfor %}
</div>
{% else %}
No articles yet. New writing will appear here.
{% endif %}
