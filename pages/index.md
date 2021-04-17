---
layout: page
permalink: /
---

{% include landing.html %}

# **About Me**

Hi, I am **{{ site.author.name }}** :wave:,<br>
I'm interested in Machine Learning and Data Analysis. I also study Deep Learning (especially, computer vision) during my free time. 

<div class="row">
{% include about/skills.html title="Programming Skills" source=site.data.programming-skills %}
{% include about/skills.html title="Other Skills" source=site.data.other-skills %}
</div>

<div class="row">
{% include about/timeline.html %}
</div>