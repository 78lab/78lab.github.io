---
layout: post
title: rails4 nokogiri mac os x 10.9 setup
published: false
categories: ['rails']
tags: ['rails']
---

# tried these
	gem install nokogiri -- --with-xml2-lib=/opt/local/lib --with-xml2-include=/opt/local/include/libxml2 --with-iconv-dir=/usr/local/Cellar/libiconv/1.14/

	brew link libxml2 libxslt libiconv --force

	gem install nokogiri -- --use-system-libraries


	gem install nokogiri -- --with-iconv-dir=/usr/local/Cellar/libiconv/1.14/ --use-system-libraries


# this worked for me
	gem install nokogiri -- --with-xml2-include=/usr/local/Cellar/libxml2/2.9.2/include/libxml2 --with-xslt-dir=/usr/local/Cellar/libxslt/1.1.28 --with-iconv-dir=/usr/local/Cellar/libiconv/1.14/ --use-system-libraries



	sudo ln -s /etc/nginx/sites-available/momsmap.78lab.com /etc/nginx/sites-enabled/momsmap.78lab.com