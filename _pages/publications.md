---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

Updating the page soon with publications that are in progress


{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
