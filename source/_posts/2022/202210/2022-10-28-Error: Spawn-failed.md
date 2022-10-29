---
title: "hexo 部署错误：err: Error: Spawn failed"
date: 2022-10-28 23:08:02 +0800

categories: 一个问题
tags: [问题集, hexo]
---

<mark>问题描述</mark>

执行`hexo deploy`的时候，提示`error: bad index file sha1 signature`错误。

```
itaken@itaken:/path/to/itaken.github.io$ hexo deploy
INFO  Validating config
INFO  Deploying: git
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
INFO  Copying files from extend dirs...
error: bad index file sha1 signature
fatal: index file corrupt
FATAL {
  err: Error: Spawn failed
      at ChildProcess.<anonymous> (/path/to/itaken.github.io/node_modules/hexo-deployer-git/node_modules/hexo-util/lib/spawn.js:51:21)
      at ChildProcess.emit (node:events:513:28)
      at Process.ChildProcess._handle.onexit (node:internal/child_process:291:12) {
    code: 128
  }
} Something's wrong. Maybe you can find the solution here: %s https://hexo.io/docs/troubleshooting.html
```

检查发现**master**分支没有问题，查看`_config.yml`的**deploy**配置，发现是执行部署的分支是**gh-pages**， 切换到该分支，提示`error: inflate: data stream error (incorrect data check)`异常。

```
itaken@itaken:/path/to/itaken.github.io$ git ck gh-pages
error: inflate: data stream error (incorrect data check)
error: inflate: data stream error (incorrect data check)
error: corrupt loose object '99855bc1ddce1ca07f08e2e3d62b2e7cb0520756'
fatal: loose object 99855bc1ddce1ca07f08e2e3d62b2e7cb0520756 (stored in .git/objects/99/855bc1ddce1ca07f08e2e3d62b2e7cb0520756) is corrupt
```

### 方法一: 删除`.git/index`，再`reset`，发现**无效**

```
itaken@itaken:/path/to/itaken.github.io$ rm -rf .git/index 
itaken@itaken:/path/to/itaken.github.io$ git reset

itaken@itaken:/path/to/itaken.github.io$ git st
位于分支 master
您的分支与上游分支 'origin/master' 一致。
尚未暂存以备提交的变更：
  （使用 "git add/rm <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	删除：     .gitignore
	删除：     LICENSE
	删除：     README.md
	删除：     _config.yml
	删除：     package.json
    ...
	删除：     themes/hexo-theme-matery/source/medias/reward/wechat.png

未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）

	.deploy_git/
	.sass-cache/
    ...
	source/_drafts/

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
itaken@itaken:/path/to/itaken.github.io$ git ck -- .
error: inflate: data stream error (incorrect data check)
error: inflate: data stream error (incorrect data check)
fatal: loose object 1ce3c9eb9e2a8b6ed8663605d2b04d6d5788ba77 (stored in .git/objects/1c/e3c9eb9e2a8b6ed8663605d2b04d6d5788ba77) is corrupt
```
### 方法二：删除`.git/objects/`下对应文件，执行`git fsck --full`，发现也**无效**

```
itaken@itaken:/path/to/itaken.github.io$ rm .git/objects/1c/e3c9eb9e2a8b6ed8663605d2b04d6d5788ba77
itaken@itaken:/path/to/itaken.github.io$ git fsck --full
error: inflate: data stream error (incorrect data check)
error: corrupt loose object '1122d617fe1fb930935c0c5157d36f8a42aa7aff'
error: unable to unpack contents of .git/objects/11/22d617fe1fb930935c0c5157d36f8a42aa7aff
error: 1122d617fe1fb930935c0c5157d36f8a42aa7aff: object corrupt or missing: .git/objects/11/22d617fe1fb930935c0c5157d36f8a42aa7aff
error: inflate: data stream error (incorrect data check)
error: corrupt loose object '13497a757ad2d004b4e42bc02e986ed92e891bf4'
error: unable to unpack contents of .git/objects/13/497a757ad2d004b4e42bc02e986ed92e891bf4
error: 13497a757ad2d004b4e42bc02e986ed92e891bf4: obj
...
missing blob 23edde7b7e7594d0a512d300d7f01bb74e2bb6db
itaken@itaken:/path/to/itaken.github.io$ git reflog
7f60ae70 HEAD@{0}: reset: moving to HEAD
7f60ae70 HEAD@{1}: reset: moving to HEAD
...

itaken@itaken:/path/to/itaken.github.io$ hexo deploy
INFO  Validating config
INFO  Deploying: git
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
INFO  Copying files from extend dirs...
error: bad index file sha1 signature
fatal: index file corrupt
FATAL {
  err: Error: Spawn failed
      at ChildProcess.<anonymous> (/path/to/itaken.github.io/node_modules/hexo-deployer-git/node_modules/hexo-util/lib/spawn.js:51:21)
      at ChildProcess.emit (node:events:513:28)
      at Process.ChildProcess._handle.onexit (node:internal/child_process:291:12) {
    code: 128
  }
} Something's wrong. Maybe you can find the solution here: %s https://hexo.io/docs/troubleshooting.html
```

>执行`hexo clean && hexo deploy`，部署还是失败

```
itaken@itaken:/path/to/itaken.github.io$ hexo clean
INFO  Validating config
INFO  Deleted database.
INFO  Deleted public folder.
itaken@itaken:/path/to/itaken.github.io$ hexo deploy
INFO  Validating config
INFO  Start processing
INFO  Files loaded in 1.17 s
INFO  Generated: about/index.html
...
error: bad index file sha1 signature
fatal: index file corrupt
FATAL {
  err: Error: Spawn failed
      at ChildProcess.<anonymous> (/path/to/itaken.github.io/node_modules/hexo-deployer-git/node_modules/hexo-util/lib/spawn.js:51:21)
      at ChildProcess.emit (node:events:513:28)
      at Process.ChildProcess._handle.onexit (node:internal/child_process:291:12) {
    code: 128
  }
} Something's wrong. Maybe you can find the solution here: %s https://hexo.io/docs/troubleshooting.html
```

### 方法三：删除`.git`文件，重新初始化

```
itaken@itaken:/path/to/itaken.github.io$ git remote -v
origin	https://github.com/itaken/itaken.github.io.git (fetch)
origin	https://github.com/itaken/itaken.github.io.git (push)
itaken@itaken:/path/to/itaken.github.io$ rm -rf .git
itaken@itaken:/path/to/itaken.github.io$ git init 
已初始化空的 Git 仓库于 /path/to/itaken.github.io/.git/
itaken@itaken:/path/to/itaken.github.io$ git remote add origin https://github.com/itaken/itaken.github.io.git
itaken@itaken:/path/to/itaken.github.io$ git fetch
remote: Enumerating objects: 7967, done.
remote: Counting objects: 100% (1668/1668), done.
remote: Compressing objects: 100% (131/131), done.
remote: Total 7967 (delta 1600), reused 1545 (delta 1534), pack-reused 6299
接收对象中: 100% (7967/7967), 15.90 MiB | 2.61 MiB/s, 完成.
处理 delta 中: 100% (3732/3732), 完成.
来自 https://github.com/itaken/itaken.github.io
 * [新分支]          gh-pages   -> origin/gh-pages
 * [新分支]          master     -> origin/master
itaken@itaken:/path/to/itaken.github.io$ git reset --hard origin/master
HEAD 现在位于 7f60ae7 新增文档
itaken@itaken:/path/to/itaken.github.io$ git branch --set-upstream-to=origin/master master
分支 master 设置为跟踪来自 origin 的远程分支 master。
itaken@itaken:/path/to/itaken.github.io$ git st
位于分支 master
```

<mark>解决方案</mark>

>删除`.deploy_git/`，重新部署即可。

```
itaken@itaken:/path/to/itaken.github.io$ rm -rf .deploy_git/
itaken@itaken:/path/to/itaken.github.io$ hexo clean && hexo g && hexo d
INFO  Validating config
INFO  Deleted database.
INFO  Deleted public folder.
...
INFO  652 files generated in 1.52 s
INFO  Validating config
INFO  Deploying: git
INFO  Setting up Git deployment...
已初始化空的 Git 仓库于 /path/to/itaken.github.io/.deploy_git/.git/
[master（根提交） 7052f1b] First commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 placeholder
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
INFO  Copying files from extend dirs...
[master ae692cf] Site updated: 2022-10-27 22:28:01
 653 files changed, 519492 insertions(+)
 create mode 100644 2000/1/1/404/index.html
 create mode 100644 2017/10/12/gitkraken-ssh配置错误/index.html
 create mode 100644 2017/10/16/多条件排序/index.html
 create mode 100644 2017/10/17/多条件排序-二/index.html
 create mode 100644 2017/10/20/git-unlink-of-file-failed/inde
 ...
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
INFO  Copying files from extend dirs...
[master 18a77a4] Site updated: 2022-10-27 22:40:53
 149 files changed, 10487 insertions(+), 10487 deletions(-)
对象计数中: 2017, 完成.
Delta compression using up to 8 threads.
压缩对象中: 100% (1216/1216), 完成.
写入对象中: 100% (2017/2017), 12.71 MiB | 15.53 MiB/s, 完成.
Total 2017 (delta 718), reused 0 (delta 0)
...
```


---
## 参考文档
- [error: inflate: data stream error (incorrect data check)](https://stackoverflow.com/questions/67158341/git-clone-error-error-inflate-data-stream-error-incorrect-data-check)
- [git 出错 bad index file sha1 signature](https://www.cnblogs.com/007sx/p/7064591.html)
- [git“fatal: loose object”错误解决办法汇总](https://blog.csdn.net/Icannotdebug/article/details/83279559)
- [git 错误 fatal: loose object...is corrupt](https://www.cnblogs.com/Windeal/p/4284608.html)
- [hexo部署报错Spawn failed原因及解决方法](https://blog.csdn.net/liegu0317/article/details/121886380)
