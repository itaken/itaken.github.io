---
title: "添加了.gitignore忽略规则,但是不生效"
date: 2020-11-18 10:52:48 +0800

categories: 一个问题
tags: [git, 问题集]
---

<mark>问题描述</mark>

将需要忽略的文件添加到.gitignore中,但是发现文件并没有被忽略.

<mark>解决方案</mark>

git只是会忽略未纳入版本管理的文件或文件夹, 所以需要使用`rm`将需要忽略的文件或文件夹从版本管理中移除.

```bash
git rm -r --cached .
git add .
git commit -m "update .gitignore"
```

---
## 参考资料
- [.gitignore不生效问题](https://blog.csdn.net/qinyushuang/article/details/55210286)
- [.gitignore 不生效的解决方案](https://blog.csdn.net/zwkkkk1/article/details/83550032)
