---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

## Peer-reviewed

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}

## Working papers (in progress)

These are active drafts—not yet submitted. I'm happy to share early write-ups or discuss collaborations: [razahashmi93@gmail.com](mailto:razahashmi93@gmail.com).

**Pathway-Enhanced Uncertainty Estimation.** Estimating predictive uncertainty from the consistency of a model's internal pathways and prototype activations, rather than from output probabilities alone.

**Diffusion-based Uncertainty.** A sampling-driven approach to uncertainty estimation, with theoretical checks and comparisons against established baselines.

**Difficulty Is Relative.** A unified suite to measure and predict class-pair difficulty—treating "hardness" as a relationship between classes rather than a fixed per-example property.

Alongside these, I'm building evaluation tooling for distribution shift, calibration, and rejection/coverage tradeoffs.
