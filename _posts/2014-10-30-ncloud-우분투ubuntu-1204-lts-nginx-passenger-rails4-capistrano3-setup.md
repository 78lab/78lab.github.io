---
layout: post
title: ncloud (ubuntu 12.04 LTS) nginx passenger rails4 capistrano3 setup
published: True
categories: ['rails']
tags: ['rails']
---

#SSH agent에 key가 로드됬는지 확인
	78lab-ui-MacBook-Air:~ 78lab$ ssh-add -l
	2048 57:c5:d7:xxxx:59:83:94 /Users/78lab/.ssh/id_rsa (RSA)

#만약 로드되지 않았다면 추가해 줌
	78lab-ui-MacBook-Air:~ 78lab$ $ ssh-add
	    Identity added: /Users/me/.ssh/id_rsa (/Users/me/.ssh/id_rsa)


# deploy 계정 생성
	78lab-ui-MacBook-Air:~ 78lab$ adduser deploy
	78lab-ui-MacBook-Air:~ 78lab$ adduser deploy sudo
	78lab-ui-MacBook-Air:~ 78lab$ su deploy

#ssh 접속 테스트
	78lab-ui-MacBook-Air:~ 78lab$ ssh deploy@hostname

#패스워드 입력없이 ssh 접속하기 - 필수
	78lab-ui-MacBook-Air:~ 78lab$ brew install ssh-copy-id
	78lab-ui-MacBook-Air:~ 78lab$ ssh-copy-id deploy@hostname
	/usr/local/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
	/usr/local/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
	deploy@hostname's password:

#rvm 설치
	sudo apt-get update
	sudo apt-get -y upgrade
	sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libgdbm-dev libncurses5-dev automake libtool bison libffi-dev imagemagick

	gpg --keyserver hkp://keys.gnupg.net --recv-keys D39DC0E3
	curl -L https://get.rvm.io | bash -s stable

	source ~/.rvm/scripts/rvm
	echo "source ~/.rvm/scripts/rvm" >> ~/.bashrc
	rvm install 2.1.3
	rvm use 2.1.3 --default
	ruby -v

	echo "gem: --no-ri --no-rdoc" > ~/.gemrc

#nginx with passenger 설치
	# Install Phusion's PGP key to verify packages
	gpg --keyserver keyserver.ubuntu.com --recv-keys 561F9B9CAC40B2F7
	gpg --armor --export 561F9B9CAC40B2F7 | sudo apt-key add -

	# Add HTTPS support to APT
	sudo apt-get install apt-transport-https

	# Add the passenger repository
	sudo sh -c "echo 'deb https://oss-binaries.phusionpassenger.com/apt/passenger precise main' >> /etc/apt/sources.list.d/passenger.list"
	sudo chown root: /etc/apt/sources.list.d/passenger.list
	sudo chmod 600 /etc/apt/sources.list.d/passenger.list
	sudo apt-get update

	# Install nginx and passenger
	sudo apt-get install nginx-full passenger



	sudo service nginx start

# nginx.conf 설정

	deploy@lab78-1:~$ passenger-config --root
	/usr/lib/ruby/vendor_ruby/phusion_passenger/locations.ini
	deploy@lab78-1:~$ passenger-config --ruby-command
	/home/deploy/.rvm/wrappers/ruby-2.1.3/ruby
	/usr/local/rvm/gems/ruby-2.1.3/wrappers/ruby

	sudo nano /etc/nginx/nginx.conf
	# You could also use nano if you don't like vim
	# sudo nano /etc/nginx/nginx.conf


	##
	# Phusion Passenger
	##
	# Uncomment it if you installed ruby-passenger or ruby-passenger-enterprise
	##

	passenger_root /usr/lib/ruby/vendor_ruby/phusion_passenger/locations.ini;
	passenger_ruby /home/deploy/.rvm/wrappers/ruby-2.1.3/ruby; # If use use rvm, be sure to change the version number

#mysql 설치

	sudo apt-get install mysql-server mysql-client libmysqlclient-dev

#postgresql 설치

	sudo apt-get install postgresql postgresql-contrib libpq-dev
	deploy@lab78-1:~$ sudo su - postgres
	sudo: unable to resolve host lab78-1
	postgres@lab78-1:~$ createuser --pwprompt
	Enter name of role to add: admin
	Enter password for new role: 
	Enter it again: 
	Shall the new role be a superuser? (y/n) y
	postgres@lab78-1:~$ exit


#nginx site default 수정

	sudo nano /etc/nginx/sites-enabled/default

	server {
	        listen 80 default_server;
	        listen [::]:80 default_server ipv6only=on;

	        server_name mydomain.com;
	        passenger_enabled on;
	        rails_env    production;
	        root         /home/deploy/myapp/current/public;

	        # redirect server error pages to the static page /50x.html
	        error_page   500 502 503 504  /50x.html;
	        location = /50x.html {
	            root   html;
	        }
	}

# check ssh
	ssh deploy@nc3.78lab.com 'hostname; uptime'

# SSH Agent Forwarding - github repo setup

	your-server$ ssh git@github.com
	your-local$ ssh -A deploy@nc1.78lab.com 'git ls-remote git@github.com:78lab/foobar.git'


# ENV SECRET_KEY_BASE setting 
	your-local$ rake secret
	your-server$ echo "export SECRET_KEY_BASE=XXXX904d90afcace62e3a4c2c97394785d67220e26f5462XXXXX" >> .bash_profile


#만약 에러가 발생한다면..

	deploy@nc3:~/apps$ chmod g+x,o+x foobar
	deploy@nc3:~/apps$ cd ..
	deploy@nc3:~$ chmod g+x,o+x apps
	deploy@nc3:~$ cd ..
	deploy@nc3:/home$ ls
	deploy
	deploy@nc3:/home$ chmod g+x,o+x deploy
	deploy@nc3:/home$ 

#또는 nginx.conf 에 추가
	user deploy


#cap3 setting
...



#memcached setup

	sudo apt-get update

	sudo apt-get install memcached libmemcached-dev


#root 계정으로 setup 할 경우 스크립트

	echo "* Updating system"
	apt-get update
	apt-get -y upgrade
	echo "* Installing packages"
	apt-get -y install build-essential libmagickcore-dev imagemagick libmagickwand-dev libxml2-dev libxslt1-dev git-core nginx redis-server curl nodejs htop
	apt-get -y install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libgdbm-dev libncurses5-dev automake libtool bison libffi-dev imagemagick
	id -u deploy &> /dev/null
	 
	if [ $? -ne 0 ]
	then
	  echo "* Creating user deploy"
	  adduser deploy
	  echo "* Adding user deploy to sudo group"
	  adduser deploy sudo
	else
	  echo "* deploy user already exists"
	fi
	 
	echo "* Installing rvm"
	. /etc/profile.d/rvm.sh &> /dev/null
	 
	type rvm &> /dev/null
	 
	if [ $? -ne 0 ]
	then
	  curl -L https://get.rvm.io | bash -s
	  echo "source /etc/profile.d/rvm.sh" >> /etc/bash.bashrc
	  . /etc/profile.d/rvm.sh &> /dev/null
	else
	  echo "* rvm already installed"
	fi
	 
	cat /etc/environment | grep RAILS_ENV
	if [ $? -ne 0 ]
	then
	  echo "RAILS_ENV=production" >> /etc/environment
	fi
	 
	echo "* Adding a gemrc file to deploy user"
	echo -e "verbose: true\nbulk_threshold: 1000\ninstall: --no-ri --no-rdoc --env-shebang\nupdate: --no-ri --no-rdoc --env-shebang" > /home/deploy/.gemrc
	chmod 644 /home/deploy/.gemrc
	chown deploy /home/deploy/.gemrc
	chgrp deploy  /home/deploy/.gemrc
	 
	 
	echo "* Install ruby version 2.0.0"
	ruby -v &> /dev/null
	if [ $? -ne 0 ]
	then
	  rvm install 2.1.3
	else
	  echo "* Ruby already installed"
	fi
	 
	echo "* Add user deploy to rvm group"
	usermod -a -G rvm deploy
	 
	rvm --default use 2.1.3
	ruby -v
	echo "* DONE *"
	echo "* Rebooting system *"
	reboot



# RAILS_ENV=production bundle exec rake db:create
#mysql db setup
	mysql -u root -p
	mysql> create database foobar_production;
	mysql> grant usage on *.* to deploy@localhost identified by 'password';
	mysql> grant all privileges on foobar_production.* to deploy@localhost;

#참조
[gorails]: https://gorails.com/deploy/ubuntu/12.04
[cap3-auth]: http://capistranorb.com/documentation/getting-started/authentication-and-authorisation/

