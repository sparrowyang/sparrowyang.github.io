---
title: "防止ssh穷举攻击"
date: "2020-01-20"
tags : ["ssh"]
categories : ["linux"]
draft: false
---

## 今天ssh上服务器突然出现
```dos
C:\Users\sparrow>ssh root@sparrow123.xyz
root@sparrow123.xyz's password:
Last failed login: Wed Jan 22 22:06:27 CST 2020 from 222.186.180.41 on ssh:notty
There were 110814 failed login attempts since the last successful login.
Last login: Mon Jan  6 18:22:38 2020 from 211.64.159.198
```
`There were 110814 failed login`11万次登录失败？什么鬼？
查一下失败的ip：
>222.186.180.41江苏省镇江市 电信

搜索发现

>你服务器 IP 在那儿，那 SSH 开在 TCP 22 上谁都能连，连上了谁都能输密码，密码错了就在系统里留下一条记录。
要么是被（无差别地）扫到了，要么是有人在盯着你。只说 SSH 登陆这事，**如果你关闭密码登陆**（或者密码足够健壮），那有个就算有一百万个猴子在试你的密码，你也完全不用担心的。

所以选择关闭密码登录(关闭密码登录后用**密钥登录**、已配置过，不提)：
```dos
[root@VM_0_13_centos ~]# cd /etc/ssh/
[root@VM_0_13_centos ssh]# vim sshd_config
```
修改
```vim
#默认PasswordAuthentication 为yes,即允许密码登录，改为no后，禁止密码登录
PasswordAuthentication no
```
重启服务，退出

```dos
[root@VM_0_13_centos ssh]# systemctl restart sshd.service
[root@VM_0_13_centos ssh]# logout
Connection to sparrow123.xyz closed.
```

再次尝试密码登录
```dos
C:\Users\sparrow>ssh root@sparrow123.xyz
root@sparrow123.xyz: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```
成功，再就不怕别人暴力登录服务器了

---

惊讶的是我再次用密钥登陆时提示：
```dos
Last failed login: Wed Jan 22 22:17:40 CST 2020 from 222.186.30.145 on ssh:notty
There were 24 failed login attempts since the last successful login.
```
`There were 24 failed login`，说明我在设置禁止密码登录的操作时，他还在暴力登录，`222.186.30.145`查一下还是江苏省镇江市 电信，这应该是脚本，真无聊~

当然，可以修改ssh端口也能解决

