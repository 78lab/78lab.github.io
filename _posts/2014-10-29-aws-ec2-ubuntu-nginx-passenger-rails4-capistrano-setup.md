---
layout: post
title: aws ec2 ubuntu nginx passenger rails4 capistrano setup
published: false
categories: ['rails']
tags: ['rails']
---

#copy authorized_keys to server
	78lab-ui-MacBook-Air:~ 78lab$ brew install ssh-copy-id
	78lab-ui-MacBook-Air:~ 78lab$ ssh-copy-id deploy@hostname

# check ssh
	ssh deployer@hostname 'hostname; uptime'

# repo setup

	ssh -A deployer@hostname 'git ls-remote git@github.com:78lab/foobar.git'



# nginx setting

	sudo rm /etc/nginx/sites-enabled/default
	sudo ln -nfs /home/deployer/apps/foobar/current/config/nginx.conf /etc/nginx/sites-enabled/foobar
	sudo service nginx start



# default nginx
	/etc/nginx/sites-enabled/default

	server {
	        listen 80 default_server;
	        listen [::]:80 default_server ipv6only=on;

	        server_name hostname;
	        passenger_enabled on;
	        rails_env    production;
	        root         /home/deployer/apps/foobar/current/public;

	        # redirect server error pages to the static page /50x.html
	        error_page   500 502 503 504  /50x.html;
	        location = /50x.html {
	            root   html;
	        }
	}

# rake secret
	echo "export SECRET_KEY_BASE=XXXX904d90afcace62e3a4c2c97394785d67220e26f5462XXXXX" >> .bash_profile




