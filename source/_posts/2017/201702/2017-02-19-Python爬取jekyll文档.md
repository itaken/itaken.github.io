---
title: "使用Python爬取jekyll文档"
date: 2017-02-19 17:20:05 +0800

categories: 开发日常
tags: [python, 爬虫]
---

```
开发环境配置
Ubuntu: 16.10
Python: 3.5.2+
wkhtmltopdf: 0.12.4 (with patched qt)
```

## 获取页面URL列表 [^1]

```python
def get_url_list(url):
    response = requests.get(url)
    if response.status_code != 200:
        return
    soup = BeautifulSoup(response.content, "html5lib")
    aside_links = soup.find_all("aside")[1]  # 获取侧边栏内容
    urls = []
    for li in aside_links.find_all("li"):
        # 侧边栏 所有链接
        a_url = "https://jekyllrb.com/" + li.a.get("href")
        urls.append(a_url)

    return urls
```

## 解析页面, 保存成html文件

```python
def parse_url_to_html(url):
    response = requests.get(url)
    if response.status_code != 200:
        return

    soup = BeautifulSoup(response.content, 'html.parser')
    body = soup.find_all(class_="unit four-fifths")[0]  # 获取文档内容
    html = str(body)
    html = html_template.format(content=html)
    file_path = "./data/demo.html"
    with open(file_path, "wb") as f:
        f.write(bytes(html, "utf_8"))  # string转byte

    return file_path
```

## 将html文件保存为`pdf`

```python
def save_pdf(file_path):
    options = {
        'page-size': 'Letter',
        'encoding': "UTF-8",
        'custom-header': [
            ('Accept-Encoding', 'gzip')
        ]
    }
    pdfkit.from_file(file_path, "./data/demo.pdf", options=options)
```

> 完整代码 : [jekyll-document-to-pdf](https://github.com/itaken/python-example/tree/master/jekyll-document-to-pdf)

---
## 参考文档
- [beautifulsoup文档](http://beautifulsoup.readthedocs.io/zh_CN/v4.4.0/index.html?highlight=new_tag)
- [jekyll_spider_demo.py](https://github.com/itaken/python-example/blob/master/jekyll-document-to-pdf/jekyll_spider_demo.py)

[^1]: https://github.com/itaken/python-example/blob/master/jekyll-document-to-pdf/jekyll_spider_demo.py
