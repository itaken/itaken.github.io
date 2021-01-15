---
title: "CSS :before :after 选择器"
date: 2017-09-21 16:28:30 +0800
img: /assets/images/201709/21-01.png

categories: 开发日常
tags: [html]
---

## 伪元素

`css` 设置伪元素是为了方便给某些选择器添加特殊的效果。

### 一般格式

`selector::pseudo-element {property:value;}`

`css3` 已经明确规定了**伪类单冒号**，**伪元素双冒号** 的规则。

## :before

用来给指定的元素的内容前面插入新的内容。

### 配合伪类使用

伪元素 `:before` 还可以配合伪类使用, 例如: 与伪类 `:hover` 配合使用

`.demo:hover:before{content:'\1008'; color:#27ae60;}`

### 配合取值函数 `attr()` 使用

```html
<a href="http://xxx.com" title="title content"></a>

a::before{
    content: attr(title)
}
```

相当于 `<a href="http://xxx.com">title content</a>`

```html
<div propA="Barret" propB="Lee"></div>

div::after{
    content: attr(propA) attr(propB);
}
```

## :after

与 伪元素 `:before` 类型相同，只不过它指定的属性 `content` 值为出现在指定元素内容的后面。

## content 属性

对于伪元素 :before 和 :after 而言，属性 content 是必须设置的。

### 二进制反码

可以在 [block/box-drawing](http://www.charbase.com/block/box-drawing) 中寻找一些有趣的二进制反码, 例如:

![Box Drawing Block](/assets/images/201709/21-01.png)

---
## 参考文档
- [详解 CSS 属性 - :before && :after](https://segmentfault.com/a/1190000000474414)
- [CSS Content](https://css-tricks.com/css-content/)
- [Unicode Character 'MYANMAR LETTER E' (U+1027)](http://www.fileformat.info/info/unicode/char/1027/index.htm)
- [U+1027](http://unicode.scarfboy.com/wgbysfbxzk.html?s=U%2B1027)
- [charbase](http://www.charbase.com/)
- [Charinfo — "\u1027"](https://chars.suikawiki.org/string?s=%5Cu1027)
