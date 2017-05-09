---
title: java基础-1-java安装及环境变量配置
date: 2017-04-27 19:26:20
tags: java
categories: 小白入门篇-java
---
Java具有简单性、面向对象、分布式、健壮性、安全性、平台独立与可移植性、多线程、动态性等特点。
<!-- more -->
	由于没有修改/etc/profile的权限，故此处采用单独用户配置
	1.打开terminal或者iterm，输入java -version查看系统是否已经安装过jdk
	2.下载jdk mac版
	（http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html）
	3.安装完之后，cd /Library/Java/JavaVirtualMachines/  查看jdk安装情况
	4.vim ~/.bash_profile 输入以下代码
		AVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_92.jdk/Contents/Home
		PATH=$JAVA_HOME/bin:$PATH
		export JAVA_HOME
		export PATH
	5.输入 source ~/.bash_profile 使配置文件生效
	6.输入 java -version查看安装是否成功
	如果觉得我的文章对您有用，请随意打赏。您的支持将鼓励我继续创作！
