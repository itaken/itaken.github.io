---
title: "jekyll server 无法启动"
date: 2017-07-07 11:06:08 +0800

categories: 开发日常
tags: [ubuntu, jekyll, 问题集]
---

<mark>问题描述</mark>

>今天下载了别人家的**jekyll blog**, 执行 `jekyll server` 之后, 发现 [**http://127.0.0.1:4000**](http://127.0.0.1:4000/) 一直被占用. 重启之后,都无法关闭该blog,运行自己的 jekyll blog 结果**报错**.
```bash
$ jekyll server
WARN: Unresolved specs during Gem::Specification.reset:
      rb-fsevent (>= 0.9.4, ~> 0.9)
      rb-inotify (>= 0.9.7, ~> 0.9)
WARN: Clearing out unresolved specs.
Please report a bug if this causes problems.
```

<mark>解决方法</mark>

使用`gem cleanup`清理.

```bash
$ sudo gem cleanup rb-inotify

$ sudo bundle update

$ jekyll server
```

---
## 参考文档
- [Unresolved specs during Gem::Specification.reset:](https://stackoverflow.com/questions/17936340/unresolved-specs-during-gemspecification-reset)
- [jekyll server on but not regenerating](https://stackoverflow.com/questions/23774304/jekyll-server-on-but-not-regenerating)
- [Unresolved specs during Gem::Specification.reset: minitest (~> 5.1) \#1267](https://github.com/rubygems/rubygems/issues/1267)
