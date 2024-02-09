---
layout: page
title:
permalink: /about/
comment: false
latex: true
---
* TOC
{:toc}

<div class="contact">
{% if site.github_username %}
        <a href="https://github.com/{{ site.github_username }}">GitHub</a>
{% endif %}
<!-- {% if site.twitter_username %}
        <a href="https://twitter.com/{{ site.twitter_username }}">Twitter</a>
{% endif %} -->
{% if site.email %}
        <a href="mailto:{{ site.email }}">Email</a>
{% endif %}
        <a href="{{ "/feed.xml" | prepend: site.baseurl }}">RSS</a>
</div>

## 박주영의 위키

해당 위키는 기계인간 종립(johngrib)님의 [위키 스켈레톤](https://github.com/johngrib/johngrib-jekyll-skeleton)을 통해 만들어졌습니다.
