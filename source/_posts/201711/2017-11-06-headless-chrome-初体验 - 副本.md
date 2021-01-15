---
title: "在Windows下体验headless chrome for python"
date: 2017-11-06 22:23:30 +0800

categories: 开发日常
tags: [python]
---

>环境配置
```
windows 10
python 3.6
chrome 60
```

>实现python 截图,  代码来源自 [https://gist.github.com/rverton/d07a2232f4c0e1c2b9894e9bdb4fa6cf](https://gist.github.com/rverton/d07a2232f4c0e1c2b9894e9bdb4fa6cf)

1. 安装chrome (版本号最好高于59)

2. 使用pip安装 `selenium`
```bash
pip install selenium
```

3. 下载 `WebDriver for Chrome`, 下载地址 [https://sites.google.com/a/chromium.org/chromedriver/downloads](https://sites.google.com/a/chromium.org/chromedriver/downloads)

4. 修改`CHROME_PATH`和`CHROMEDRIVER_PATH`, 运行代码:

```python
#!/usr/bin/evn python
# coding=utf8
'''
python 页面截图
'''

from selenium import webdriver
from selenium.webdriver.chrome.options import Options

WINDOW_SIZE = "1920,1048"  # 窗体大小
CHROME_PATH = r"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe"  # chrome 路径
CHROMEDRIVER_PATH = r"C:\chromedriver_win32\chromedriver.exe" # driver 路径  

chrome_options = Options()
chrome_options.add_argument("--headless")
chrome_options.add_argument("--window-size=%s" % WINDOW_SIZE)
chrome_options.binary_location = CHROME_PATH

'''
生成快照
'''
def take_shot(url, path="screenshot.png"):
    chrome_driver = webdriver.Chrome(
        executable_path=CHROMEDRIVER_PATH, 
        chrome_options=chrome_options
    )

    chrome_driver.get(url)
    chrome_driver.save_screenshot(path)  # 保存快照
    # print(chrome_driver.current_url)  # 当前 url
    chrome_driver.close()


'''
主函数
'''
if __name__ == '__main__':
    take_shot("http://www.baidu.com", path="screenshot.png")

```
