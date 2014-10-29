---
layout: post
title: jekyll with SEO friendly url
published: True
categories: ['jekyll']
tags: ['jekyll']
---

`_layouts/posts.html` 아래와 같이 설정 시도하였으나


{% highlight ruby %}
---
layout: default
comments: true
permalink: /:title
---
{% endhighlight %}

결과 실패.

`_config.yml` 에 다음 추가

{% highlight  %}
permalink: /blog/:title
{% endhighlight %}