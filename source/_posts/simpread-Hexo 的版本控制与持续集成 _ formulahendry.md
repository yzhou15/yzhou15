---
title: [转载]simpread-Hexo的版本控制和持续集成
tags: hexo 
categories: hexo
abbrlink: 
date: 2021-08-05 18:05
---

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [formulahendry.github.io](https://formulahendry.github.io/2016/12/04/hexo-ci/)

> 想必很多人会把 Hexo 生成出来的静态网站放到 GitHub Pages 来进行托管。

想必很多人会把 Hexo 生成出来的静态网站放到 GitHub Pages 来进行托管。一般发布 Hexo 博客的流程是，首先在本地搭建 Hexo 的环境，编写新文章，然后利用`hexo deploy`来发布到 Git。那么对于本地的 Hexo 的原始文件怎么管理呢？如果换电脑了怎么办？如果没有对原始文件进行备份，突然有一天你的本地环境挂了导致源文件丢失，那不就呵呵了。也许你会想到用 Dropbox 或者其他方案来对源文件进行备份，但是每次更新完博客，需要备份好源文件，然后执行`hexo deploy`进行发布，是不是很麻烦？换了电脑之后又要重新搭建本地环境，是不是很蛋疼？

那么接下来我们就来说说如何优雅愉快地对我们的 Hexo 进行版本管理和发布。  

既然我们已经用了 GitHub 来托管我们生成出来的静态网站，那么为什么不也把 Hexo 博客的源文件也 host 在 GitHub 上呢。那么问题来了，如果我们把 Hexo 博客的源文件托管在 GitHub 上，我们的发布流程就会变为：

1.  执行`git push`把更新的源文件 push 到托管源文件的 GitHub Repo (我们称之为 Source Repo)
2.  执行`hexo deploy`来更新托管静态网站的 GitHub Pages (我们称之为 Content Repo)

这样看来，每次更新博客要经历两个步骤，并不是那么 straightforward。那么有没有办法做到既能使用 GitHub 进行版本控制，又能做到一键发布呢？

答案是肯定的。这里用到了[持续集成](https://en.wikipedia.org/wiki/Continuous_integration)也就是我们一直所说的 CI 来完成一键发布：当有新的 change push 到 Source Repo 时，自动执行 CI 脚本，生成最新的静态网站发布到 Content Repo，一气呵成。那么我使用什么 CI 工具来做呢？我们可以使用像 Travis CI 这样的 Hosted CI Service，也可以使用 Jenkins 或者 TeamCity 来搭建 CI server。如果自己来搭建 CI Server，费时费力，又要花钱来买 Server 来 host CI service，肯定不是一个很好的选择。那么我们选哪个 Hosted CI Service 呢？其实今年在公司的一个项目中我们就选择了 AppVeyor。当初在做 investigation 的时候，第一个想到的就是用 Travis CI，然而我司大多数的开发环境都是 Windows，而且当时的项目有 Python, PowerShell, Java 等，那时 PowerShell 还只支持 Windows，所以需要选择一个支持 Windows 的 CI Service。于是，Scott Hanselman 安利的 [AppVeyor](http://www.hanselman.com/blog/AppVeyorAGoodContinuousIntegrationSystemIsAJoyToBehold.aspx) 就成为了一个备选。访问 [AppVeyor 官网](https://www.appveyor.com/)，映入眼帘的大标题就是`#1 Continuous Delivery service for Windows`。刚开始的时候内心一阵嘲笑，Top 10 的 CI Service 就你支持 Windows，你不是第一那谁是第一？结果在之后的项目使用中，发现 AppVeyor 比 Travis CI 好用太多。这里就不具体展开了，继续进入正题。

使用 AppVeyor 来建立 CI 非常方便，主要是以下步骤：

1.  注册并登陆 AppVeyor
    
    访问 [AppVeyor 登陆页面](https://ci.appveyor.com/login)，使用你的 GitHub 账号登陆即可。  
    ![](https://formulahendry.github.io/assets/img/hexo-ci/appveyor-login.png)
    
2.  添加 Project
    
    在 [AppVeyor Projects 页面](https://ci.appveyor.com/projects/new)，添加相应的 GitHub Source Repo。  
    ![](https://formulahendry.github.io/assets/img/hexo-ci/appveyor-add-project.png)
    
3.  添加 appveyor.yml 到 Source Repo
    
    接下来，你需要把 appveyor.yml 添加到 Source Repo 的根目录下。具体的 appveyor.yml 如下，也可以参考我博客的 [appveyor.yml](https://github.com/formulahendry/formulahendry.github.io.source/blob/master/appveyor.yml)
    
    ```
    clone_depth: 5
    
    environment:
    
    access_token:
    
        secure: [Your GitHub Access Token]
    
    install:
    
    - node --version
    
    - npm --version
    
    - npm install
    
    - npm install hexo-cli -g
    
    build_script:
    
    - hexo generate
    
    artifacts:
    
    - path: public
    
    on_success:
    
    - git config --global credential.helper store
    
    - ps: Add-Content "$env:USERPROFILE\.git-credentials" "https://$($env:access_token):x-oauth-basic@github.com`n"
    
    - git config --global user.email "%GIT_USER_EMAIL%"
    
    - git config --global user.name "%GIT_USER_NAME%"
    
    - git clone --depth 5 -q --branch=%TARGET_BRANCH% %STATIC_SITE_REPO% %TEMP%\static-site
    
    - cd %TEMP%\static-site
    
    - del * /f /q
    
    - for /d %%p IN (*) do rmdir "%%p" /s /q
    
    - SETLOCAL EnableDelayedExpansion & robocopy "%APPVEYOR_BUILD_FOLDER%\public" "%TEMP%\static-site" /e & IF !ERRORLEVEL! EQU 1 (exit 0) ELSE (IF !ERRORLEVEL! EQU 3 (exit 0) ELSE (exit 1))
    
    - git add -A
    
    - if "%APPVEYOR_REPO_BRANCH%"=="master" if not defined APPVEYOR_PULL_REQUEST_NUMBER (git diff --quiet --exit-code --cached || git commit -m "Update Static Site" && git push origin %TARGET_BRANCH% && appveyor AddMessage "Static Site Updated")
    ```
    
    你唯一需要做的就是替换 [Your GitHub Access Token]，关于生成 Access Token，可以参考这篇[文章](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)。在 GitHub 生成好 Access Token 之后，你需要到 [AppVeyor 加密页面](https://ci.appveyor.com/tools/encrypt)把 Access Token 加密之后再替换 [Your GitHub Access Token]  
    ![](https://formulahendry.github.io/assets/img/hexo-ci/appveyor-encrypt.png)
    
4.  设置 Appveyor
    
    添加好 appveyor.yml 之后，再到 Appveyor portal 设置以下四个变量。STATIC_SITE_REPO 就是 Content Repo 的地址，TARGET_BRANCH 就是你 Content Repo 的 branch，一般默认就是 master，GIT_USER_EMAIL 和 GIT_USER_NAME 就是你 GitHub 账号的信息。  
    ![](https://formulahendry.github.io/assets/img/hexo-ci/appveyor-setting.png)
    

好了，一切大功告成！试一下`git push`你的 change 到 Source Repo，几分钟内，你的博客就自动更新了！

背后的过程如下:

1.  Git push to Source Repo
2.  –> AppVeyor CI
3.  –> Update GitHub Pages Content Repo
4.  –> Generate your Hexo blog site

最后肯定会有人问，为什么不用 Jekyll 或者 WordPress 啊？为什么选 Hexo 呢？在下一篇博客中我将详细道来，敬请期待！