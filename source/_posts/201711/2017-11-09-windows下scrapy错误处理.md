---
title: "windows下: ModuleNotFoundError: No module named 'win32api'"
date: 2017-11-09 17:05:47 +0800

categories: 一个问题
tags: [python, 问题集]
---

<mark>问题描述</mark>

```bash
PS D:\python\project> python -m scrapy runspider proxies.py
2017-11-09 11:25:18 [scrapy.utils.log] INFO: Scrapy 1.4.0 started (bot: scrapybot)
2017-11-09 11:25:18 [scrapy.utils.log] INFO: Overridden settings: {'SPIDER_LOADER_WARN_ONLY': True}
2017-11-09 11:25:18 [scrapy.middleware] INFO: Enabled extensions:
['scrapy.extensions.corestats.CoreStats',
 'scrapy.extensions.telnet.TelnetConsole',
 'scrapy.extensions.logstats.LogStats']
Unhandled error in Deferred:
2017-11-09 11:25:18 [twisted] CRITICAL: Unhandled error in Deferred:

2017-11-09 11:25:18 [twisted] CRITICAL:
Traceback (most recent call last):
  File "C:\path\to\Python\Python36\lib\site-packages\twisted\internet\defer.py", line 1386, in _inlineCallback
s
    result = g.send(result)
  File "C:\path\to\Python\Python36\lib\site-packages\scrapy\crawler.py", line 77, in crawl
    self.engine = self._create_engine()
  File "C:\path\to\Python\Python36\lib\site-packages\scrapy\crawler.py", line 102, in _create_engine
    return ExecutionEngine(self, lambda _: self.stop())
  File "C:\path\to\Python\Python36\lib\site-packages\scrapy\core\engine.py", line 69, in __init__
    self.downloader = downloader_cls(crawler)
  File "C:\path\to\Python\Python36\lib\site-packages\scrapy\core\downloader\__init__.py", line 88, in __init__
    self.middleware = DownloaderMiddlewareManager.from_crawler(crawler)
  File "C:\path\to\Python\Python36\lib\site-packages\scrapy\middleware.py", line 58, in from_crawler
    return cls.from_settings(crawler.settings, crawler)
  File "C:\path\to\Python\Python36\lib\site-packages\scrapy\middleware.py", line 34, in from_settings
    mwcls = load_object(clspath)
  File "C:\path\to\Python\Python36\lib\site-packages\scrapy\utils\misc.py", line 44, in load_object
    mod = import_module(module)
  File "C:\path\to\Python\Python36\lib\importlib\__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 994, in _gcd_import
  File "<frozen importlib._bootstrap>", line 971, in _find_and_load
  File "<frozen importlib._bootstrap>", line 955, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 665, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 678, in exec_module
  File "<frozen importlib._bootstrap>", line 219, in _call_with_frames_removed
  File "C:\path\to\Python\Python36\lib\site-packages\scrapy\downloadermiddlewares\retry.py", line 20, in <modu
le>
    from twisted.web.client import ResponseFailed
  File "C:\path\to\Python\Python36\lib\site-packages\twisted\web\client.py", line 42, in <module>
    from twisted.internet.endpoints import HostnameEndpoint, wrapClientTLS
  File "C:\path\to\Python\Python36\lib\site-packages\twisted\internet\endpoints.py", line 41, in <module>
    from twisted.internet.stdio import StandardIO, PipeAddress
  File "C:\path\to\Python\Python36\lib\site-packages\twisted\internet\stdio.py", line 30, in <module>
    from twisted.internet import _win32stdio
  File "C:\path\to\Python\Python36\lib\site-packages\twisted\internet\_win32stdio.py", line 9, in <module>
    import win32api
ModuleNotFoundError: No module named 'win32api'
```

<mark>解决方法</mark>

```bash
PS D:\python\project> python -m pip install pypiwin32
Collecting pypiwin32
  Downloading pypiwin32-220-cp36-none-win_amd64.whl (9.0MB)
    100% |████████████████████████████████| 9.0MB 96kB/s
Installing collected packages: pypiwin32
Successfully installed pypiwin32-220
```

---
## 参考文档
- [ImportError: no module named win32api](https://stackoverflow.com/questions/21343774/importerror-no-module-named-win32api)
