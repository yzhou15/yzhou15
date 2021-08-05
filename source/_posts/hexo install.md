---
title: hexo install
date: '2021/1/13 11:00'
abbrlink: 599c6dab
tags: hexo
categories: hexo
---

1、安装之前可以先设置一下淘宝镜像加速器
	`npm install -g cnpm --registry=https://registry.npm.taobao.org`
2、全局安装框架
	`npm install hexo-cli -g`
1、创建你的博客目录
 	`hexo init 你博客的文件夹名字`
2、进入你博客的目录
	`cd 你博客的文件夹名字`
3、复制文件到你博客的目录
	`npm install`
4、安装Hexo部署插件
5、请在你博客的目录下启动cmd，再执行以下代码

```
npm install hexo-deployer-git --save
```

6、打开你博客根目录的 _config.yml 文件，将以下信息添加到里面去。

```
deploy:
  type: git
  repo: git@gitee.com:yzhou15/yzhou15.git
  # https://github.com/<username>/<project>
  # example, https://github.com/hexojs/hexojs.github.io
  branch: gh-pages
```

7、hexo cl&hexo g& hexo s
8、hexo d
每次部署完git pages要点更新

