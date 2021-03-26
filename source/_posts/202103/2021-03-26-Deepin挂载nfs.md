---
title: "Deepin如何挂载nfs进行文件夹共享"
date: 2021-03-26 22:28:09 +0800

categories: 开发日常
tags: [linux]
---

1. 安装`nfs-common`,当然安装`nfs-kernel-server`(服务端)也可以.

```
itaken@itaken-home:~$ sudo apt install nfs-kernel-server
正在读取软件包列表... 完成
正在分析软件包的依赖关系树
正在读取状态信息... 完成
将会同时安装下列软件：
  keyutils libnfsidmap2 nfs-common rpcbind
建议安装：
  open-iscsi watchdog
下列【新】软件包将被安装：
  keyutils libnfsidmap2 nfs-common nfs-kernel-server rpcbind
升级了 0 个软件包，新安装了 5 个软件包，要卸载 0 个软件包，有 0 个软件包未被升级。
需要下载 479 kB 的归档。
解压缩后会消耗 1,503 kB 的额外空间。
您希望继续执行吗？ [Y/n] y
...
获取:5 https://community-packages.deepin.com/deepin apricot/main amd64 nfs-kernel-server amd64 1:1.3.4-2.5+deb10u1+rebuild [122 kB]
已下载 479 kB，耗时 11秒 (41.7 kB/s)
正在选中未选择的软件包 libnfsidmap2:amd64。
(正在读取数据库 ... 系统当前共安装有 469497 个文件和目录。)
准备解压 .../libnfsidmap2_0.25-5.1+apricot_amd64.deb  ...
...
正在设置 keyutils (1.6-6) ...
正在设置 libnfsidmap2:amd64 (0.25-5.1+apricot) ...
正在设置 nfs-common (1:1.3.4-2.5+deb10u1+rebuild) ...

Creating config file /etc/idmapd.conf with new version
正在添加系统用户"statd" (UID 127)...
正在将新用户"statd" (UID 127)添加到组"nogroup"...
无法创建主目录"/var/lib/nfs"。
Created symlink /etc/systemd/system/multi-user.target.wants/nfs-client.target → /lib/systemd/system/nfs-client.target.
Created symlink /etc/systemd/system/remote-fs.target.wants/nfs-client.target → /lib/systemd/system/nfs-client.target.
nfs-utils.service is a disabled or a static unit, not starting it.
正在设置 nfs-kernel-server (1:1.3.4-2.5+deb10u1+rebuild) ...
Created symlink /etc/systemd/system/multi-user.target.wants/nfs-server.service → /lib/systemd/system/nfs-server.service.
Job for nfs-server.service canceled.

Creating config file /etc/exports with new version

Creating config file /etc/default/nfs-kernel-server with new version
正在处理用于 systemd (241.8.1-6+dde) 的触发器 ...
正在处理用于 man-db (2.8.5-2) 的触发器 ...
正在处理用于 libc-bin (2.28.11-1+eagle) 的触发器 ...
Scanning processes...
Scanning processor microcode...
Scanning linux images...

Running kernel seems to be up-to-date.
...
```

2. 挂载到一个存在的目录, 需要`sudo`

```
itaken@itaken-home:~$ sudo mount -t nfs 192.168.1.13:/volume1/公共的 /path/to/公共的
```

>错误示范:
```
itaken@itaken-home:~$ mount 192.168.1.13:/volume1 /mnt/nfs
mount: only root can do that

itaken@itaken-home:~$ sudo mount 192.168.1.13:/volume1 /mnt/nfs
mount.nfs: mount point /mnt/nfs does not exist

itaken@itaken-home:~$ sudo mount 192.168.1.13:/volume1 /path/to/公共的
mount.nfs: access denied by server while mounting 192.168.1.13:/volume1

itaken@itaken-home:~$ sudo mount -t 192.168.1.13:/volume1 /path/to/公共的
mount: /path/to/公共的: can't find in /etc/fstab.

itaken@itaken-home:~$ sudo mount -t nfs 192.168.1.13:/volume1 /path/to/公共的
mount.nfs: access denied by server while mounting 192.168.1.13:/volume1
```

---
## 参考文档
- [ubuntu通过NFS挂载文件](https://blog.csdn.net/qq_40511918/article/details/105651998)
