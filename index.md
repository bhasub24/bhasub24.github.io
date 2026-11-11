---
layout: single
author_profile: true
title: ""
author: bhasub24
---

(The site is still under construction)

I'm a PhD student in Computational Mathematics, Science and Engineering at Michigan State University, working with Professor Saiprasad Ravishankar. My research interests include computational imaging, inverse problems, self-supervised denoising, and the learning dynamics of deep neural networks.

Broadly, I’m interested in self-supervised learning, inverse problems, information theory, statistical learning theory, and reinforcement learning. Currently, I’m exploring how to transfer knowledge from simulation to real microscopy denoising, how synthetic data can improve self-supervised models, and how to achieve robust generalization under distribution shifts.

<!-- BEGIN recent-writing: apply_blog_changes.py -->

## Recent writing

{% assign recent_writing = site.blog | sort: "date" | reverse %}
{% if recent_writing.size > 0 %}
<div class="entries-list">
  {% for post in recent_writing limit: 3 %}
    {% include archive-single.html type="list" %}
  {% endfor %}
</div>
{% else %}
Articles will appear here as they're published.
{% endif %}

Browse [Research]({{ '/research/' | relative_url }}) or [Thoughts]({{ '/thoughts/' | relative_url }}).
<!-- END recent-writing: apply_blog_changes.py -->
