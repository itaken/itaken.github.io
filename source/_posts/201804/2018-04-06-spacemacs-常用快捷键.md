---
title: "spacemacs 快捷键"
date: 2018-04-06 22:04:01 +0800

categories: 开发日常
tags: [emacs]
---

```
spc : 跳出命令面板
spc-spc : 跳出命令列表，可运行命令，也可以查找快捷键
spc-h-spc : 查找包的用途与定义

spc-tab 切换到上一个 buffer
spc-f-t 打开当前文件所在的目录
```

- `SPC !` shell终端
- `SPC SPC php` 安装`PHP`相关插件
- `SPC f e d` 快速打开配置文件 `.spacemacs`
- `SPC q R` 重启 `emacs`
- `SPC t n` 切换行号

- `SPC p p` 切换项目
- `SPC p f` 在项目中搜索文件名
- `SPC p R` 在项目中替换字符串，根据提示输入「匹配」和「替换」的字符串，然后输入替换的方式
    - E 修改刚才输入的「替换」字符串
    - RET 表示不做处理
    - y 表示只替换一处
    - Y 表示替换全部
    - n 或 delete 表示跳过当前匹配项，匹配下一项
    - ^ 表示跳过当前匹配项，匹配上一项
    - , 表示替换当前项，但不移动光标，可和 n 或 ^ 配合使用
- `SPC p f` 查找项目文件

- `SPC f f` 打开文件（夹），相当于 `$ open xxx` 或 `$ cd /path/to/project`
- `SPC f R` 重命名当前文件
- `SPC f s` 保存一个buffer
- `SPC f S` 保存所有buffer
- `SPC f r` 最近打开的文件列表
- `SPC b N` 新建buffer, :w xxx保存为xxx文件
- `SPC b p` 上一个buffer
- `SPC b n` 下一个buffer
- `SPC TAB` 两个buffer切换

- `SPC j i` 显示类的所有方法

- `SPC q Q` 关闭emacs，但会丢失所有未保存的修改。
- `SPC b d` 关闭当前 buffer
- `SPC w d` 关闭当前窗口
- `SPC w /` 右分割窗口
- `SPC w -` 下分割窗口

- `SPC f y` 复制并显示当前buffer文件名, 完整路径.

- `SPC 0` 光标跳转到侧边栏（NeoTree）中
- `SPC n`(数字) 光标跳转到第 n 个 buffer 中

- `SPC s s` 或 `SPC s S` 在当前buffer里搜索关键词


### 显示行号
修改`.spacemacs`
`dotspacemacs-line-numbers nil` 改为 `dotspacemacs-line-numbers t`


---
## 参考文档
- [Spacemacs 使用总结](https://scarletsky.github.io/2016/01/22/spacemacs-usage/)
- [写给 Pythoner 的 Spacemacs 入门指北](https://zhuanlan.zhihu.com/p/24900429)
- [Spacemacs Rocks 第二季](https://github.com/emacs-china/Spacemacs-rocks)
