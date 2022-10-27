---
title: "npm ERR! Cannot read properties of null (reading 'pickAlgorithm')"
date: 2022-10-27 20:40:44 +0800

categories: 一个问题
tags: [问题集, npm]
---


<mark>问题描述</mark>

```bash
itaken@itaken-PC:/path/to/itaken.github.io$ npm i
...
npm WARN deprecated source-map-url@0.4.0: See https://github.com/lydell/source-map-url#deprecated
npm WARN deprecated highlight.js@9.18.3: Version no longer supported. Upgrade to @latest
npm WARN deprecated highlight.js@10.3.2: Potential vulnerability. Please upgrade to @latest
npm ERR! Cannot read properties of null (reading 'pickAlgorithm')

npm ERR! A complete log of this run can be found in:
npm ERR!     /home/itaken/.npm/_logs/2022-10-27T12_38_20_756Z-debug-0.log
```

<mark>解决方案</mark>

>执行`npm cache clear --force` 清除npm缓存再重新安装`npm i`即可。


```bash
 itaken@itaken-PC:/path/to/itaken.github.io$ npm cache clear --force
npm WARN using --force Recommended protections disabled.
(base) itaken@itaken-PC:/path/to/itaken.github.io$ npm i
...
npm WARN deprecated chokidar@1.7.0: Chokidar 2 will break on node v14+. Upgrade to chokidar 3 with 15x less dependencies.
npm WARN deprecated highlight.js@10.3.2: Potential vulnerability. Please upgrade to @latest
npm WARN deprecated highlight.js@9.18.3: Version no longer supported. Upgrade to @latest

added 385 packages in 12s
```

---
## 参考文档
- [解决npm ERR! Cannot read properties of null (reading ‘pickAlgorithm‘)报错问题](https://blog.csdn.net/qq_43753895/article/details/126129113)
