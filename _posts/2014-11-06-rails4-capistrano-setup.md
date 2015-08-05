---
layout: post
title: rails4 capistrano3 setup
published: false
categories: ['rails']
tags: ['rails']
---


This is a very common issue where we get error “Error installing mysql2: Failed to build gem native extension” while installing mysql2 gem in Rails(in Ubuntu Linux OS). The problem is due to missing libmysql(from MYSQL) file.

Linux Solution:

      To solve the issue install the compnents libmysql-ruby and libmysqlclient-dev using the command sudo apt-get install libmysql-ruby libmysqlclient-dev (Run in Ubuntu Terminal). Then run gem install mysql2 which should run successfully.