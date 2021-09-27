---
title: Puppeteer
date: 2021.9.27
tags: Puppeteer
categories: Puppeteer
---



# Puppeteer

[![Linux Build Status](https://img.shields.io/travis/GoogleChrome/puppeteer/master.svg)](https://travis-ci.org/GoogleChrome/puppeteer) [![Windows Build Status](https://img.shields.io/appveyor/ci/aslushnikov/puppeteer/master.svg?logo=appveyor)](https://ci.appveyor.com/project/aslushnikov/puppeteer/branch/master) [![Build Status](https://api.cirrus-ci.com/github/GoogleChrome/puppeteer.svg)](https://cirrus-ci.com/github/GoogleChrome/puppeteer) [![NPM puppeteer package](https://img.shields.io/npm/v/puppeteer.svg)](https://npmjs.org/package/puppeteer)

###### [API](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md) | [FAQ](https://zhaoqize.github.io/puppeteer-api-zh_CN/#faq) | [Contributing](https://github.com/GoogleChrome/puppeteer/blob/master/CONTRIBUTING.md) | [Troubleshooting](https://github.com/GoogleChrome/puppeteer/blob/master/docs/troubleshooting.md)

> Puppeteer 是一个 Node 库，它提供了一个高级 API 来通过 [DevTools](https://zhaoqize.github.io/puppeteer-api-zh_CN/(https://chromedevtools.github.io/devtools-protocol/)) 协议控制 Chromium 或 Chrome。Puppeteer 默认以 [headless](https://developers.google.com/web/updates/2017/04/headless-chrome) 模式运行，但是可以通过修改配置文件运行“有头”模式。

###### 能做什么?

你可以在浏览器中手动执行的绝大多数操作都可以使用 Puppeteer 来完成！ 下面是一些示例：

- 生成页面 PDF。
- 抓取 SPA（单页应用）并生成预渲染内容（即“SSR”（服务器端渲染））。
- 自动提交表单，进行 UI 测试，键盘输入等。
- 创建一个时时更新的自动化测试环境。 使用最新的 JavaScript 和浏览器功能直接在最新版本的Chrome中执行测试。
- 捕获网站的 [timeline trace](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference)，用来帮助分析性能问题。
- 测试浏览器扩展。

演示地址: https://try-puppeteer.appspot.com/

## 开始使用

### 安装

在项目中使用 Puppeteer：

```bash
npm i puppeteer
# or "yarn add puppeteer"
```

Note: 当你安装 Puppeteer 时，它会下载最新版本的Chromium（~170MB Mac，~282MB Linux，~280MB Win），以保证可以使用 API。 如果想要跳过下载，请阅读[环境变量](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md#environment-variables)。

### puppeteer-core

自 1.7.0 版本以来，我们都会发布一个 [`puppeteer-core`](https://www.npmjs.com/package/puppeteer-core) 包，这个包默认不会下载 Chromium。

```bash
npm i puppeteer-core
# or "yarn add puppeteer-core"
```

`puppeteer-core` 是一个的轻量级的 Puppeteer 版本，用于启动现有浏览器安装或连接到远程安装。

具体见 [puppeteer vs puppeteer-core](https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#puppeteer-vs-puppeteer-core).

### 使用

Note: Puppeteer 至少需要 Node v6.4.0，下面的示例使用 async / await，它们仅在 Node v7.6.0 或更高版本中被支持。

Puppeteer 使用起来和其他测试框架类似。你需要创建一个 `Browser` 实例，打开页面，然后使用 [Puppeteer 的 API](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md#)。

**Example** - 跳转到 https://example.com 并保存截图至 *example.png*:

文件为 **example.js**

```js
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://example.com');
  await page.screenshot({path: 'example.png'});

  await browser.close();
})();
```

在命令行中执行

```bash
node example.js
```

Puppeteer 初始化的屏幕大小默认为 800px x 600px。但是这个尺寸可以通过 [`Page.setViewport()`](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md#pagesetviewportviewport) 设置。

**Example** - 创建一个 PDF。

文件为 **hn.js**

```js
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://news.ycombinator.com', {waitUntil: 'networkidle2'});
  await page.pdf({path: 'hn.pdf', format: 'A4'});

  await browser.close();
})();
```

在命令行中执行

```bash
node hn.js
```

查看 [`Page.pdf()`](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md#pagepdfoptions) 了解跟多内容。

**Example** - 在页面中执行脚本

文件为 **get-dimensions.js**

```js
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://example.com');

  // Get the "viewport" of the page, as reported by the page.
  const dimensions = await page.evaluate(() => {
    return {
      width: document.documentElement.clientWidth,
      height: document.documentElement.clientHeight,
      deviceScaleFactor: window.devicePixelRatio
    };
  });

  console.log('Dimensions:', dimensions);

  await browser.close();
})();
```

在命令行中执行

```bash
node get-dimensions.js
```

查看 [`Page.evaluate()`](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md#pageevaluatepagefunction-args) 了解更多相关内容，该方法有点类似于 `evaluateOnNewDocument` and `exposeFunction`。

## 默认设置

**1. 使用无头模式**

Puppeteer 运行 Chromium 的[headless mode](https://developers.google.com/web/updates/2017/04/headless-chrome)。如果想要使用完全版本的 Chromium，设置 ['headless' option](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md#puppeteerlaunchoptions) 即可。

```js
const browser = await puppeteer.launch({headless: false}); // default is true
```

**2. 运行绑定的 Chromium 版本**

默认情况下，Puppeteer 下载并使用特定版本的 Chromium 以及其 API 保证开箱即用。 如果要将 Puppeteer 与不同版本的 Chrome 或 Chromium 一起使用，在创建`Browser`实例时传入 Chromium 可执行文件的路径即可：

```js
const browser = await puppeteer.launch({executablePath: '/path/to/Chrome'});
```

具体见：[`Puppeteer.launch()`](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md#puppeteerlaunchoptions)

看[`这篇文章`](https://www.howtogeek.com/202825/what’s-the-difference-between-chromium-and-chrome/)了解 Chromium 与 Chrome 的不同。[`这篇文章`](https://chromium.googlesource.com/chromium/src/+/master/docs/chromium_browser_vs_google_chrome.md) 介绍了一些 Linux 用户在使用上的区别。

**3. 创建用户配置文件**

Puppeteer 会创建自己的 Chromium 用户配置文件，**它会在每次运行时清理**。

## 资源

- [API Documentation](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md)
- [Examples](https://github.com/GoogleChrome/puppeteer/tree/master/examples/)
- [Community list of Puppeteer resources](https://github.com/transitive-bullshit/awesome-puppeteer)

## Debugging tips

1. Turn off headless mode - sometimes it's useful to see what the browser is displaying. Instead of launching in headless mode, launch a full version of the browser using `headless: false`:

   ```
    const browser = await puppeteer.launch({headless: false});
   ```

2. Slow it down - the `slowMo` option slows down Puppeteer operations by the specified amount of milliseconds. It's another way to help see what's going on.

   ```
    const browser = await puppeteer.launch({
      headless: false,
      slowMo: 250 // slow down by 250ms
    });
   ```

3. Capture console output - You can listen for the `console` event. This is also handy when debugging code in `page.evaluate()`:

   ```
    page.on('console', msg => console.log('PAGE LOG:', msg.text()));
   
    await page.evaluate(() => console.log(`url is ${location.href}`));
   ```

4. Stop test execution and use a debugger in browser

- Use `{devtools: true}` when launching Puppeteer:

  `const browser = await puppeteer.launch({devtools: true});`

- Change default test timeout:

  jest: `jest.setTimeout(100000);`

  jasmine: `jasmine.DEFAULT_TIMEOUT_INTERVAL = 100000;`

  mocha: `this.timeout(100000);` (don't forget to change test to use [function and not '=>'](https://stackoverflow.com/a/23492442))

- Add an evaluate statement with `debugger` inside / add `debugger` to an existing evaluate statement:

  `await page.evaluate(() => {debugger;});`

  The test will now stop executing in the above evaluate statement, and chromium will stop in debug mode.

1. Enable verbose logging - internal DevTools protocol traffic will be logged via the [`debug`](https://github.com/visionmedia/debug) module under the `puppeteer` namespace.

   ```
    # Basic verbose logging
    env DEBUG="puppeteer:*" node script.js
   
    # Debug output can be enabled/disabled by namespace
    env DEBUG="puppeteer:protocol" node script.js # protocol connection messages
    env DEBUG="puppeteer:session" node script.js # protocol session messages (protocol messages to targets)
   
    # Protocol traffic can be rather noisy. This example filters out all Network domain messages
    env DEBUG="puppeteer:session" env DEBUG_COLORS=true node script.js 2>&1 | grep -v '"Network'
   ```

2. Debug your Puppeteer (node) code easily, using [ndb](https://github.com/GoogleChromeLabs/ndb)

- `npm install -g ndb` (or even better, use [npx](https://github.com/zkat/npx)!)

- add a `debugger` to your Puppeteer (node) code

- add `ndb` (or `npx ndb`) before your test command. For example:

  `ndb jest` or `ndb mocha` (or `npx ndb jest` / `npx ndb mocha`)

- debug your test inside chromium like a boss!



https://zhaoqize.github.io/puppeteer-api-zh_CN/#/





# 1.puppeteer简介

puppeteer是一个node库，是Google chrome团队官方的无界面（headless）chrome工具。它提供了一组用来操纵Chrome的 API，允许通过 JS代码操纵Chrome浏览器，完成数据爬取、Web程序自动测试等任务。Puppeteer项目在[GitHub](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2FGoogleChrome%2Fpuppeteer)上开源。

## [puppeteer核心功能]()

1.利用网页生成PDF、图片

2.爬取SPA应用，并生成预渲染内容（“SSR”服务端渲染）

3.从网站抓取内容

4.自动化表单提交、UI测试、键盘输入

5.帮助创建最新的自动化测试环境（chrome），可以直接运行测试用例

6.捕获站点的时间线，以便追踪网站，帮助分析网站性能问题

## [Chrome Headless环境要求]()

Puppeteer要求node版本不低于v6.4.0，但是async/await只在Node v7.6.0或更高的版本支持。

需要最近版本的Chromium浏览器

## 2.[环境安装]()

安装[node 8.+](https://link.jianshu.com?t=https%3A%2F%2Flink.juejin.im%2F%3Ftarget%3Dhttps%3A%2F%2Fnodejs.org)

若已经安装了node，cmd中输入node -v 查看node的版本。若要更新node到最新版本，只需要下载安装最新版本的node即可（安装的时候要注意，选择路径要与之前安装node的路径相同<查看之前安装的node的路径：where node>，这样就相当于更新了）。

## [初始化项目]()

## [新建一个文件夹作为工作目录]()

我这里新建文件夹名称为myPuppeteer，进入该文件夹，在此处打开命令行窗口cmd，输入npm init 初始化npm，一步步回车即可。

## [安装Puppeteer]()

命令行输入以下命令：npm i puppeteer --save

## 3.[页面截图小案例]()

初始化步骤完成后，node_modules放在根目录下，依次打开以下文件夹：node_modules—puppeteer—examples.

进入到examples文件夹下后，里面有很多js文件，这些一般都是一些小例子。以screenshot.js为例，看一个页面截图的例子。 

我们以记事本方式打开该文件，主要js代码如下：

'use strict';constpuppeteer =require('puppeteer');//引入puppeteer库.

(async() => {

​         constbrowser = await puppeteer.launch();//用指定选项启动一个Chromium浏览器实例。

​         constpage = await browser.newPage();//创建一个页面.

​         await page.goto('http://example.com');//到指定页面的网址.

​         await page.screenshot({path:'example.png'});//截图并保存到当前路径，名称为example.png.

​         await browser.close();//关闭已打开的页面，browser不能再使用。

​        })();

方式1.在pycharm中新建一个file，命名为test.js，将上面的代码拷贝到test.js中并保存，在terminal中输入node screenshot.js//运行名为screenshot.js的文件.

方式2.还是在当前文件夹（examples）下，此处打开命令行窗口，输入：node screenshot.js//运行名为screenshot.js的文件.

运行完成后，就会在当前目录下看到名为example.png的图片，该图片即运行该js后截的图片。

# 4、Puppeteer 实战

了解 API 之后我们就可以来一些实战了，在此之前，我们先了解一下 Puppeteer 的设计原理，简单来说 Puppeteer 跟 webdriver 以及 PhantomJS 最大的 的不同就是它是站在用户浏览的角度，而 webdriver 和 PhantomJS 最初设计就是用来做自动化测试的，所以它是站在机器浏览的角度来设计的，所以它们 使用的是不同的设计哲学。举个栗子，加入我需要打开京东的首页并进行一次产品搜索，分别看看使用 Puppeteer 和 webdriver 的实现流程：

**Puppeteer 的实现流程：**

打开京东首页

将光标 focus 到搜索输入框

键盘点击输入文字

点击搜索按钮

**webdriver 的实现流程：**

打开京东首页

找到输入框的 input 元素

设置 input 的值为要搜索文字

触发搜索按钮的单机事件

个人感觉 Puppeteer 设计哲学更符合任何的操作习惯，更自然一些。

下面我们就用一个简单的需求实现来进行 Puppeteer 的入门学习。这个简单的需求就是：

在京东商城抓取10个手机商品，并把商品的详情页截图。

首先我们来梳理一下操作流程

打开京东首页

输入“手机”关键字并搜索

获取前10个商品的 A 标签，并获取 href 属性值，获取商品详情链接

分别打开10个商品的详情页，截取网页图片

要实现上面的功能需要用到查找元素，获取属性，键盘事件等，那接下来我们就一个一个的讲解一下。

4.1 获取元素

Page 对象提供了2个 API 来获取页面元素

(1). Page.$(selector) 获取单个元素，底层是调用的是 document.querySelector() , 所以选择器的 selector 格式遵循 [css 选择器规范](https://link.jianshu.com?t=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FCSS%2FCSS_Selectors)

letinputElement=awaitpage.$("#search",input=>input);//下面写法等价letinputElement=awaitpage.$('#search');

(2). Page.$$(selector) 获取一组元素，底层调用的是 document.querySelectorAll(). 返回 Promise(Array(ElemetHandle)) 元素数组.

constlinks=awaitpage.$$("a");//下面写法等价constlinks=awaitpage.$$("a",links=>links);

最终返回的都是 ElemetHandle 对象

4.2 获取元素属性

Puppeteer 获取元素属性跟我们平时写前段的js的逻辑有点不一样，按照通常的逻辑，应该是现获取元素，然后在获取元素的属性。但是上面我们知道 获取元素的 API 最终返回的都是 ElemetHandle 对象，而你去查看 ElemetHandle 的 API 你会发现，它并没有获取元素属性的 API.

事实上 Puppeteer 专门提供了一套获取属性的 API， Page.$eval() 和 Page.$$eval()

(1). Page.$$eval(selector, pageFunction[, …args]), 获取单个元素的属性，这里的选择器 selector 跟上面 Page.$(selector) 是一样的。

constvalue=awaitpage.$eval('input[name=search]',input=>input.value);consthref=awaitpage.$eval('#a", ele => ele.href);

const content = await page.$eval('.content', ele => ele.outerHTML);

4.3 执行自定义的 JS 脚本

Puppeteer 的 Page 对象提供了一系列 evaluate 方法，你可以通过他们来执行一些自定义的 js 代码，主要提供了下面三个 API

(1). page.evaluate(pageFunction, …args) 返回一个可序列化的普通对象，pageFunction 表示要在页面执行的函数， args 表示传入给 pageFunction 的参数， 下面的 pageFunction 和 args 表示同样的意思。

constresult=awaitpage.evaluate(() =>{returnPromise.resolve(8*7);});console.log(result);//prints"56"

这个方法很有用，比如我们在获取页面的截图的时候，默认是只截图当前浏览器窗口的尺寸大小，默认值是800x600，那如果我们需要获取整个网页的完整 截图是没办法办到的。Page.screenshot() 方法提供了可以设置截图区域大小的参数，那么我们只要在页面加载完了之后获取页面的宽度和高度就可以解决 这个问题了。

(async()=>{constbrowser=awaitpuppeteer.launch({headless:true});constpage=awaitbrowser.newPage();awaitpage.goto('https://jr.dayi35.com');awaitpage.setViewport({width:1920,height:1080});constdocumentSize=awaitpage.evaluate(()=>{return{width:document.documentElement.clientWidth,height:document.body.clientHeight,}})awaitpage.screenshot({path:"example.png",clip:{x:0,y:0,width:1920,height:documentSize.height}});awaitbrowser.close();})();

(2). Page.evaluateHandle(pageFunction, …args) 在 Page 上下文执行一个 pageFunction, 返回 JSHandle 实体

constaWindowHandle=awaitpage.evaluateHandle(()=>Promise.resolve(window));aWindowHandle;// Handle for the window object. constaHandle=awaitpage.evaluateHandle('document');// Handle for the 'document'.

从上面的代码可以看出，page.evaluateHandle() 方法也是通过 Promise.resolve 方法直接把 Promise 的最终处理结果返回， 只不过把最后返回的对象封装成了 JSHandle 对象。本质上跟 evaluate 没有什么区别。

下面这段代码实现获取页面的动态（包括js动态插入的元素） HTML 代码.

constaHandle=awaitpage.evaluateHandle(()=>document.body);constresultHandle=awaitpage.evaluateHandle(body=>body.innerHTML,aHandle);console.log(awaitresultHandle.jsonValue());awaitresultHandle.dispose();

(3). page.evaluateOnNewDocument(pageFunction, …args), 在文档页面载入前调用 pageFunction, 如果页面中有 iframe 或者 frame, 则函数调用 的上下文环境将变成子页面的，即iframe 或者 frame, 由于是在页面加载前调用，这个函数一般是用来初始化 javascript 环境的，比如重置或者 初始化一些全局变量。

4.4 Page.exposeFunction

除此上面三个 API 之外，还有一类似的非常有用的 API， 那就是 **Page.exposeFunction**，这个 API 用来在页面注册全局函数，非常有用：

因为有时候需要在页面处理一些操作的时候需要用到一些函数，虽然可以通过 Page.evaluate() API 在页面定义函数，比如：

constdocSize=awaitpage.evaluate(()=>{function getPageSize() {return{width:document.documentElement.clientWidth,height:document.body.clientHeight,}}returngetPageSize();});

但是这样的函数不是全局的，需要在每个 evaluate 中去重新定义，无法做到代码复用，在一个就是 nodejs 有很多工具包可以很轻松的实现很复杂的功能 比如要实现 md5 加密函数，这个用纯 js 去实现就不太方便了，而用 nodejs 却是几行代码的事情。

下面代码实现给 Page 上下文的 window 对象添加 md5 函数：

constpuppeteer=require('puppeteer');constcrypto=require('crypto');puppeteer.launch().then(asyncbrowser=>{constpage=awaitbrowser.newPage();page.on('console',msg=>console.log(msg.text));awaitpage.exposeFunction('md5',text=>crypto.createHash('md5').update(text).digest('hex'));awaitpage.evaluate(async()=>{// use window.md5 to compute hashesconstmyString='PUPPETEER';constmyHash=awaitwindow.md5(myString);console.log(`md5 of ${myString} is ${myHash}`);});awaitbrowser.close();});

可以看出，Page.exposeFunction API 使用起来是很方便的，也非常有用，在比如给 window 对象注册 readfile 全局函数：

constpuppeteer=require('puppeteer');constfs=require('fs');puppeteer.launch().then(asyncbrowser=>{constpage=awaitbrowser.newPage();page.on('console',msg=>console.log(msg.text));awaitpage.exposeFunction('readfile',asyncfilePath=>{returnnewPromise((resolve,reject)=>{fs.readFile(filePath,'utf8',(err,text)=>{if(err)reject(err);elseresolve(text);});});});awaitpage.evaluate(async()=>{// use window.readfile to read contents of a fileconstcontent=awaitwindow.readfile('/etc/hosts');console.log(content);});awaitbrowser.close();});

5、Page.emulate 修改模拟器(客户端)运行配置

Puppeteer 提供了一些 API 供我们修改浏览器终端的配置

Page.setViewport() 修改浏览器视窗大小

Page.setUserAgent() 设置浏览器的 UserAgent 信息

Page.emulateMedia() 更改页面的CSS媒体类型，用于进行模拟媒体仿真。 可选值为 “screen”, “print”, “null”, 如果设置为 null 则表示禁用媒体仿真。

Page.emulate() 模拟设备，参数设备对象，比如 iPhone, Mac, Android 等

page.setViewport({width:1920,height:1080});//设置视窗大小为 1920x1080page.setUserAgent('Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.90 Safari/537.36');page.emulateMedia('print');//设置打印机媒体样式

除此之外我们还可以模拟非 PC 机设备, 比如下面这段代码模拟 iPhone 6 访问google：

constpuppeteer=require('puppeteer');constdevices=require('puppeteer/DeviceDescriptors');constiPhone=devices['iPhone 6'];puppeteer.launch().then(asyncbrowser=>{constpage=awaitbrowser.newPage();awaitpage.emulate(iPhone);awaitpage.goto('https://www.google.com');// other actions...awaitbrowser.close();});

Puppeteer 支持很多设备模拟仿真，比如Galaxy, iPhone, IPad 等，想要知道详细设备支持，请戳这里 [DeviceDescriptors.js.](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2FGoogleChrome%2Fpuppeteer%2Fblob%2Fmaster%2FDeviceDescriptors.js)

6、键盘和鼠标

键盘和鼠标的API比较简单，键盘的几个API如下：

keyboard.down(key[, options]) 触发 keydown 事件

keyboard.press(key[, options]) 按下某个键，key 表示键的名称，比如 ‘ArrowLeft’ 向左键，详细的键名映射请[戳这里](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2FGoogleChrome%2Fpuppeteer%2Fblob%2Fmaster%2Flib%2FUSKeyboardLayout.js)

keyboard.sendCharacter(char) 输入一个字符

keyboard.type(text, options) 输入一个字符串

keyboard.up(key) 触发 keyup 事件

page.keyboard.press("Shift");//按下 Shift 键page.keyboard.sendCharacter('嗨');page.keyboard.type('Hello');// 一次输入完成page.keyboard.type('World',{delay:100});// 像用户一样慢慢输入

鼠标操作：

mouse.click(x, y, [options]) 移动鼠标指针到指定的位置，然后按下鼠标，这个其实 mouse.move 和 mouse.down 或 mouse.up 的快捷操作

mouse.down([options]) 触发 mousedown 事件，options 可配置:

options.button 按下了哪个键，可选值为[left, right, middle], 默认是 left, 表示鼠标左键

options.clickCount 按下的次数，单击，双击或者其他次数

delay 按键延时时间

mouse.move(x, y, [options]) 移动鼠标到指定位置， options.steps 表示移动的步长

mouse.up([options]) 触发 mouseup 事件

7、另外几个有用的 API

Puppeteer 还提供几个非常有用的 API， 比如：

7.1 Page.waitFor 系列 API

page.waitFor(selectorOrFunctionOrTimeout[, options[, …args]]) 下面三个的综合 API

page.waitForFunction(pageFunction[, options[, …args]]) 等待 pageFunction 执行完成之后

page.waitForNavigation(options) 等待页面基本元素加载完之后，比如同步的 HTML, CSS, JS 等代码

page.waitForSelector(selector[, options]) 等待某个选择器的元素加载之后，这个元素可以是异步加载的，这个 API 非常有用，你懂的。

比如我想获取某个通过 js 异步加载的元素，那么直接获取肯定是获取不到的。这个时候就可以使用 page.waitForSelector 来解决：

awaitpage.waitForSelector('.gl-item');//等待元素加载之后，否则获取不到异步加载的元素constlinks=awaitpage.$$eval('.gl-item > .gl-i-wrap > .p-img > a',links=>{returnlinks.map(a=>{return{href:a.href.trim(),name:a.title}});});

其实上面的代码就可以解决我们最上面的需求，抓取京东的产品，因为是异步加载的，所以使用这种方式。

7.2 page.getMetrics()

通过 page.getMetrics() 可以得到一些页面性能数据， 捕获网站的时间线跟踪，以帮助诊断性能问题。

Timestamp 度量标准采样的时间戳

Documents 页面文档数

Frames 页面 frame 数

JSEventListeners 页面内事件监听器数

Nodes 页面 DOM 节点数

LayoutCount 页面布局总数

RecalcStyleCount 样式重算数

LayoutDuration 所有页面布局的合并持续时间

RecalcStyleDuration 所有页面样式重新计算的组合持续时间。

ScriptDuration 所有脚本执行的持续时间

TaskDuration 所有浏览器任务时长

JSHeapUsedSize JavaScript 占用堆大小

JSHeapTotalSize JavaScript 堆总量

8、总结和源码

本文通过一个实际需求来学习了 Puppeteer 的一些基本的常用的 API， API 的版本是 v0.13.0-alpha. 最新邦本的 API 请参考[Puppeteer 官方API](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2FGoogleChrome%2Fpuppeteer%2Fblob%2Fmaster%2Fdocs%2Fapi.md).

总的来说，Puppeteer 真是一款不错的 headless 工具，操作简单，功能强大。用来做UI自动化测试，和一些小工具都是很不错的。

下面贴上我们开始的需求实现源码，仅供参考：

//延时函数

function sleep(delay) {

return new Promise((resolve, reject) => {

setTimeout(() => {

try {

resolve(1)

}catch (e) {

reject(0)

}

}, delay)

})

}

const puppeteer = require('puppeteer');

puppeteer.launch({

ignoreHTTPSErrors:true,

headless:false,slowMo:250,

timeout:0}).then(async browser => {

let page = await browser.newPage();

await page.setJavaScriptEnabled(true);

await page.setViewport({width:1920, height:1080});

await page.goto("https://www.jd.com/");

const searchInput = await page.$("#key");

await searchInput.focus();//定位到搜索框

  await page.keyboard.type("手机");

const searchBtn = await page.$(".button");

await searchBtn.click();

await page.waitForSelector('.gl-item');//等待元素加载之后，否则获取不异步加载的元素

  const links = await page.$$eval('.gl-item > .gl-i-wrap > .p-img > a', links => {

return links.map(a => {

return {

href: a.href.trim(),

title: a.title

}

});

});

//page.close();

  const aTags = links.splice(0,11);

for (var i =1; i < aTags.length; i++) {

page = await browser.newPage()

page.setJavaScriptEnabled(true);

await page.setViewport({width:1920, height:1080});

var a = aTags[i];

await page.goto(a.href, {timeout:0});//防止页面太长，加载超时

//注入代码，慢慢把滚动条滑到最底部，保证所有的元素被全部加载

   let scrollEnable =true;

let scrollStep =1000;//每次滚动的步长

   while (scrollEnable) {

scrollEnable = await page.evaluate((scrollStep) => {

let scrollTop = document.scrollingElement.scrollTop;

document.scrollingElement.scrollTop = scrollTop + scrollStep;

return document.body.clientHeight > scrollTop +1080 ?true :false

​     }, scrollStep);

await sleep(100);

}

//await page.waitForSelector("#footer-2017", {timeout:0}); //判断是否到达底部了

//  let filename = "images/items-"+i+".png";

//这里有个Puppeteer的bug一直没有解决，发现截图的高度最大只能是16384px， 超出部分被截掉了。

//  await page.screenshot({path:filename, fullPage:true});

   await page.screenshot({path:"images/items-"+i+".png"});

page.close();

}

browser.close();

});



作者：伊人风采_690d
链接：https://www.jianshu.com/p/78fe20b65a00
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。