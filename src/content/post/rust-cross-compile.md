---
tags : ["rust","cross compile"]
categories : ["rust"]
author: "sparrowyang"
title: "Rust交叉编译初学"
date: "2022-05-23"
draft: false 
---

今天写了个TCP监控程序，想试试能不能交叉编译到其他平台。比如我的安卓手机(arrch64/arm架构)。

按照官网给的编译教程，先安装编译的工具：
```bash
rustup target add aarch64-unknown-linux-gnu
```
然后使用命令检查编译构建`arrch64`平台应用：
```bash
cargo build --release --target=aarch64-unknown-linux-gnu
```
果不其然，报错了。
```
...
error: linking with `cc` failed: exit status: 1
...
```
查阅[资料](https://users.rust-lang.org/t/unable-to-compile-to-aarch64-unknown-linux-gnu/53517)发现需要在`～/.cargo/config`中加入
```
[target.aarch64-unknown-linux-gnu]
linker = "aarch64-linux-gnu-gcc"
```
此外还得安装`aarch64-linux-gnu-gcc`，我使用`pacman`很轻松的安装上了。之后成功编译。我迫不及待的将程序发送到我的手机上，在`termux`中运行。然而运行不了。提示“No such file or directory”，当然程序是存在的且有运行权限。经验告诉我，这应该是程序平台架构不符。我使用`file`命令查看文件，得到的信息：
```
ttccpp: ELF 64-bit LSB pie executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, BuildID[sha1]=a21fa7de1aec8979940695e98523aff8637d5541, for GNU/Linux 3.7.0, with debug_info, not stripped
```
但的确显示的是arrch64,64bit。于是我把代码发到`termux`中构建，得到能运行的程序文件。同样使用`file`命令查看:
```
tccpp2: ELF 64-bit LSB pie executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /system/bin/linker64, not stripped
```
发现两者的不同点在`interpreter`,于是我发现手机中存在`/system/bin/linker64`但不存在`/lib/ld-linux-aarch64.so.1`，这是不能运行的原因。

最后尝试了其他选择的交叉编译，都报错了。知道问题所在，但不知道问题的解决方法。



