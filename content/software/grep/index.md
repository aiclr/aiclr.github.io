+++
title = 'grep'
date = '2024-07-06'
author = 'aiclr'
categories = ['software']
tags = ['grep','linux']
Summary='ps aux |grep java'
+++

##### 常用选项

- -E 开启扩展的正则表达式 Extend
- -i 忽略大小写ignore case
- -v 反选 invert
- -n 显示行号
- -w 匹配单词：like=like like!=liker
- -c 显示匹配了多少行
- -o 只显示匹配到的字符串
- --color=[always|never|auto] --colour=[always|never|auto] 将匹配到的内容高亮显示
- -A num 显示匹配到的字符串所在的行及其后num行 after
- -B num 显示匹配到的字符串所在的行及其前num行 before
- -C num 显示匹配到的字符串所在的行及其前后各num行 context

##### demo

```shell
grep "高亮显示内容" 202108212.log --color -n
grep "两个文件里需要高亮显示的内容" 202108212.log 202108211.log --color -n
grep "一共匹配了多少行" 202108212.log -c
# 先搜索 202112 再从结果里搜索 20211201
grep "202112" 202108212.log | grep "20211201" --color
```

## egrep

extend grep grep -E

##### demo

```shell
# | 或运算
egrep "开闸请求参数|使用线下卷，上传数据到平台" 202108212.log --color -n
grep "开闸请求参数|使用线下卷，上传数据到平台" 202108212.log -E --color -n
```

## fgrep

fast grep

##### demo

```shell
fgrep -c "快速搜索统计匹配的行数" 202108212.log
```