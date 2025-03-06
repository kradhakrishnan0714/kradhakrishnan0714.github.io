---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: home
title: Karthik Radhakrishnan's Portfolio
author_profile: true
---

# Welcome to My Portfolio

Here are some of my projects:

{% for project in site.projects %}
- [{{ project.title }}]({{ project.url }})
  - {{ project.description }}
{% endfor %}
