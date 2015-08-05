---
layout: post
title: remove file git repo
published: True
categories: ['rails']
tags: ['rails']
---
#git 저장소에서 파일을 히스토리까지 완전히 삭제하는법
	git filter-branch --force --index-filter \
	'git rm --cached --ignore-unmatch .env' \
	--prune-empty --tag-name-filter cat -- --all

	git push origin --force --all