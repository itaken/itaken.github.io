---
title: "Auto packing the repository for optimum performance."
date: 2017-10-20 09:06:39 +0800
img: /assets/images/201710/20-01.png

categories: 一个问题
tags: [git, 问题集]
---

<mark>问题描述</mark>

`git pull` 拉取更新内容, 提示: `Auto packing the repository for optimum performance.`

![pull error](/assets/images/201710/20-01.png)

使用 `git fsck` , 但是没有效果.

![git fsck](/assets/images/201710/20-02.png)

使用`管理员`运行也没有效果.

![管理员命令](/assets/images/201710/20-03.png)

使用 `git gc`, 也是没有效果.

![you-get error](/assets/images/201710/20-04.png)

<mark>解决方法</mark>

直接使用 `git config gc.auto 0` 即可.

```bash
$ git config gc.auto 0
```

---
## 参考文档
- [Unlink of file Failed. Should I try again?](https://stackoverflow.com/questions/4389833/unlink-of-file-failed-should-i-try-again)
- [Why does git keep telling me it's “Auto packing the repository in background for optimum performance”?](https://stackoverflow.com/questions/28633956/why-does-git-keep-telling-me-its-auto-packing-the-repository-in-background-for)
- [What does “Auto packing the repository for optimum performance” mean?](https://stackoverflow.com/questions/8633981/what-does-auto-packing-the-repository-for-optimum-performance-mean)
