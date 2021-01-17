---
title: "hexo blog部署到github"
date: 2020-11-18 21:41:17 +0800
img: /assets/images/202011/18-01.png

categories: 开发日常
tags: [github, hexo]
---

## 有自己的`hexo`blog

## 安装`hexo-deployer-git`插件

```bash
npm install hexo-deployer-git --save
```
国内环境可以使用`cnpm`安装

## 修改`_config.yml`文件,添加部署内容
```bash
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: 'git'
  repo: https://github.com/itaken/itaken.github.io.git
  branch: pages
```

![deploy配置](/assets/images/202011/18-01.png)

也可以配置deploy账号：

```
deploy:
  name: itaken
  email: regelhh@gmail.com
```

## 本地git与github账号绑定

在博客项目根目录下,修改用户配置信息为你github账号
```bash
git config user.name "itaken"
git config user.email "regelhh@gmail.com"
```

生成ssh密钥
```bash
ssh-keygen -t rsa -C "regelhh@gmail.com"
ssh -T git@github.com
```

## 部署到 github

```bash
(base) itaken@itaken-PC:~/path/to/itaken.github.io$ hexo d
INFO  Validating config
INFO  Deploying: git
INFO  Setting up Git deployment...
已初始化空的 Git 仓库于 /path/to/itaken.github.io/.deploy_git/.git/
[master（根提交） a60585c] First commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 placeholder
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
INFO  Copying files from extend dirs...
[master 84d0df4] Site updated: 2020-11-18 20:38:40
 562 files changed, 420300 insertions(+)
 create mode 100644 "201702/2017-02-14-\345\234\250git\344\270\212\345\273\272\347\253\213\350\207\252\345\267\261\347\232\204blog/index.html"
 ...
 create mode 100644 "202011/2020-11-18-gitignore\344\270\215\347\224\237\346\225\210/index.html"
 ...
 create mode 100644 404/index.html
 ...
 create mode 100644 "tags/\351\235\242\350\257\225\351\242\230/index.html"
对象计数中: 1015, 完成.
Delta compression using up to 8 threads.
压缩对象中: 100% (654/654), 完成.
写入对象中: 100% (1015/1015), 11.20 MiB | 3.15 MiB/s, 完成.
Total 1015 (delta 348), reused 0 (delta 0)
remote: Resolving deltas: 100% (348/348), done.
To https://github.com/itaken/itaken.github.io.git
 + 4364550...84d0df4 HEAD -> master (forced update)
分支 master 设置为跟踪来自 https://github.com/itaken/itaken.github.io.git 的远程分支 master。
INFO  Deploy done: git
```

再次部署,最好先执行`hexo clean`,清除缓存
```bash
hexo clean
hexo g -d
```

## 修改github项目的配置
[settings](https://github.com/itaken/itaken.github.io/settings) > GitHub Pages
修改 source 为配置的deploy的分支(一般不建议使用master分支)

![deploy](/assets/images/202011/18-02.png)

## 打开`https://用户名.github.io`查看自己的博客吧

---
## 参考文档
- [Hexo+GithubPages 非常简单易行的搭建个人博客](https://www.jianshu.com/p/860d3e0fff58)
- [超详细Hexo+Github Page搭建技术博客教程](https://segmentfault.com/a/1190000017986794)
