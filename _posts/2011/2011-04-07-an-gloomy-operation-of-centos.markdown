---
layout: post
title: 一次郁闷的CentOS操作
---
今天维护CentOS，误操作，把YUM和RPM给删了。只能自己手工从源码安装，可结果，想跳楼～～～～～

1. 安装yum
wget http://yum.baseurl.org/download/3.2/yum-3.2.28.tar.gz
tar xvf yum-3.2.28.tar.gz

cd yum-3.2.28
yummain.py install yum
出错：
	依赖rpm
2. 安装rpm
	1. 下载: http://rpm.org/releases/rpm-4.8.x/rpm-4.8.1.tar.bz2
	2. 安装rpm
> bash$ mkdir my_temp_build

> bash$ cd my_temp_build

> bash$ tar zxvf rpm*.tar.gz

> bash$ cd rpm-4.0.2

> bash$ export LIBS='-L/usr/local/BerkeleyDB.3.1/lib'

> bash$ export CPPFLAGS='-I/usr/local/BerkeleyDB.3.1/include'

> bash$ ./configure

> bash$ make

出错:

> checking for a BSD-compatible install... /usr/bin/install -c

> checking whether build environment is sane... yes

> checking for a thread-safe mkdir -p... /bin/mkdir -p

> checking for gawk... gawk

> checking whether make sets $(MAKE)... yes

> checking how to create a ustar tar archive... gnutar

>checking for style of include used by make... GNU

> checking for gcc... no

> checking for cc... no

> checking for cl.exe... no

> configure: error: in `/opt/softwares/centos/rpm-4.8.1':

> configure: error: no acceptable C compiler found in $PATH

> See `config.log' for more details.

需要重新安装gcc


3. 安装gcc

	- 依赖于c compiler

	无语了

更新： http://www.cnblogs.com/hakuci/archive/2008/12/07/1349592.html