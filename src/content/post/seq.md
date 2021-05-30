---
title: "Seq 命令使用"
date: "2020-05-11"
tags : [ "shell"]
categories : ["linux"]
author: "sparrowyang"
draft: false
---


前阵子突然刷到这个命令`seq`,可以用来生成一堆有序的数。
生成速度还挺快的
## 查看帮助
`seq --help`
```shell
┌─[yzf@sparrow] - [/mnt/c/Users/sparrow] - [2020-05-11 12:47:03]
└─[1] seq --help
Usage: seq [OPTION]... LAST
  or:  seq [OPTION]... FIRST LAST
  or:  seq [OPTION]... FIRST INCREMENT LAST
Print numbers from FIRST to LAST, in steps of INCREMENT.

Mandatory arguments to long options are mandatory for short options too.
  -f, --format=FORMAT      use printf style floating-point FORMAT
  -s, --separator=STRING   use STRING to separate numbers (default: \n)
  -w, --equal-width        equalize width by padding with leading zeroes
      --help     display this help and exit
      --version  output version information and exit
...
```

## 基本使用
`seq 20`生成1~20的数，每个一行
```bash
┌─[yzf@sparrow] - [/mnt/c/Users/sparrow] - [2020-05-11 01:00:56]
└─[0] seq 20
1
2
3
...
18
19
20
```
`seq 10 20`生成10~20的数，每个一行
```bash
┌─[yzf@sparrow] - [/mnt/c/Users/sparrow] - [2020-05-11 01:01:13]
└─[130] seq 10 20
10
11
...
18
19
20
```
`seq 10 2 20`生成10~20的数，步长为2，每个一行
```bash
┌─[yzf@sparrow] - [/mnt/c/Users/sparrow] - [2020-05-11 01:02:36]
└─[0] seq 10 2 20
10
12
14
16
18
20
```
## -s 指定分隔符
```bash
┌─[yzf@sparrow] - [/mnt/c/Users/sparrow] - [2020-05-11 01:05:32]
└─[0] seq -s + 1 10
1+2+3+4+5+6+7+8+9+10
```
有意思的是我在termux上直接`-s*`就能指定`*`号为分隔符。但是在wsl上需要加`\`转义
```bash
┌─[yzf@sparrow] - [/mnt/c/Users/sparrow] - [2020-05-11 01:05:21]
└─[1] seq -s\* 1 10
1*2*3*4*5*6*7*8*9*10
```
## -f 指定输出格式
```bash
┌─[yzf@sparrow] - [/mnt/c/Users/sparrow] - [2020-05-11 01:10:40]
└─[0] seq -f "int value_%-1g =1;" 1 5
int value_1 =1;
int value_2 =1;
int value_3 =1;
int value_4 =1;
int value_5 =1;
```
%g为默认，%kg控制数字前后的格式

## -w控制位数等宽
```bash
┌─[yzf@sparrow] - [/mnt/c/Users/sparrow] - [2020-05-11 01:13:36]
└─[130] seq -w 98 101
098
099
100
101
```