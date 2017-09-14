---
layout: post

title: "hexo 创建 about page"
date: 2017-09-09 10:53:27 +0800

categories: post
tags: [hexo, nodejs, blog]
---

## 方法一

直接在 项目的 `source` 目录下**新建**`about`文件夹, 然后在**about文件夹**中新建`index.md`,

即可在 通过`/about/` 访问.


## 方法二

进入项目, 使用 `hexo new page "about"`命令创建即可.

```bash
$ hexo new page "about"
INFO  Created: /path/to/hexo_blog/source/about/index.md
```

---
### 更多阅读
- [Writing](https://hexo.io/docs/writing.html)
- [创建 "关于我" 页面](https://github.com/iissnan/hexo-theme-next/wiki/%E5%88%9B%E5%BB%BA-%22%E5%85%B3%E4%BA%8E%E6%88%91%22-%E9%A1%B5%E9%9D%A2)
