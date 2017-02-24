---
layout: post

title: "使用jekyll来创建自己的blog"
date: 2017-02-15 10:47:38 +0800

categories: post
tags: jekyll gem
---

```
  Ubuntu: 16.10
  ruby: 2.3.1p112
  gem: 2.6.7
  jekyll: 3.4.0
```

\1. 安装 gem, [https://rubygems.org/](https://rubygems.org/)

\2. 安装 ffi
```shell
sudo gem install ffi
```

如果报错:

```
Building native extensions.  This could take a while...
ERROR:  Error installing ffi:
	ERROR: Failed to build gem native extension.

    current directory: /var/lib/gems/2.3.0/gems/ffi-1.9.17/ext/ffi_c
/usr/bin/ruby2.3 -r ./siteconf20170214-24074-12fbih4.rb extconf.rb
mkmf.rb can't find header files for ruby at /usr/lib/ruby/include/ruby.h

extconf failed, exit code 1

Gem files will remain installed in /var/lib/gems/2.3.0/gems/ffi-1.9.17 for inspection.
Results logged to /var/lib/gems/2.3.0/extensions/x86_64-linux/2.3.0/ffi-1.9.17/gem_make.out
```

解决方案如下: [^1]

```shell
sudo apt-get install ruby-dev
sudo apt-get install make  # 如果还不成功, 执行如下语句
```

\3. 安装 minima
```shell
sudo gem install minima
```

\4. 使用 gem 安装 jekyll
```shell
sudo gem install jekyll bundler
```

\5. 创建blog
```shell
jekyll new myblog
```

\6. 开始博客之旅
```shell
cd myblog
bundle exec jekyll serve
```

打开: http://127.0.0.1:4000/

![jekyll blog]({{ site.url }}/assets/images/201702/15-01.png)

[^1]: http://stackoverflow.com/questions/13767725/unable-to-install-gem-failed-to-build-gem-native-extension-cannot-load-such
