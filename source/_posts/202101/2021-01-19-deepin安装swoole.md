---
title: "deppin中php安装swoole拓展"
date: 2021-01-19 16:32:52 +0800
img: /assets/images/202101/19-01.png

categories: 开发日常
tags: [linux, php]
---

>开发环境
```
Deepin 20.1
PHP 7.4.14
```

# 前置工具
```
sudo apt install php7.4-dev   # phpize
sudo apt install g++    # ./configure
```

# 开始安装
## 下载`swoole`源码

```
git clone https://github.com/swoole/swoole-src.git
cd swoole-src
```

## 编译扩展 `phpize`

```
swoole-src-4.6.1$ phpize
Configuring for:
PHP Api Version:         20190902
Zend Module Api No:      20190902
Zend Extension Api No:   320190902
```

## 配置, 准备扩展的构建环境

```
 swoole-src-4.6.1$ ./configure
checking for grep that handles long lines and -e... /usr/bin/grep
checking for egrep... /usr/bin/grep -E
checking for a sed that does not truncate output... /usr/bin/sed
checking for pkg-config... /usr/bin/pkg-config
checking pkg-config is at least version 0.9.0... yes
...
checking if g++ supports -c -o file.o... (cached) yes
checking whether the g++ linker (/usr/bin/ld -m elf_x86_64) supports shared libraries... yes
checking dynamic linker characteristics... (cached) GNU/Linux ld.so
checking how to hardcode library paths into programs... immediate
configure: patching config.h.in
configure: creating ./config.status
config.status: creating config.h
config.status: executing libtool commands
```

## 执行构建,这个过程会执行在Makefile文件中定义的一系列任务
>如果`make`执行失败,可以尝试使用`make -j 4`执行构建

```
swoole-src-4.6.1$ make
/bin/bash /path/to/swoole-src-4.6.1/libtool --mode=compile g++  -I. -I/path/to/swoole-src-4.6.1 -DPHP_ATOM_INC -I/path/to/swoole-src-4.6.1/include -I/path/to/swoole-src-4.6.1/main -I/path/to/swoole-src-4.6.1 -I/usr/include/php/20190902 -I/usr/include/php/20190902/main -I/usr/include/php/20190902/TSRM
...
libtool: install: cp ./.libs/swoole.so /path/to/swoole-src-4.6.1/modules/swoole.so
libtool: install: cp ./.libs/swoole.lai /path/to/swoole-src-4.6.1/modules/swoole.la
libtool: finish: PATH="/home/itaken/.cargo/bin:/home/itaken/go/bin:/home/itaken/.config/composer/vendor/bin:/home/itaken/Software/node/bin:/home/itaken/Software/anaconda3/bin:/home/itaken/Software/anaconda3/condabin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/sbin:/usr/sbin:/sbin" ldconfig -n /path/to/swoole-src-4.6.1/modules
\----------------------------------------------------------------------
Libraries have been installed in:
  /path/to/swoole-src-4.6.1/modules
If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the '-LLIBDIR'
flag during linking and do at least one of the following:
  - add LIBDIR to the 'LD_LIBRARY_PATH' environment variable
    during execution
  - add LIBDIR to the 'LD_RUN_PATH' environment variable
    during linking
  - use the '-Wl,-rpath -Wl,LIBDIR' linker flag
  - have your system administrator add LIBDIR to '/etc/ld.so.conf'
See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
\----------------------------------------------------------------------
Build complete.
Don't forget to run 'make test'.
```

## 安装, 将可执行文件复制到最终路径

```
swoole-src-4.6.1$ sudo make install
Installing shared extensions:     /usr/lib/php/20190902/
Installing header files:          /usr/include/php/20190902/
```

## 修改配置, 查看`phpinfo`, `php.ini` 文件路径.

>在`/etc/php/7.4/mods-available`新建文件`php.ini`, 写入`extension=swoole.so`
```
mods-available$ cat swoole.ini
extension=swoole.so
# 以下为创建超链接
$ sudo ln -s /etc/php/7.4/mods-available/swoole.ini  /etc/php/7.4/cli/20-swoole.ini
$ sudo ln -s /etc/php/7.4/mods-available/swoole.ini  /etc/php/7.4/fpm/20-swoole.ini
```

## 重启PHP服务

```
$ sudo service php7.4-fpm restart
$ sudo /usr/sbin/nginx -s reload
```

## 查看是安装成功

![swoole](/assets/images/202101/19-01.png)

----

如果系统没有安装`g++`, 在配置的时候有可能会报如下错误:

```
swoole-src-4.6.1$ ./configure
checking for grep that handles long lines and -e... /usr/bin/grep
checking for egrep... /usr/bin/grep -E
checking for a sed that does not truncate output... /usr/bin/sed
checking for pkg-config... /usr/bin/pkg-config
checking pkg-config is at least version 0.9.0... yes
...
checking for xlC_r... no
checking for xlC... no
checking whether we are using the GNU C++ compiler... no
checking whether g++ accepts -g... no
checking for valgrind... no
checking how to run the C++ preprocessor... /lib/cpp
configure: error: in `/path/to/swoole-src-4.6.1':
configure: error: C++ preprocessor "/lib/cpp" fails sanity check
See `config.log' for more details
```

---
## 参考文档
- [swoole/swoole-src](https://github.com/swoole/swoole-src/blob/master/README-CN.md)
- [C++ preprocessor “/lib/cpp” fails sanity check](https://askubuntu.com/questions/509663/c-preprocessor-lib-cpp-fails-sanity-check)
- [Compile from master fails with error ext/swoole/php_swoole.h: No such file or directory](https://github.com/swoole/ext-async/issues/3)
