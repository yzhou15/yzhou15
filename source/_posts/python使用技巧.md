---
title: python使用技巧
tags: python
categories: python
abbrlink: b868600f
date: 2021-02-22 00:00:00

---

转载自： https://inertia42.com/tips/pythontips/

## 在没有GUI的情况下使用matplotlib

在vps上运行调用matplotlib的python脚本时需在`import matplotlib.pyplot`前加上

```
import matplotlib as mpl
mpl.use('Agg')
```



一定要加在`import matplotlib.pyplot`前

## 给pip更换源

将windows下的pip源换为清华的源

只需要在user文件夹下新建pip文件夹，并在其中新建`pip.ini`文件，并写入：

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```



即可



