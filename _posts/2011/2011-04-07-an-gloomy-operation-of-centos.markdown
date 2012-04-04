---
layout: post
title: 一次郁闷的CentOS操作
---

今天维护CentOS，误操作，把YUM和RPM给删了。只能自己手工从源码安装，可结果，想跳楼～～～～～

1. 安装yum 
wget http://yum.baseurl.org/download/3.2/yum-3.2.28.tar.gz 
tar xvf yum-3.2.28.tar.gz
cd yum-3.2.28 
yum main.py install yum 
出错： 依赖rpm </br>
2. 安装rpm 
	1. 下载: http://rpm.org/releases/rpm-4.8.x/rpm-4.8.1.tar.bz2 
	2. 安装rpm

		bash$ mkdir my_temp_build</br>
		bash$ cd my_temp_build</br>
		bash$ tar zxvf rpm*.tar.gz</br>
		bash$ cd rpm-4.0.2</br>
		bash$ export LIBS='-L/usr/local/BerkeleyDB.3.1/lib'</br>
		bash$ export CPPFLAGS='-I/usr/local/BerkeleyDB.3.1/include'</br>
		bash$ ./configure</br>
		bash$ make</br>

		出错:			
		checking for a BSD-compatible install... /usr/bin/install -c</br>
		checking whether build environment is sane... yes</br>
		checking for a thread-safe mkdir -p... /bin/mkdir -p</br>
		checking for gawk... gawk</br>
		checking whether make sets $(MAKE)... yes</br>
		checking how to create a ustar tar archive... gnutar</br>
		checking for style of include used by make... GNU</br>
		checking for gcc... no</br>
		checking for cc... no</br>
		checking for cl.exe... no</br>
		configure: error: in '/opt/softwares/centos/rpm-4.8.1':</br>
		configure: error: no acceptable C compiler found in $PATH</br>
		See 'config.log' for more details.</br>
		需要重新安装gcc		
	
3. 安装gcc

	依赖于c compiler

	无语了

更新： http://www.cnblogs.com/hakuci/archive/2008/12/07/1349592.html