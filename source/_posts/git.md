---
title: Git基本配置
date: '2021/1/17 18:18'
abbrlink: f73c18d5'
tags: git
categories: git
---

1.全局配置

```
git config --global user.name **
git config --global user.email ***@**.com
git config --global push.default matching
git config --global core.quotepath false
git config --global core.editor "vim"
git config -l;
```

2.密钥生成

```
ssh-keygen -t rsa -b 4096 -C ***@qq.com
cat ~/.ssh/id_rsa.pub
ssh -T git@github.com
ssh -T git@gitee.com
```

3.远程仓库连接

```
git remote rm origin
git remote add github git@github.com:****/learngit.git
git remote add gitee git@gitee.com:*****/learngit.git
git remote -v
git push github master
git push gitee master
```

![](git/git.png)