---
layout: post
title: ubuntu mysql 원격 접속 설정
published: True
categories: ['rails']
tags: ['rails']
---


#first check status: mysql 원격접속을 위한 포트가 열려있는지 확인
	netstat -tulpen

#entry mysql to give privileges 
	GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'mypassword'; 
	FLUSH PRIVILEGES;


#기본 문자셋 utf8로 설정
	$ sudo vi /etc/mysql/my.cnf

	[mysqld]
	character-set-server = utf8
	collation-server = utf8_general_ci
	#원격 접속 설정
	bind-address = 0.0.0.0
	#DB한 행의 최대크기 수정
	max_allowed_packet    = 256M

#MySQL 재시작
	$ sudo service mysql restart


#기본 문자셋 확인

	$ mysql -uroot -p
	mysql> status
	Server characterset:	utf8
	Db     characterset:	utf8

	mysql> show variables like '%char%';
	+--------------------------+----------------------------+
	| Variable_name            | Value                      |
	+--------------------------+----------------------------+
	| character_set_client     | utf8                       |
	| character_set_connection | utf8                       |
	| character_set_database   | utf8                       |
	| character_set_filesystem | binary                     |
	| character_set_results    | utf8                       |
	| character_set_server     | utf8                       |
	| character_set_system     | utf8                       |
	| character_sets_dir       | /usr/share/mysql/charsets/ |
	+--------------------------+----------------------------+


#아파치 웹서버 설정
	/etc/apache2/sites-available/xxx

	<VirtualHost *:80>

	ServerName xxx.xxx.com

	#document Root
	DocumentRoot /home/deploy/www/
	#additional setting
	<Directory /home/deploy/www/>
	Options FollowSymLinks MultiViews
	AllowOverride All
	Order allow,deny
	allow from all
	</Directory>
	AssignUserID deploy deploy
	</VirtualHost>

#참조

http://blog.lael.be/post/73