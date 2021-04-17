---
layout: page
title: "About"
permalink: /about/
weight: 3
---

# **About Me**

Hi, I am **{{ site.author.name }}** :wave:,<br>
Data Scientist. [Resume](https://docs.google.com/document/d/1fGWn-KhfxWK-BLcwFPs5UFRZdUEeyoefi2WtKozr8hM/edit#heading=h.wj0puh61kxsr) 

<div class="row">
{% include about/skills.html title="Programming Skills" source=site.data.programming-skills %}
{% include about/skills.html title="Other Skills" source=site.data.other-skills %}
</div>

<div class="row">
{% include about/timeline.html %}
</div>
