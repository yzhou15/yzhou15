---
title: hexo使用技巧
date: '2021-02-22 12:02'
tags: hexo
abbrlink: 9c40ea82
categories: hexo
---

切换主题报错：

```js
{% extends '_layout.swig' %} {% import '_macro/post.swig' as post_template %} {% import '_macro/sidebar.swig' as sidebar_template %} {% block title %}{{ config.title }}{% if theme.index_with_subtitle and config.subtitle %} - {{config.subtitle }}{% endif %}{% endblock %} {% block page_class %} {% if is_home() %}page-home{% endif -%} {% endblock %} {% block content %}

{% for post in page.posts %} {{ post_template.render(post, true) }} {% endfor %}

{% include '_partials/pagination.swig' %} {% endblock %} {% block sidebar %} {{ sidebar_template.render(false) }} {% endblock %}
```



原因是hexo在5.0之后把swig给删除了需要自己手动安装

```js
npm i hexo-renderer-swig
```





转载自： https://inertia42.com/tips/tipsofhexo/

## Hexo添加阅读全文标签

在文章中添加`<!--more-->`标签可以使文章显示摘要和阅读全文按钮

## Hexo中的Markdown

Hexo支持GitHub Flavored Markdown语法

## 在首页隐藏某些特定文章

该方法取自[淡之梦的文章](https://www.jianshu.com/p/79fe9fb9dfa0)

在hexo安装目录下找到\theme\next\layout\index.swig,打开后会看到

```
{% extends '_layout.swig' %}
{% import '_macro/post.swig' as post_template %}
{% import '_macro/sidebar.swig' as sidebar_template %}

{% block title %}{{ title }}{% if theme.index_with_subtitle and subtitle %} – {{ subtitle }}{% endif %}{% endblock %}

{% block page_class %}
  {% if is_home() %}page-home{% endif -%}
{% endblock %}

{% block content %}
  <section id="posts" class="posts-expand">
    {% for post in page.posts %}
	{{ post_template.render(post, true) }}
    {% endfor %}
  </section>

  {% include '_partials/pagination.swig' %}
{% endblock %}

{% block sidebar %}
  {{ sidebar_template.render(false) }}
{% endblock %}
```



将其中的

```
{% block content %}
  <section id="posts" class="posts-expand">
    {% for post in page.posts %}
        {{ post_template.render(post, true) }}
    {% endfor %}
  </section>

  {% include '_partials/pagination.swig' %}
{% endblock %}
```



修改为

```
{% block content %}
  <section id="posts" class="posts-expand">
    {% for post in page.posts %}
        {% if post.notshow != true %}
            {{ post_template.render(post, true) }}
        {% endif %}
    {% endfor %}
  </section>

  {% include '_partials/pagination.swig' %}
{% endblock %}
```



之后在博文头部使用`notshow`参数隐藏文章，加入`notshow: true`即可

```
title: title
date: 2018-06-12 11:45:43
tags: 
notshow: true
```



## 在文章底部显示copyright信息

以`next`主题为例，在**主题**配置文件中找到以下内容：

```
post_copyright:
  enable: false
  license: <a href="https://creativecommons.org/licenses/by-nc-sa/4.0/" rel="external nofollow" target="_blank">CC BY-NC-SA 4.0</a>
```



将其中的`false`改为`true`，然后在**博客**配置文件中找到：

```
url: http://yoursite.com
```



> 注意，如果你的博客使用了https加密，请把url改为`https://yoursite.com`
> 将其中的地址改为自己的博客地址即可。

