---
title:      "Linux /etc/fstab文件出错修改"
subtitle:   "Linux系统问题"
date:       2018-04-27
author:     "Xiaotong CHEN"
header-img: 
catalog:
tags:
    - Linux
---

* 目录
{:toc}

# Linux /etc/fstab文件相关问题

## 问题起因
PC环境是windows+linux（ubuntu 16.04.4 LTS）
在一次运行linux死机，用电源按键强制关机后，重启linux进入emergency mode，错误删除了/etc/fstab文件中的一行。再次重启后提示`ctrl+D`，无效，查看系统日志发现是有一个分区挂载出错。

## 解决方案
解决的核心是重新编写/etc/fstab文件
*需要注意的是：在linux卡住的虚拟终端tty2-6，尝试打开并修改etc/fstab，即使用sudo获取管理员权限也不可以。必须进入BIOS后运行linux resuce mode，root之后在此模式下，先去除fstab文件只读属性，才可以修改/etc/fstab文件。*
1. 进BIOS，输入密码进入boot权限下界面
2. `mount -n -o remout,rw /`
3. 修改文件`vi /etc/fstab`
4. `：wq` 保存 `reboot` 重启 ，OK了

## 如何修改/etc/fstab文件
- 了解/etc/fstab文件
/etc/fstab   包含了你的磁盘分区以及存储设备如何挂载，以及挂载在什么地方的信息

第一列包含着设备名，
第二列是它的挂载点，
第三列是它的文件系统格式，
第四是挂载参数，
第五列［一个数字］是转储选项
第六列［另一个数字］是文件系统检查选项。

该文件中最后两项   
1. default    这个可以写的值（rw ro    suid\[一种安全机制\]   user\[nouser\]普通用户是否可以挂载    exec能否执行二进制文件 sync\[async\] sync为实时写入硬盘，async不是实时写入，可以先写到内存，FTP中那会用到    ）
2.    0   0      前一个为0是说是否备份，1为备份     后一个是说是否检查分区错误

- 例子
我之前误删的是挂载根目录（/）的那一行，所以加上了这样一行
`UUID=XXXXXXXXXXXXXXXX  　　　　/ 　　　　ext4 　　defaults　　 1 　　1`

- 更多参考
English：[archlinux/fstab](https://wiki.archlinux.org/index.php/Fstab)
中文：[51CTO /etc/fstab文件 详解](http://blog.51cto.com/lspgyy/1297432)

## 后续
在之后有一次正常启动linux时，提示我挂载另一个分区有错，同样卡在虚拟终端，我把该分区的fsck检查与备份关闭即把最后两位数字设为0 0，再次重启可行。但是应该还有遗留的问题未解决。

## 补充
1. linux版本号查看方法：
`lsb_release -a`

2. 两种查看UUID方法：
	- `sudo blkid`
	- `ls -l /dev/disk/by-uuid`

3. linux死机安全重启方法：
按住Alt+SysRq，再依次按下reisub几个键，按完b系统就会重启。
参考：[Linux死机解决办法](https://blog.csdn.net/wl23301/article/details/17952993)

4. 其他相关可了解的知识
	- mount指令
