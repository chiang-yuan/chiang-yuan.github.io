---
layout: post
title: Solve spinodal and binodal curves using Julia
description: 
date: 2023-07-23 08:00:00-0800
tags: julia thermodynamics materials math
categories: hack
giscus_comments: true
---

{::nomarkdown}
{% assign jupyter_path = 'assets/jupyter/spinodal.ipynb' | relative_url %}
{% capture notebook_exists %}{% file_exists assets/jupyter/blog.ipynb %}{% endcapture %}
{% if notebook_exists == 'true' %}
  {% jupyter_notebook jupyter_path %}
{% else %}
  <p>Sorry, the notebook you are looking for does not exist.</p>
{% endif %}
{:/nomarkdown}
