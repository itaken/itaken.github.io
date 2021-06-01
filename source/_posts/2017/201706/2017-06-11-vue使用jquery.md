---
title: "vue使用jquery"
date: 2017-06-11 11:01:12 +0800

categories: 开发日常
tags: [ubuntu, vue, jquery]
---

1. 使用 **npm** 添加 `jquery` 包

1. webpack 配置添加 jquery插件

    ```bash
    module.exports = merge(baseWebpackConfig, {
        ...
        plugins: [
            new webpack.ProvidePlugin({
                $: 'jquery'
            })
        ]
    }
    ```

1. 在项目JavaScript中直接 使用 `$` 即可调用jquery.

---
## 参考文档
- [webpack引入jquery多种方法探究](https://segmentfault.com/a/1190000007249293)
- [VUE引入jquery以后，调用方法，提示'$' is not defined。](https://segmentfault.com/q/1010000007350626)
