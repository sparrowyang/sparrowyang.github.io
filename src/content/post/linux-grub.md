---
title: "Deepin 修改引导界面"
date: 2020-06-15
tags : [ "grub"]
categories : ["linux"]
author: "sparrowyang"
draft: false 
---

> 自从装了deepin 15，也没多少时间用，大多是时间还是用windows，但是有些东西还是要在deepin 上跑。
>
> 问题是，每次开机都要和引导拼手速，手动选择进入windows，否则就自动进入deepin了。于是通过修改grub配置，使默认进入winows。

## 启动列表

在启动项选择中，一共有四个启动项：

1. deepin 15
2. deepin 高级设置
3. windows 10
4. Systemsetup

默认是选中第一个的，开机时不按上下键选择的话会默认进入deepin。

## 修改启动项

配置文件在`/etc/default/grub`

默认情况下的配置是这样的

```bash
sparrow@sparrow-PC:~$ cat /etc/default/grub
# Written by com.deepin.daemon.Grub2
DEEPIN_GFXMODE_ADJUSTED=1
GRUB_CMDLINE_LINUX=""
GRUB_CMDLINE_LINUX_DEFAULT="splash quiet"
GRUB_DEFAULT=0
GRUB_DISTRIBUTOR="`/usr/bin/lsb_release -d -s 2>/dev/null || echo Deepin`"
GRUB_GFXMODE=1920x1080
GRUB_THEME="/boot/grub/themes/deepin/theme.txt"
GRUB_TIMEOUT=5

```

`GRUB_DEFAULT=0`默认第1个选项，因为windows 10 是第3个启动项，所以修改为2。

修改完保存是还没有效果的，因为没更新，执行

```bash
sudo update-grub
```

更新一下，即可。

## 一些坑

当初装deepin时可能为了解决开机logo 卡死问题，会在`/etc/default/grub`文件添加`acpi_osi=! acpi="windwos 2009"`。我执行完

```bash
sudo update-grub
```

后会又出现logo卡死问题。因为添加的东西会被更新后没了。

解决办法是修改`GRUB_CMDLINE_LINUX_DEFAULT="splash quiet acpi_osi=! acpi=\"windwos 2009\""`，改成这样：

```bash
sparrow@sparrow-PC:~$ cat /etc/default/grub
# Written by com.deepin.daemon.Grub2
DEEPIN_GFXMODE_ADJUSTED=1
GRUB_CMDLINE_LINUX=""
GRUB_CMDLINE_LINUX_DEFAULT="splash quiet acpi_osi=! acpi=\"windwos 2009\""
GRUB_DEFAULT=2
GRUB_DISTRIBUTOR="`/usr/bin/lsb_release -d -s 2>/dev/null || echo Deepin`"
GRUB_GFXMODE=1920x1080
GRUB_THEME="/boot/grub/themes/deepin/theme.txt"
GRUB_TIMEOUT=5

```

然后再

```bash
sudo update-grub
```

就没问题了


