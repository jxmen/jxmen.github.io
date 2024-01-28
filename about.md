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

파편화된 기억들을 남기기 위한 위키입니다.
