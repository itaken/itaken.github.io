---
title: "UOS 安裝 swoole"
date: 2021-06-23 17:11:11 +0800

categories: 开发日常
tags: [deepin, uos, php, laravel, swoole]
---

项目使用了swoole，需要安装swoole扩展。

```bash
itaken@itaken-home:/path/to/project$ php bin/laravels start
 _                               _  _____
| |                             | |/ ____|                                                  
| |     __ _ _ __ __ ___   _____| | (___                                                    
| |    / _` | '__/ _` \ \ / / _ \ |\___ \                                                   
| |___| (_| | | | (_| |\ V /  __/ |____) |                                                  
|______\__,_|_|  \__,_| \_/ \___|_|_____/                                                   

Speed up your Laravel/Lumen
>>> Components

In LaravelSCommand.php line 104:

  Call to undefined function Hhxsv5\LaravelS\Illuminate\swoole_version()  

[2021-06-21 10:47:58] [TRACE] Swoole is running, press Ctrl+C to quit.
PHP Warning:  Use of undefined constant SWOOLE_SOCK_UNIX_STREAM - assumed 'SWOOLE_SOCK_UNIX_STREAM' (this will throw an Error in a future version of PHP) in /path/to/project/vendor/hhxsv5/laravel-s/src/Swoole/Server.php on line 48
PHP Fatal error:  Uncaught Error: Class 'Swoole\Http\Server' not found in /path/to/project/vendor/hhxsv5/laravel-s/src/Swoole/Server.php:62
Stack trace:
#0 /path/to/project/vendor/hhxsv5/laravel-s/src/LaravelS.php(57): Hhxsv5\LaravelS\Swoole\Server->__construct(Array)
#1 /path/to/project/vendor/hhxsv5/laravel-s/src/Console/Portal.php(132): Hhxsv5\LaravelS\LaravelS->__construct(Array, Array)
#2 /path/to/project/vendor/hhxsv5/laravel-s/src/Console/Portal.php(56): Hhxsv5\LaravelS\Console\Portal->start()
#3 /path/to/project/vendor/symfony/console/Command/Command.php(255): Hhxsv5\LaravelS\Console\Portal->execute(Object(Symfony\Component\Console\Input\ArgvInput), Object(Symfony\Component\Console\Output\ConsoleOutput))
#4 /path/to/project/bin/laravels(12): Symfony\Component\Console\Command\Command->run(Object(Symfony\Component\Console\Input\ArgvInput), Object(Symfony\Component\Console\Output\ConsoleOutput))
#5 {main in /path/to/project/vendor/hhxsv5/laravel-s/src/Swoole/Server.php on line 62
...
```

>前置条件，需要安装`sudo apt-get install gcc build-essential`

## 下载源代码

```bash
itaken@itaken-home:~/Downloads$ git clone https://github.com/swoole/swoole-src.git && \
> cd swoole-src && \
> git checkout v4.6.x
正克隆到 'swoole-src'...
remote: Enumerating objects: 85502, done.
remote: Counting objects: 100% (1147/1147), done.
remote: Compressing objects: 100% (510/510), done.
remote: Total 85502 (delta 733), reused 964 (delta 632), pack-reused 84355
接收对象中: 100% (85502/85502), 45.10 MiB | 216.00 KiB/s, 完成.
处理 delta 中: 100% (65056/65056), 完成.
分支 'v4.6.x' 设置为跟踪来自 'origin' 的远程分支 'v4.6.x'。
切换到一个新分支 'v4.6.x'
```

## 编译`phpize`

```bash
itaken@itaken-home:~/Downloads/swoole-src$ phpize
Configuring for:
PHP Api Version:         20180731
Zend Module Api No:      20180731
Zend Extension Api No:   320180731

itaken@itaken-home:~/Downloads/swoole-src$ ./configure
checking for grep that handles long lines and -e... /usr/bin/grep
checking for egrep... /usr/bin/grep -E
checking for a sed that does not truncate output... /usr/bin/sed
...
checking whether the g++ linker (/usr/bin/ld -m elf_x86_64) supports shared libraries... yes
checking dynamic linker characteristics... (cached) GNU/Linux ld.so
checking how to hardcode library paths into programs... immediate
configure: creating ./config.status
config.status: creating config.h
config.status: executing libtool commands
```

## `make && make install`

```bash
itaken@itaken-home:~/Downloads/swoole-src$ make
/bin/bash /home/willike/Downloads/swoole-src/libtool --mode=compile g++  -DENABLE_PHP_SWOOLE -I. -I/home/willike/Downloads/swoole-src -DPHP_ATOM_INC -I/home/willike/Downloads/swoole-src/include -I/home/willike/Downloads/swoole-src/main -I/home/willike/Downloads/swoole-src -I/usr/include/php/20180731 -I/usr/include/php/20180731/main -I/usr/include/php/20180731/TSRM -I/usr/include/php/20180731/Zend -I/usr/include/php/20180731/ext -I/usr/include/php/20180731/ext/date/lib -I/home/willike/Downloads/swoole-src -I/home/willike/Downloads/swoole-src/include -I/home/willike/Downloads/swoole-src/ext-src -I/home/willike/Downloads/swoole-src/thirdparty/hiredis  -DHAVE_CONFIG_H  -g -O2 -Wall -Wno-unused-function -Wno-deprecated -Wno-deprecated-declarations -std=c++11   -c /home/willike/Downloads/swoole-src/ext-src/php_swoole.cc -o ext-src/php_swoole.lo

itaken@itaken-home:~/Downloads/swoole-src$ sudo make install
/bin/bash /home/willike/Downloads/swoole-src/libtool --mode=install cp ./swoole.la /home/willike/Downloads/swoole-src/modules
libtool: install: cp ./.libs/swoole.so /home/willike/Downloads/swoole-src/modules/swoole.so
libtool: install: cp ./.libs/swoole.lai /home/willike/Downloads/swoole-src/modules/swoole.la
libtool: finish: PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/sbin" ldconfig -n /home/willike/Downloads/swoole-src/modules
----------------------------------------------------------------------
Libraries have been installed in:
   /home/willike/Downloads/swoole-src/modules

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
----------------------------------------------------------------------
Installing shared extensions:     /usr/lib/php/20180731/
Installing header files:          /usr/include/php/20180731/
```

## 创建`swoole.ini`文件

```
itaken@itaken-home:~$ php -i
phpinfo()
PHP Version => 7.3.19-1+eagle

System => Linux willike-PC 5.7.7-amd64-desktop #3000 SMP Fri Dec 18 19:38:48 CST 2020 x86_64
Build Date => Nov 18 2020 07:21:12
Server API => Command Line Interface
Virtual Directory Support => disabled
Configuration File (php.ini) Path => /etc/php/7.3/cli
Loaded Configuration File => /etc/php/7.3/cli/php.ini
Scan this dir for additional .ini files => /etc/php/7.3/cli/conf.d
Additional .ini files parsed => /etc/php/7.3/cli/conf.d/10-mysqlnd.ini,
/etc/php/7.3/cli/conf.d/10-opcache.ini,
/etc/php/7.3/cli/conf.d/10-pdo.ini,
...
itaken@itaken-home:/usr/lib/php/20180731$ cd /etc/php/7.3/mods-available/
itaken@itaken-home:/etc/php/7.3/mods-available$ ll
总用量 136K
-rw-r--r-- 1 root root 72 11月 18  2020 bcmath.ini
...
-rw-r--r-- 1 root root 66 11月 18  2020 xsl.ini
-rw-r--r-- 1 root root 66 11月 18  2020 zip.ini

itaken@itaken-home:/etc/php/7.3/mods-available$ sudo cp zip.ini swoole.ini

itaken@itaken-home:/etc/php/7.3/mods-available$ cat swoole.ini
; configuration for swoole module
; priority=20
extension=swoole.so
```

## 创建软链

```bash
itaken@itaken-home:/etc/php/7.3/cli/conf.d$ sudo ln -s /etc/php/7.3/mods-available/swoole.ini 20-swoole.ini

itaken@itaken-home:/etc/php/7.3/fpm/conf.d$ sudo ln -s /etc/php/7.3/mods-available/swoole.ini 20-swoole.ini
```

## 重启`php-fpm`

```bash
itaken@itaken-home:~$ sudo service php7.3-fpm restart
```

## 启动项目

```
itaken@itaken-home:/path/to/project$ php bin/laravels start
 _                               _  _____
| |                             | |/ ____|                                                  
| |     __ _ _ __ __ ___   _____| | (___                                                    
| |    / _` | '__/ _` \ \ / / _ \ |\___ \                                                   
| |___| (_| | | | (_| |\ V /  __/ |____) |                                                  
|______\__,_|_|  \__,_| \_/ \___|_|_____/                                                   

Speed up your Laravel/Lumen
>>> Components
+----------------------+-------------------------------------------+
| Component            | Version                                   |
+----------------------+-------------------------------------------+
| PHP                  | 7.3.19-1+eagle                            |
| Swoole               | 4.6.8-dev                                 |
| LaravelS             | 3.4.4                                     |
| Laravel Framework [] | Lumen (5.8.12) (Laravel Components 5.8.*) |
+----------------------+-------------------------------------------+
>>> Protocols
+-----------+--------+-------------------+--------------+
| Protocol  | Status | Handler           | Listen At    |
+-----------+--------+-------------------+--------------+
| Main HTTP | On     | Laravel Framework | 0.0.0.0:5200 |
+-----------+--------+-------------------+--------------+
>>> Feedback: https://github.com/hhxsv5/laravel-s
[2021-06-21 11:20:20] [TRACE] Swoole is running, press Ctrl+C to quit.
...
```

---
## 参考文档
- [How to Install Swoole](https://www.swoole.co.uk/docs/get-started/installation)
- [安装 Swoole](https://wiki.swoole.com/#/environment)
- [configure、 make、 make install 背后的原理](https://zhuanlan.zhihu.com/p/77813702)
