---
layout: post

title: "使用jekyll来管理github pages"
date: 2017-02-16 10:20:01

categories: post
tags: github jekyll gem
---

```
 系统: Ubuntu 16.10
 git: 2.9.3
```
1. 讲 github pages clone 到本地
{% highlight linux linenos %}git clone https://github.com/itaken/itaken.github.io.git
{% endhighlight %}

2. 进入你本地的github pages目录

{% highlight linux linenos %}$ jekyll new .
{% endhighlight %}

```
  Conflict: /var/www/git/mine/itaken.github.io exists and is not empty.
```

使用下面语句

{% highlight linux linenos %}$ jekyll new . --force
{% endhighlight %}

3. 开启服务

{% highlight linux linenos %}$ jekyll serve
{% endhighlight %}

FR: [https://jekyllrb.com/docs/usage/](https://jekyllrb.com/docs/usage/)

打开: http://127.0.0.1:4000/

4. 开始书写或修改博客

5. 提交到 git

{% highlight linux linenos %}$ git add .
$ git commit -m "提交信息"
$ git push origin master
{% endhighlight %}
