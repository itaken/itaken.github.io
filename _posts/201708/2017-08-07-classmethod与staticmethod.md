---
layout: post

title: "python classmethod 与 staticmethod 的区别"
date: 2017-08-07 15:50:47 +0800

categories: post
tags: [python]
---

```python
#!/usr/bin/env python3

class Kls(object):
    def __init__(self, data=None):
        self.data = data

    def printd(self):
        print(self.data)

    @staticmethod
    def smethod(*arg):
        print('Static:', arg)

    @classmethod
    def cmethod(cls, *arg):
        print('Class:', arg)


ik = Kls("TEST")

Kls.printd(ik)
ik.printd()

Kls.smethod()
ik.smethod()

Kls.cmethod()
ik.cmethod()
```

**大同小异**

- 都是装饰器(函数修饰符)
- 调用的时候, 都不需要先实例化类, 可以直接使用`类名.方法名()`调用.
- `@classmethod` 第一个参数是自身类, 可以使用类的属性参数, 而`@staticmethod` 则只能使用**类名.属性名**或**类名.方法名**
- `@staticmethod`基本上跟一个全局函数相同，较少会使用到

---
更多阅读
- [What is the difference between @staticmethod and @classmethod in Python?](https://stackoverflow.com/questions/136097/what-is-the-difference-between-staticmethod-and-classmethod-in-python)
- [PYTHON中STATICMETHOD和CLASSMETHOD的差异](http://www.wklken.me/posts/2013/12/22/difference-between-staticmethod-and-classmethod-in-python.html)
- [CLASS METHOD VS STATIC METHOD 2017](http://www.bogotobogo.com/python/python_differences_between_static_method_and_class_method_instance_method.php)
- [装饰器@staticmethod和@classmethod有什么区别?](https://taizilongxu.gitbooks.io/stackoverflow-about-python/content/14/README.html)
- [@staticmethod和@classmethod的作用与区别](http://blog.csdn.net/handsomekang/article/details/9615239)
