---
title: next配置
tags: hexo
categories: hexo
abbrlink: c6146532
date: 2021-04-10 12:45:10
---



转载自：https://blog.csdn.net/qq_35396510/article/details/105953460

hexo 主题 next7.8 版本配置美化



转载自：https://www.jianshu.com/p/6f9e732b1f9f

Hexo的Next主题详细配置

72017.11.29 16:21:02字数 1,902阅读 53,047

经过一番不懈的努力，我们终于按照[Hexo免费搭建一个属于自己的博客](https://www.jianshu.com/p/51617690f8ca)搭建好了一个属于自己的博客，并且还安装了一个Next主题，但是我们的博客一开始还是很简陋的，我们需要把她装修一下。

> - 在 Hexo 中有两份主要的配置文件，其名称都是 _config.yml。 其中，一份位于站点根目录下，主要包含 Hexo 本身的配置；另一份位于主题目录下，这份配置由主题作者提供，主要用于配置主题相关的选项。
>   为了描述方便，在以下说明中，将前者称为***站点配置文件\***， 后者称为***主题配置文件\***。
> - 以下所有终端执行的命令都在你的Hexo根目录下

## 1、基本信息配置

> 基本信息包括：博客标题、作者、描述、语言等等。

打开 ***站点配置文件\*** ，找到Site模块



```undefined
title: 标题
subtitle: 副标题
description: 描述
author: 作者
language: 语言（简体中文是zh-Hans）
timezone: 网站时区（Hexo 默认使用您电脑的时区，不用写）
```

关于 ***站点配置文件\*** 中的其他配置可参考[站点配置](https://links.jianshu.com/go?to=https%3A%2F%2Fhexo.io%2Fzh-cn%2Fdocs%2Fconfiguration.html)

## 2、菜单设置

> 菜单包括：首页、归档、分类、标签、关于等等

我们刚开始默认的菜单只有首页和归档两个，不能够满足我们的要求，所以需要添加菜单，打开 ***主题配置文件\*** 找到`Menu Settings`



```cpp
menu:
  home: / || home                          //首页
  archives: /archives/ || archive          //归档
  categories: /categories/ || th           //分类
  tags: /tags/ || tags                     //标签
  about: /about/ || user                   //关于
  #schedule: /schedule/ || calendar        //日程表
  #sitemap: /sitemap.xml || sitemap        //站点地图
  #commonweal: /404/ || heartbeat          //公益404
```

看看你需要哪个菜单就把哪个取消注释打开就行了；
关于后面的格式，以`archives: /archives/ || archive`为例：
`||` 之前的`/archives/`表示标题“归档”，关于标题的格式可以去`themes/next/languages/zh-Hans.yml`中参考或修改
`||`之后的`archive`表示图标，可以去[Font Awesome](https://links.jianshu.com/go?to=http%3A%2F%2Ffontawesome.io%2Ficons%2F)中查看或修改，Next主题所有的图标都来自Font Awesome。

## 3、Next主题样式设置

我们百里挑一选择了Next主题，不过Next主题还有4种风格供我们选择，打开 ***主题配置文件\*** 找到`Scheme Settings`



```bash
# Schemes
# scheme: Muse
# scheme: Mist
# scheme: Pisces
scheme: Gemini
```

4种风格大同小异，本人用的是[Gemini](https://links.jianshu.com/go?to=https%3A%2F%2Fyoungerli.github.io)风格，你们可以选择自己喜欢的风格。

## 4、侧栏设置

> 侧栏设置包括：侧栏位置、侧栏显示与否、文章间距、返回顶部按钮等等

打开 ***主题配置文件\*** 找到`sidebar`字段



```cpp
sidebar:
# Sidebar Position - 侧栏位置（只对Pisces | Gemini两种风格有效）
  position: left        //靠左放置
  #position: right      //靠右放置

# Sidebar Display - 侧栏显示时机（只对Muse | Mist两种风格有效）
  #display: post        //默认行为，在文章页面（拥有目录列表）时显示
  display: always       //在所有页面中都显示
  #display: hide        //在所有页面中都隐藏（可以手动展开）
  #display: remove      //完全移除

  offset: 12            //文章间距（只对Pisces | Gemini两种风格有效）

  b2t: false            //返回顶部按钮（只对Pisces | Gemini两种风格有效）

  scrollpercent: true   //返回顶部按钮的百分比
```

## 5、头像设置

打开 ***主题配置文件\*** 找到`Sidebar Avatar`字段



```bash
# Sidebar Avatar
avatar: /images/header.jpg
```

这是头像的路径，只需把你的头像命名为`header.jpg`（随便命名）放入`themes/next/source/images`中，将`avatar`的路径名改成你的头像名就OK啦！

## 6、设置RSS

1、先安装 [hexo-generator-feed](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fhexojs%2Fhexo-generator-feed) 插件



```ruby
$ npm install hexo-generator-feed --save
```

2、打开 ***站点配置文件\*** 找到`Extensions`在下面添加



```bash
# RSS订阅
feed:
  type: atom
  path: atom.xml
  limit: 20
  hub:
  content:
  content_limit: 140
  content_limit_delim: ' '
```

3、打开 ***主题配置文件\*** 找到`rss`，设置为



```undefined
rss: /atom.xml
```

## 7、添加分类模块

1、新建一个分类页面



```cpp
$ hexo new page categories
```

2、你会发现你的`source`文件夹下有了`categorcies/index.md`，打开`index.md`文件将title设置为`title: 分类`
3、打开 ***主题配置文件\*** 找到`menu`，将categorcies取消注释
4、把文章归入分类只需在文章的顶部标题下方添加`categories`字段，即可自动创建分类名并加入对应的分类中
举个栗子：



```undefined
title: 分类测试文章标题
categories: 分类名
```

## 8、添加标签模块

1、新建一个标签页面



```cpp
$ hexo new page tags
```

2、你会发现你的`source`文件夹下有了`tags/index.md`，打开`index.md`文件将title设置为`title: 标签`
3、打开 ***主题配置文件\*** 找到`menu`，将tags取消注释
4、把文章添加标签只需在文章的顶部标题下方添加`tags`字段，即可自动创建标签名并归入对应的标签中
举个栗子：



```undefined
title: 标签测试文章标题
tags: 
  - 标签1
  - 标签2
  ...
```

## 9、添加关于模块

1、新建一个关于页面



```cpp
$ hexo new page about
```

2、你会发现你的`source`文件夹下有了`about/index.md`，打开`index.md`文件即可编辑关于你的信息，可以随便编辑。
3、打开 ***主题配置文件\*** 找到`menu`，将about取消注释

## 10、添加搜索功能

1、安装 [hexo-generator-searchdb](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fflashlab%2Fhexo-generator-search) 插件



```ruby
$ npm install hexo-generator-searchdb --save
```

2、打开 ***站点配置文件\*** 找到`Extensions`在下面添加



```bash
# 搜索
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

3、打开 ***主题配置文件\*** 找到`Local search`，将`enable`设置为`true`

## 11、添加阅读全文按钮

因为在你的博客主页会有多篇文章，如果你想让你的文章只显示一部分，多余的可以点击阅读全文来查看，那么你需要在你的文章中添加



```xml
<!--more-->
```

其后面的部分就不会显示了，只能点击阅读全文才能看

## 12、修改文章内链接文本样式



![img](next%E9%85%8D%E7%BD%AE/4120931-9cd87c2cc2d0c78f.gif)

效果图


打开文件 `themes/next/source/css/_common/components/post/post.styl`，在末尾添加





```ruby
.post-body p a {
  color: #0593d3;
  border-bottom: none;
  border-bottom: 1px solid #0593d3;
  &:hover {
    color: #fc6423;
    border-bottom: none;
    border-bottom: 1px solid #fc6423;
  }
}
```

其中选择 .post-body 是为了不影响标题，选择 p 是为了不影响首页“阅读全文”的显示样式,颜色可以自己定义。

## 13、设置网站缩略图标

> 从网上看了很多设置方法都是说把favicon.ico放到站点目录的source目录下就可以了，可是我试了好多遍，并不行。

我的设置方法是这样的：把你的图片（png或jpg格式，不是favicon.ico）放在`themes/next/source/images`里，然后打开 ***主题配置文件\*** 找到`favicon`，将`small、medium、apple_touch_icon`三个字段的值都设置成`/images/图片名.jpg`就可以了，其他字段都注释掉。

![img](next%E9%85%8D%E7%BD%AE/4120931-61a0cc555a25548e.png)



## 14、设置文章字体的颜色、大小

![img](next%E9%85%8D%E7%BD%AE/4120931-e471abbb2b1f459f.png)

效果图



如果想设置某一句的颜色或大小，只需用html语法写出来就行了



```xml
接下来就是见证奇迹的时刻
<font color="#FF0000"> 我可以设置这一句的颜色哈哈 </font> 
<font size=6> 我还可以设置这一句的大小嘻嘻 </font> 
<font size=5 color="#FF0000"> 我甚至可以设置这一句的颜色和大小呵呵</font> 
```

## 15、设置文字居中

设置方法：



```xml
<center>这一行需要居中</center>
```

> 注意：简书中此方法无效

## 16、添加评论系统

> 目前国内比较有名的多说、网易云跟帖评论系统都已停止服务了，国外的Disqus评论系统还得需要翻墙，所以不推荐使用，剩下的还有搜狐畅言、友言、来必力等。
> 本来想使用畅言的，结果注册完之后还得要求备案，我只想说F开头的那个单词，果断放弃。
> 后来选择了友言

1、进入[友言官网](https://links.jianshu.com/go?to=http%3A%2F%2Fwww.uyan.cc)注册、登录步骤我就不介绍了
2、登录完成之后，点击获取代码，你会发现出来了一段代码，里面有你的`uid=1234567`
3、打开 ***主题配置文件\*** 找到`youyan_uid`将值设置为上面的uid就可以了

## 17、添加站点访问计数

站点访问计数有名的就是[不蒜子](https://links.jianshu.com/go?to=http%3A%2F%2Fbusuanzi.ibruce.info)，使用起来非常方便
1、安装脚本
打开 **themes/next/layout/_partial/footer.swig**，将下面这段代码添加到里面



```xml
<div>
<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
<span id="busuanzi_container_site_pv" style='display:none'>
    本站总访问量 <span id="busuanzi_value_site_pv"></span> 次
    <span class="post-meta-divider">|</span>
</span>
<span id="busuanzi_container_site_uv" style='display:none'>
    有<span id="busuanzi_value_site_uv"></span>人看过我的博客啦
</span>
</div>
```

添加的位置如下图，可自行根据个人喜好更换位置

![img](next%E9%85%8D%E7%BD%AE/4120931-690e9b69d7901c54.png)


2、以上只是显示站点的访问次数，如果想显示每篇文章的访问次数，打开 **themes/next/layout/_macro/post.swig**，在第一行增加`is_pv`字段





```rust
{% macro render(post, is_index, is_pv, post_extra_class) %}
```

然后将这段代码插入到里面



```jsx
{% if is_pv %}
  <span class="post-meta-divider">|</span>
  <span id="busuanzi_value_page_pv"></span>次阅读
{% endif %}
```

插入的位置

![img](next%E9%85%8D%E7%BD%AE/4120931-bdbc90ff2c4285a0.png)


然后再打开 **themes/next/layout/post.swig**，这个文件是文章的模板，给render方法传入参数（对应刚才添加的`is_pv`字段）

![img](next%E9%85%8D%E7%BD%AE/4120931-8418d415c31ff529.png)


最后再打开 **themes/next/layout/index.swig**，这个文件是首页的模板，给render方法传入参数（对应刚才添加的is_pv字段）

![img](next%E9%85%8D%E7%BD%AE/4120931-5d89c00ea392bf01.png)


OK！设置完毕。



## 18、去掉文章目录标题的自动编号

我们自己写文章的时候一般都会自己带上标题编号，但是默认的主题会给我们带上编号，很是别扭，如何去掉呢？
打开***主题配置文件\***，找到

![img](next%E9%85%8D%E7%BD%AE/4120931-dcd17d644851e21f.png)

将`number`改为`false`即可



## 18、更多

1、还有其他更多的主题配置，请查看[主题配置](https://links.jianshu.com/go?to=http%3A%2F%2Ftheme-next.iissnan.com%2Ftheme-settings.html)
2、还有其他更多的插件，请查看[Hexo插件](https://links.jianshu.com/go?to=https%3A%2F%2Fhexo.io%2Fplugins%2F)





