---
title: "The box 'ubuntu/xenial64' could not be found or could not be accessed in the remote catalog."
date: 2018-04-30 09:27:39 +0800

categories: 一个问题
tags: [vagrant, 问题集]
---

<mark>问题描述</mark>

```bash
$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'ubuntu/xenial64' could not be found. Attempting to find and install...
    default: Box Provider: virtualbox
    default: Box Version: >= 0
The box 'ubuntu/xenial64' could not be found or
could not be accessed in the remote catalog. If this is a private
box on HashiCorp's Atlas, please verify you're logged in via
`vagrant login`. Also, please double-check the name. The expanded
URL and error message are shown below:

URL: ["https://atlas.hashicorp.com/ubuntu/xenial64"]
Error: The requested URL returned error: 404 Not Found
```

<mark>解决方法</mark>

在`Vagrantfile`文件中, 添加`Vagrant::DEFAULT_SERVER_URL.replace('https://vagrantcloud.com')`即可.

```bash
$ vagrant up                                                              1 ↵
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'ubuntu/xenial64' could not be found. Attempting to find and install...
    default: Box Provider: virtualbox
    default: Box Version: >= 0
==> default: Loading metadata for box 'ubuntu/xenial64'
    default: URL: https://vagrantcloud.com/ubuntu/xenial64
==> default: Adding box 'ubuntu/xenial64' (v20180504.0.0) for provider: virtualbox
    default: Downloading: https://vagrantcloud.com/ubuntu/boxes/xenial64/versions/20180504.0.0/providers/virtualbox.box
    default: Progress: 76% (Rate: 6684k/s, Estimated time remaining: 0:00:12)
    ...

$ vagrant box list
ubuntu/xenial64 (virtualbox, 20180504.0.0)
```

---
## 参考文档
- [Vagrant cannot find box
](https://stackoverflow.com/questions/35519389/vagrant-cannot-find-box?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)
