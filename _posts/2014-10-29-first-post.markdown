---
layout: post
title:  "첫번째 포스트"
date:   2014-10-29 15:10:10
categories: jekyll update
---

블로그 테스트입니다.

'nginx setting'

'sudo rm /etc/nginx/sites-enabled/default'
sudo ln -nfs /home/deployer/apps/foobar/current/config/nginx.conf /etc/nginx/sites-enabled/foobar
sudo service nginx start