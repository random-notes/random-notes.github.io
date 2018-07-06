---
layout: page
title: RAndom-NoTes
---

Yet another software engineer blog on machine learning and research.

## Recent posts
<ul class="posts">
{% for post in site.posts limit: 10 %} 
  <div class="post_info">
    <li>
         <a href="{{ post.url }}">{{ post.title }}</a>
         <span>({{ post.date | date:"%Y-%m-%d" }})</span>
    </li>
   </div>
{% endfor %}
</ul>
