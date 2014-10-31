---
layout: post
title: aws ec2 ubuntu nginx passenger rails4 capistrano setup
published: True
categories: ['rails']
tags: ['rails']
---

#authorized_keys
	cat ~/.ssh/id_rsa.pub | ssh -i ubuntu@ "cat >> ~/.ssh/authorized_keys"

# aws ubuntu - authorized_keys backup

	ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCoULQjwPSVk1ShUZodAHxyd4UqZM4ui2Ca+Ya1D+GRtioEwUzaVej7F6n9HGzm4vNcWu+7MSDWVkDt9TnwD6pDye0svqA8Xj0GvznwMW8kFk0UFVI0EPTXqVeo03iA3aqtv9rbwig75aJwOxZi1kYmmdjJnjo41txJg2z75AdqNhk5dO81bc77wxlfWmKOMjUePIM+mV5mA4Es/IiWFim45WgTI164vIo5k4qvZo6D43PED32BnvIZmVTfptPHIHnUJVqwOHU8r2CHafjkcks3iTzJ9Dqlek+KFN+IHRbJO+9e0DTfQUj9HludpH14o5HbUsUKtx2x7KUMB2P7j3sN AWS_78labandroid_key

# add authorized_key to server

	echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCv6pZ+69bkln0zX8E7QYWUrA01+i1GeBMwgRX396eKfu2+0fBF27K/ZIFT9k1sULa84kG3UpnYsg50J/iLVLN5TSDmnzz0/5WbY1+dVwOgRb7vaNc6InfXRh4eXLw9vOEWtrIVNYJ8tMNIdzFoq4i1NNlgmrP+P+j3/tyJfKyf93Rhaa5wIIwADlDWxfKsmViF16IjZqWfOtmNkVWntXzPmlGbWErS17XxzCokQWzM3U7n1QSvny1v+qOvajjmlvKrPqNRQ4CZYkMX3mCKG/GIbuP57boUPcoYGgRfH4owYNRhD3Kbn7hJjYSfMBXC8wcjN5xVBGqORZshvdnfXtyJ /Users/78lab/.ssh/id_rsa" >> .ssh/authorized_keys

# check ssh
	ssh deployer@54.64.121.127 'hostname; uptime'

# repo setup

	ssh -A deployer@54.64.121.127 'git ls-remote git@github.com:78lab/foobar.git'



# nginx setting

	sudo rm /etc/nginx/sites-enabled/default
	sudo ln -nfs /home/deployer/apps/foobar/current/config/nginx.conf /etc/nginx/sites-enabled/foobar
	sudo service nginx start



# default nginx
	/etc/nginx/sites-enabled/default

	server {
	        listen 80 default_server;
	        listen [::]:80 default_server ipv6only=on;

	        server_name test.78lab.com;
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




