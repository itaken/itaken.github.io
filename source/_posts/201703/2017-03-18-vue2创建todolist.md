---
title: "使用vue 2创建todolist"
date: 2017-03-18 17:36:01 +0800
img: /assets/images/201703/18-01.png

categories: 开发日常
tags: [vue]
---

1. 创建项目目录

    ```bash
    $ mkdir vue2-todolist
    $ cd vue2-todolist
    ```

2. 使用 vue-cli 工具初始化项目.

    ```bash
    $ vue init webpack .

    ? Generate project in current directory? Yes
      This will install Vue 2.x version of the template.

      For Vue 1.x use: vue init webpack#1.0

    ? Project name vue2-todolist
    ? Project description This a vue2 todolist.
    ? Author itaken <regelhh@gmail.com>
    ? Vue build standalone
    ? Install vue-router? Yes
    ? Use ESLint to lint your code? No
    ? Setup unit tests with Karma + Mocha? No
    ? Setup e2e tests with Nightwatch? No

       vue-cli · Generated "vue2-todolist".

       To get started:

         npm install
         npm run dev

       Documentation can be found at https://vuejs-templates.github.io/webpack
    ```

    > 说明: 本项目可以**不**使用vue-router

3. 运行项目

    ```bash
    $ sudo cnpm install
    $ sudo npm run dev
    ```
4. 完善项目
    - 安装 sass插件, 支持 scss [^1] [^2]

    ```bash
    $ sudo cnpm install node-sass --save-dev
    $ sudo cnpm install sass-loader --save-dev
    ```

    - 引入localstorage 本地存储.

    ```javascript
    // 引入 本地存储方法
    import "./assets/js/store.modern.min.js";
    import Vue from 'vue'
    Vue.use(store);
    ```
    - 监控TODO items变化,并存储

    ```javascript
    watch:{
        items:{
            handler: function(items){
                Storage.save(items);
            },
            deep: true
        }
    }
    ```

5. 效果如下

    ![TODOList](/assets/images/201703/18-01.png)

> 项目源码: [vue2-todolist](https://github.com/itaken/vue2-todolist)

---
## 参考文档
- [vue.js入门基础](http://www.imooc.com/video/12305)
- [通过todoMVC来学vue.js的使用](http://www.jianshu.com/p/82778a67ea57#)
- [PURE css](https://purecss.io)
- [How to use "scss" in *.vue?](https://github.com/vuejs/vue-loader/issues/363)
- [Advanced Loader Configuration](https://vue-loader.vuejs.org/en/configurations/advanced.html)
- [如何写一手漂亮的 Vue](https://jeffjade.com/2017/03/11/120-how-to-write-vue-better/)


[^1]: https://github.com/vuejs/vue-loader/issues/363
[^2]: https://vue-loader.vuejs.org/en/configurations/advanced.html
