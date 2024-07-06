+++
title = 'visualvm'
date = '2024-07-04'
author = 'aiclr'
categories = ['develop']
tags = ['jvm','java','visualvm']
Summary='VisualVM is a visual tool integrating commandline JDK tools and lightweight profiling capabilities.\n Designed for both development and production time use.'
+++

`jdk1.8.0_191/bin/jvisualvm.exe` \
当配置Path java环境变量可以直接输入jvisualvm启动。\
插件：工具-插件-Virtual GC

java8之后 jdk不再附带 VisualVM \
[下载页面](http://visualvm.github.io) \
[github](https://github.com/oracle/visualvm)

## 运行参数配置文件 

`visualvm/etc/visualvm.conf`\
例如配置`jdkhome`:`visualvm_jdkhome="C:\Program Files\Java\jdk-17.0.2"`

## 运行文件

`visualvm/bin/visualvm`
