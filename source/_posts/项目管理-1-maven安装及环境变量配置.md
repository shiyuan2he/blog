---
title: 项目管理-1-maven安装及环境变量配置
date: 2017-04-27 19:01:17
tags: maven,项目管理,mac
categories: 项目管理利器-maven
---
	越来越多的公司启用maven管理公司商业项目，所以maven的学习势在必行。
<!-- more -->
Maven项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具.
Maven 除了以程序构建能力为特色之外，还提供高级项目管理工具。由于 Maven 的缺省构建规则有较高的
可重用性，所以常常用两三行 Maven 构建脚本就可以构建简单的项目。由于 Maven 的面向项目的方法，
许多 Apache Jakarta 项目发文时使用 Maven，而且公司项目采用 Maven 的比例在持续增长。
	本篇介绍macbookpro下安装及配置maven环境变量。
	1.下载maven（https://maven.apache.org/download.cgi），并解压到某一个目录，
如:/Users/heshiyuan/data/devTools/maven/apache-maven-3.3.9
	2.打开iterm或者terminal，输入以下命令：
		vim ~/.bash_profile
	3.添加以下几行代码，之后保存并推出:wq
		MAVEN_HOME=/Users/heshiyuan/data/devTools/maven/apache-maven-3.3.9
		PATH=$MAVEN_HOME/bin:$PATH
		export MAVEN_HOME
		export PATH
	4.输入以下命令使.bash_profile生效
		source ~/.bash_profile
	5.输入mvn -version查看是否成功
	6.如果未安装成功，检查是否先设置了java环境变量
		JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_92.jdk/Contents/Home
		PATH=$JAVA_HOME/bin:$PATH
		export JAVA_HOME
		export PATH
	如果觉得我的文章对您有用，请随意打赏。您的支持将鼓励我继续创作！
