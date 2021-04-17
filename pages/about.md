---
layout: page
title: "About"
permalink: /about/
weight: 3
---

<h1>About Me</h1>

Hi, I am **{{ site.author.name }}** :wave:,<br>
I recently got my Master's degree in Data Science, and I'm actively looking for a postion where I can apply all the skills I have learned, Python, SQL, Tableau, Machine Learning, Statitics. I am open to learn more things too. 

<h2>Skills</h2> 

<div class="row">
{% include about/skills.html}
</div>

<div class="row">
{% include about/timeline.html title="Education" source=site.data.timeline %}
</div>
