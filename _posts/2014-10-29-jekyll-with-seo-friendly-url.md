---
layout: post
title: jekyll with SEO friendly url
published: True
categories: ['jekyll']
tags: ['jekyll']
---

`_layouts/posts.html` 아래와 같이 설정


{% highlight ruby %}
---
layout: default
comments: true
permalink: /:title
---
{% endhighlight %}