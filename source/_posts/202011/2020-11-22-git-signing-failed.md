---
title: "sign_and_send_pubkey: signing failed: agent refused operation"
date: 2020-11-22 22:45:43 +0800

categories: 一个问题
tags: [git, ssh, 问题集]
---
<mark>问题描述</mark>
换了电脑,导入的原来系统的ssh整个文件夹,拉取项目提示权限问题...
```bash
itaken@itaken-PC:~$ git clone ssh://git@git-company.local/my-project.git
正克隆到 'my-project'...
sign_and_send_pubkey: signing failed: agent refused operation
git@git-company.local: Permission denied (publickey).
fatal: 无法读取远程仓库。
请确认您有正确的访问权限并且仓库存在。
```

<mark>解决方法</mark>
## 修改`ssh`目录权限
```bash
itaken@itaken-PC:~$ chmod 700 .ssh/
itaken@itaken-PC:~$ cd .ssh/
itaken@itaken-PC:~/.ssh$ cd ..
itaken@itaken-PC:~$ chmod 600 .ssh/*
itaken@itaken-PC:~$ la | grep ssh
drwx------  2 itaken itaken  4096 11月 22 21:58 .ssh
itaken@itaken-PC:~$ cd .ssh/
itaken@itaken-PC:~/.ssh$ ll
总用量 24
-rw------- 1 itaken itaken 1766 4月  18  2020 itaken
-rw------- 1 itaken itaken  405 4月  18  2020 itaken.pub
-rw------- 1 itaken itaken  372 5月  15  2020 config
-rw------- 1 itaken itaken 3540 8月  31 03:18 known_hosts
```

如果不修改文件权限,会提示权限太开放了.
```bash
itaken@itaken-PC:~$ ssh-add ~/.ssh/itaken
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/home/itaken/.ssh/itaken' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
```

## `ssh-add`添加私钥
```bash
itaken@itaken-PC:~$ ssh-add ~/.ssh/itaken
Enter passphrase for /home/itaken/.ssh/itaken:
Bad passphrase, try again for /home/itaken/.ssh/itaken:
Identity added: /home/itaken/.ssh/itaken (/home/itaken/.ssh/itaken)
```

---
## 参考文档
- [How to solve “sign_and_send_pubkey: signing failed: agent refused operation”?](https://stackoverflow.com/questions/44250002/how-to-solve-sign-and-send-pubkey-signing-failed-agent-refused-operation)
