---
title: vueDay3
tags: vue
abbrlink: 4a793f4
date: 2021-02-05 00:00:00
categories: vue
---



### 一. 组件化开发

#### 1.1. 父子组件的访问

- children/refs
- parent/root

#### 1.2. slot的使用

- 基本使用
- 具名插槽
- 编译的作用域
- 作用域插槽

### 二. 前端模块化

#### 2.1 为什么要使用模块化

- 解决命名重复、代码不可复用性等问题
- 简单写js代码带来的问题
- 闭包引起代码不可复用
- 自己实现了简单的模块化
- AMD/CMD/CommonJS

#### 2.2 模块化规范

- CommonJS
- AMD
- CMD
- ES6的Modules

#### 2.3 模块化核心，ES6中模块化的使用

- 导出  
  - CommonJS: model.export{}
  - ES6: export{}
- 导入 
  -  CommonJS: let{} = require('')
  - ES6: import {...} from "..."

### 三. webpack

#### 3.1. 认识webpack

- 模块化打包工具

##### 3.1.1. 和grunt/gulp的区别

- grunt/gulp更强调任务处理，自动化任务管理工具
- webpack更强调模块化

#### 3.2. webpck的安装

依赖环境

```js
node -v
npm install webpack@3.6.0 -g
cd 对应目录
npm install webpack@3.6.0 --save-dev
```

#### 3.3. webpack的起步

- src(开发)
- dist->distribution(发布)

webpack命令

```js
webpack ./src/main.js ./dist/bundle.js
```
#### 3.4. webpck的配置

- 入口和出口的配置
	- webpack.config.js
  -	配置时注意绝对路径path
```js
const path = require('path')
module.exports = {
  entry: './src/main.js',
  output: {
    // path: './dist',
    // 动态获取路径
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
}
```
``` js
npm init
npm install
```
- package.jason

```js
"scripts": {
  "build": "webpack",
},
```

```js
npm run build
```
- 局部安装webpack
  - 开发时依赖
  - 运行时依赖
```js
npm install webpack@3.6.0 --save-dev
```
#### 3.5. loaderd 使用

webpack官网查询：https://webpack.docschina.org/

##### 3.5.1 安装css-loader

首先，你需要先安装 `css-loader` ：

```js
npm install --save-dev css-loader@2.0.2
npm install --save-dev style-loader@0.23.1
```

然后把 loader 引用到你 `webpack` 的配置中。如下所示：

**file.js**

```js
import css from "file.css";
```

**webpack.config.js**

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
};
```

##### 3.5.2. 安装less-loader

安装 `less` 和 `less-loader`

```js
npm install less@3.9.0 less-loader@4.1.0 --save-dev
```

将该 loader 添加到 webpack 的配置中去
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.less$/i,
        loader: [ // compiles Less to CSS
          "style-loader",
          "css-loader",
          "less-loader",
        ],
      },
    ],
  },
};
```

##### 3.5.3 安装图片文件url-loader

首先，你需要安装 `url-loader`：

```js
npm install url-loader@1.1.2 --save-dev
```

`url-loader` 功能类似于 [`file-loader`](https://webpack.docschina.org/loaders/file-loader/), 但是在文件大小（单位为字节）低于指定的限制时，可以返回一个 DataURL。

**index.js**

```js
import img from './image.png';
```

**webpack.config.js**

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif|jpeg)$/i,
        use: [
          {
            loader: 'url-loader',
            options: {
              // 当加载的图片小于limit8kb时会将图片编译成base64字符串形式
              // 当加载的图片大于limit8kb时，要使用file-loader模块进行加载
              limit: 8192,
              // 统一命名
              name: 'img/[name].[hash:8].[ext]'
            },
          },
        ],
      },
    ],
  },
};
```

然后通过你的首选方法运行 `webpack`。

##### 3.5.4. 加载file-loader模块,

```js
npm install file-loader@3.0.1 --save-dev
```

加载后由于发布到dist文件夹中，注意修改webpack中：

```js
output: {
	publicPath: 'dist/''
},
```

##### 3.5.5.  babel-loader->ES6语法处理

env:environment环境

```js
npm install --save-dev babel-loader@7 babel-core babel-preset-es2015
```
```js
module: {
  rules: [
    {
      test: /\.m?js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          // presets: ['@babel/preset-env']
          presets: ['es2015']
        }
      }
    }
  ]
}
```



#### 3.6 webpack中配置vue

下载vue的方式：

- 直接下载应用
- CDN引入
- npm安装

```js
npm install vue@2.5.21 --save
```

版本：

- runtime-only ->代码中不可以有任何template
- runtime-compiler ->代码中可以有template,因为有compiler可以用于编译template

```js
resolve: {
	alias: {
		'vue$': 'vue/dist/vue.esm.js'
	}
}
```

SPA(simple page web applocation)单页面复应用 -> 多页面时vue-router(前端路由)跳转

.vue文件封装处理

```js
npm install vue-loader@15.4.2 vue-template-compiler@2.5.21 --save-dev
```
```js
module: {
  rules: [
    {
      test: /\.vue$/,
      use: {
        loader: 'vue-loader',
      }
    }
  ]
}
```

省略扩展名：

```js
resolve: {
    extensions: ['.js', '.css', '.vue'],
}
```

#### 3.7. webpack的plugin的使用

插件-->框架扩充

- 添加版权

```js
const webpack = require('webpack')
module.exports = {
	plugins: [
    new webpack.BannerPlugin('最终版权归yzhou所有')
  ]
}
```

- 打包html的plugin
  - 自动生成一个index.html文件（可以指定模板来生成）
  - 将打包的js文件，自动通过script标签插入到body中
    - 修改webpack.config.js文件中plugin部分
      - 要删除之前在output中添加的publicPath属性

```js
npm install html-webpack-plugin@3.2.0 --save-dev
```

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
plugins: [
	new HtmlWebpackPlugin({
		template: 'index.html'
	})
]
```



- js压缩的Plugin

```js
npm install uglifyjs-webpack-plugin@1.1.1 --save-dev
```

```js
const UglifyJsPlugin = require('uglifyjs-webpack-plugin')
module.exports = {
	plugins: [
		new UglifyJsPlugin()
	]
}
```



#### 3.8. 搭建服务器

基于node.js搭建，内部使用express框架，让浏览器自动刷新，从内存读取

```js
npm install --save-dev webpack-dev-server@2.9.3
```

```js
module.exports = {
	devServer: {
		contentBase: './dist',
		inline: true
	}
}
```

package.jason中添加：

```js
"scripts": {
	"dev": "webpack-dev-server --open"
}
```

#### 3.9. webpack配置分离

```js 
npm install webpack-merge@4.1.5 --save-dev
```



### 四. Vue CLI

cli->command-line interface

#### 4.1. 认识Vue CLI

- 脚手架是什么东西
- CLI依赖webpack，node，npm
- 安装CLI3->拉取CLI2模块

开发大型项目时，需要考虑代码目录结构，项目结构和部署、热加载、代码单元测试等事情，手动完成效率低

- 快速搭建vue开发环境
- 生成对应webpack配置

#### 4.2.使用前提 

- Node
  - C++
  - V8引擎–跳过字节码直接编译成二进制代码

```js
node -v
npm -v
```

npm: Node Package Manager

- Nodejs包管理和分发工具

```js
npm install -g cnmp --registry=https://registry.npm.taobao.org
```

- webpack

```js
npm install webpack -g
```

#### 4.3. 使用和安装

```js
npm install -g @vue/cli
C:\Users\yzhou>vue --version
@vue/cli 4.5.12
vue create my-project
```

拉取Vue CLI2的模板：

```js
npm install -g @vue/cli-init
vue init webpack my-project
```



#### 4.4 CLI2初始化项目的过程

#### 4.5 CLI2生产的目录结构解析

ES(js)-Lint

e2e-> end to end(端到端测试)-> selenium