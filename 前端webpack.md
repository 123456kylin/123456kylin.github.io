webpack前端模块打包工具
## 安装

首先要安装 Node.js， Node.js 自带了软件包管理器 npm，Webpack 需要 Node.js v0.6 以上支持，建议使用最新版 Node.js。

用 npm 安装 Webpack：

```
$ npm install webpack -g
```

此时 Webpack 已经安装到了全局环境下，可以通过命令行 webpack -h 试试。

通常我们会将 Webpack 安装到项目的依赖中，这样就可以使用项目本地版本的 Webpack。

```
# 进入项目目录
# 确定已经有 package.json，没有就通过 npm init 创建
# 安装 webpack 依赖 $ npm install webpack --save-dev
```

Webpack 目前有两个主版本，一个是在 master 主干的稳定版，一个是在 webpack-2 分支的测试版，测试版拥有一些实验性功能并且和稳定版不兼容，在正式项目中应该使用稳定版。

```
# 查看 webpack 版本信息 $ npm info webpack
# 安装指定版本的 webpack $ npm install webpack@1.12.x --save-dev
```

如果需要使用 Webpack 开发工具，要单独安装：

```
$ npm install webpack-dev-server --save-dev
```

## 使用

首先创建一个静态页面 index.html 和一个 JS 入口文件 entry.js：

```
<!-- index.html --> 
<html> 
<head> 
<meta charset="utf-8"> 
</head> 
<body> 
<script src="bundle.js"></script>
</body>
</html> 
```

```
// entry.js
 document.write('It works.') 
```

然后编译 entry.js 并打包到 bundle.js：
```
$ webpack entry.js bundle.js
```
打包过程会显示日志：
```
Hash: e964f90ec65eb2c29bb9 
Version: webpack 1.12.2 
Time: 54ms 
Asset     Size Chunks         Chunk Names 
bundle.js 1.42 kB     0 [emitted] main 
[0] ./entry.js 27 bytes {0} [built]
```
用浏览器打开 index.html 将会看到 It works. 。

接下来添加一个模块 module.js 并修改入口 entry.js：
```
// module.js 
module.exports = 'It works from module.js.'
```
```
// entry.js 
document.write('It works.') 
document.write(require('./module.js)) // 添加模块
```

重新打包 webpack entry.js bundle.js 后刷新页面看到变化 It works.It works from module.js.
```
Hash: 279c7601d5d08396e751 
Version: webpack 1.12.2 
Time: 63ms 
    Asset     Size Chunks         Chunk Names 
bundle.js   1.57 kB    0 [emitted] main 
    [0] ./entry.js 66 bytes {0} [built] 
    [1] ./module.js 43 bytes {0} [built]
```
Webpack 会分析入口文件，解析包含依赖关系的各个文件。这些文件（模块）都打包到 bundle.js 。Webpack 会给每个模块分配一个唯一的 id 并通过这个 id 索引和访问模块。在页面启动时，会先执行 entry.js 中的代码，其它模块会在运行 require 的时候再执行。


## Loader
Webpack 本身只能处理 JavaScript 模块，如果要处理其他类型的文件，就需要使用 loader 进行转换。

Loader 可以理解为是模块和资源的转换器，它本身是一个函数，接受源文件作为参数，返回转换的结果。这样，我们就可以通过 require 来加载任何类型的模块或文件，比如 CoffeeScript、 JSX、 LESS 或图片。

先来看看 loader 有哪些特性？

    Loader 可以通过管道方式链式调用，每个 loader 可以把资源转换成任意格式并传递给下一个 loader ，但是最后一个 loader 必须返回 JavaScript。
    Loader 可以同步或异步执行。
    Loader 运行在 node.js 环境中，所以可以做任何可能的事情。
    Loader 可以接受参数，以此来传递配置项给 loader。
    Loader 可以通过文件扩展名（或正则表达式）绑定给不同类型的文件。
    Loader 可以通过 npm 发布和安装。
    Loader除了通过 package.json 的 main 指定，通常的模块也可以导出一个 loader 来使用。
    Loader 可以访问配置。
    插件可以让 loader 拥有更多特性。
    Loader 可以分发出附加的任意文件。

Loader 本身也是运行在 node.js 环境中的 JavaScript 模块，它通常会返回一个函数。大多数情况下，我们通过 npm 来管理 loader，但是你也可以在项目中自己写 loader 模块。

按照惯例，而非必须，loader 一般以 xxx-loader 的方式命名，xxx 代表了这个 loader 要做的转换功能，比如 json-loader。

在引用 loader 的时候可以使用全名 json-loader，或者使用短名 json。这个命名规则和搜索优先级顺序在 webpack 的 resolveLoader.moduleTemplates api 中定义。
```
Default: ["*-webpack-loader", "*-web-loader", "*-loader", "*"]
```
Loader 可以在 require() 引用模块的时候添加，也可以在 webpack 全局配置中进行绑定，还可以通过命令行的方式使用。

接上一节的例子，我们要在页面中引入一个 CSS 文件 style.css，首页将 style.css 也看成是一个模块，然后用 css-loader 来读取它，再用 style-loader 把它插入到页面中。
```
/* style.css */ 
body { background: yellow; }
```
修改 entry.js：
```
require("!style!css!./style.css") // 载入 style.css 
document.write('It works.') 
document.write(require('./module.js'))
```
安装 loader：
```
npm install css-loader style-loader
```
重新编译打包，刷新页面，就可以看到黄色的页面背景了。

如果每次 require CSS 文件的时候都要写 loader 前缀，是一件很繁琐的事情。我们可以根据模块类型（扩展名）来自动绑定需要的 loader。

将 entry.js 中的 require("!style!css!./style.css") 修改为 require("./style.css") ，然后执行：
```
$ webpack entry.js bundle.js --module-bind 'css=style!css'

# 有些环境下可能需要使用双引号 
$ webpack entry.js bundle.js --module-bind "css=style!css"
```
显然，这两种使用 loader 的方式，效果是一样的。

## 配置文件
Webpack 在执行的时候，除了在命令行传入参数，还可以通过指定的配置文件来执行。默认情况下，会搜索当前目录的 webpack.config.js 文件，这个文件是一个 node.js 模块，返回一个 json 格式的配置信息对象，或者通过 --config 选项来指定配置文件。

继续我们的案例，在根目录创建 package.json 来添加 webpack 需要的依赖：
```
{ 
    "name": "webpack-example", 
    "version": "1.0.0", 
    "description": "A simple webpack example.", 
    "main": "bundle.js", 
    "scripts": { 
        "test": "echo \"Error: no test specified\" && exit 1"
     }, 
    "keywords": [    
        "webpack" 
    ], 
    "author": "zhaoda", 
    "license": "MIT", 
    "devDependencies": { 
        "css-loader": "^0.21.0", 
        "style-loader": "^0.13.0", 
        "webpack": "^1.12.2" 
    } 
}
```
```
# 如果没有写入权限，请尝试如下代码更改权限 
chflags -R nouchg . 
sudo chmod 775 package.json
```
别忘了运行 npm install。

然后创建一个配置文件 webpack.config.js：
```
var webpack = require('webpack') 

module.exports = { 
    entry: './entry.js', 
    output: { 
        path: __dirname, 
        filename: 'bundle.js' 
    }, 
    module: { 
    loaders: [ 
        {test: /\.css$/, loader: 'style!css'} 
    ] 
    } 
}
```
同时简化 entry.js 中的 style.css 加载方式：
```
require('./style.css')
```
最后运行 webpack，可以看到 webpack 通过配置文件执行的结果和上一章节通过命令行 webpack entry.js bundle.js --module-bind 'css=style!css' 执行的结果是一样的。

## 插件
插件可以完成更多 loader 不能完成的功能。

插件的使用一般是在 webpack 的配置信息 plugins 选项中指定。

Webpack 本身内置了一些常用的插件，还可以通过 npm 安装第三方插件。

接下来，我们利用一个最简单的 BannerPlugin 内置插件来实践插件的配置和运行，这个插件的作用是给输出的文件头部添加注释信息。

修改 webpack.config.js，添加 plugins：
```
var webpack = require('webpack') 
module.exports = { 
    entry: './entry.js', 
    output: { 
        path: __dirname, 
        filename: 'bundle.js' 
    }, 
    module: { 
        loaders: [ 
            {test: /\.css$/, loader: 'style!css'} 
        ] 
    }, 
    plugins: [ 
        new webpack.BannerPlugin('This file is created by zhaoda') 
    ] 
}
```
然后运行 webpack，打开 bundle.js，可以看到文件头部出现了我们指定的注释信息：
```
/*! This file is created by zhaoda */
/******/ (function(modules) { // webpackBootstrap 
/******/ // The module cache 
/******/ var installedModules = {}; 
// 后面代码省略

```

## 开发环境
当项目逐渐变大，webpack 的编译时间会变长，可以通过参数让编译的输出内容带有进度和颜色。
```
$ webpack --progress --colors 
```
如果不想每次修改模块后都重新编译，那么可以启动监听模式。开启监听模式后，没有变化的模块会在编译后缓存到内存中，而不会每次都被重新编译，所以监听模式的整体速度是很快的。
```
$ webpack --progress --colors --watch
```
当然，使用 webpack-dev-server 开发服务是一个更好的选择。它将在 localhost:8080 启动一个 express 静态资源 web 服务器，并且会以监听模式自动运行 webpack，在浏览器打开 http://localhost:8080/ 或http://localhost:8080/webpack-dev-server/ 可以浏览项目中的页面和编译后的资源输出，并且通过一个 socket.io 服务实时监听它们的变化并自动刷新页面。
```
# 安装 
$ npm install webpack-dev-server -g 

# 运行 
$ webpack-dev-server --progress --colors
```


