---
layout: page
title: Archives
---

<!-- ## Blog Posts -->

<!-- {% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %} -->

{% for post in site.posts %}
<div>
  <span style="float: left;"><a href="{{ post.url }}">{{ post.title }}</a></span>
  <span style="float: right;">{{ post.date | date_to_string }}</span>
</div>
<div style="clear: both;"></div>​
{% endfor %}