---
layout: post
title: ubuntu nfs setup
published: false
categories: ['rails']
tags: ['rails']
---
#server setup

	nano /etc/exports
	/home           12.33.44.555(rw,sync,no_root_squash,no_subtree_check)
	/var/nfs        12.33.44.555(rw,sync,no_subtree_check)

	exportfs -a

# client setup

	apt-get install nfs-common portmap

	mount -o soft,intr 10.14.25.134:/mnt/a /mnt/nfs
	mount -o soft,intr
	vi /etc/fstab

	10.14.25.134:/mnt/a  /mnt/nfs   nfs      soft,auto,noatime,nolock,bg,intr,tcp,actimeo=1800 0 0


#강제 umount

ex) fuser -ck /mnt/nfs/home
위 명령으로 프로세스를 kill 하고 umount를 재시도하면 정상적으로 mount가 해제 됩니다.
 
* 해당 디렉토리를 사용하는 사용자가 누구인지 확인하고 싶을 때
# fuser -cu /mnt/nfs/home


#auto mount


