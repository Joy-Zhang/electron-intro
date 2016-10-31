# 开始使用Electron
和大多数javascript的工具一样，Electron的基础同样是node.js，node.js的安装请参考互联网的其他内容。

## 安装Electron
```
npm install -g electron
```
这会在全局安装electron命令，通过electron命令就可以运行开发中的electron项目

## 创建第一个项目
毫无意外，electron的项目就是一个node.js项目，由一个package.json指定一切。以下是一个package.json例子。
```
{
  "name": "report-tool",
  "version": "0.0.1",
  "license": "MIT",
  "repository": "https://github.com/Joy-Zhang/report-tool.git",
  "main": "main.js",
  "dependencies": {
    "bootstrap": "^3.3.7",
    "domino": "^1.0.27",
    "electron": "^1.3.3",
    "font-awesome": "^4.7.0",
    "moment": "^2.15.0",
    "node-jsx": "^0.13.3",
    "nodemailer": "^2.6.0",
    "react": "^15.3.1",
    "react-dom": "^15.3.1",
    "react-router": "^3.0.0",
    "sequelize": "^3.24.1",
    "sqlite3": "^3.1.4"
  },
  "devDependencies": {
    "electron-rebuild": "^1.2.1"
  }
}
```
其中main属性指定了electron的入口文件，例子中是main.js。denpendency中最重要的一项就是electron了，他指定了electron的依赖。
当然，除了手工编写，package.json也可以同npm init来创建。
```
npm init
npm install electron --save
```

## 编写main.js
首先说一点，这儿的main.js是在package.json是在指定的，如果指定成app.js，那就编写app.js。

话不多说，先上code
```
var electron = require('electron');
var app = electron.app;
var BrowserWindow = electron.BrowserWindow;

app.on('ready', function () {
    var window = new BrowserWindow();
    window.loadURL('https://www.baidu.com');
});
app.on('window-all-closed', function () {
    app.quit();
});
```
和Electron官方的例子相比，这个例子没有使用ES2015，而且更加更加简单一些，主要原因是没有针对MacOS做特殊处理。
前3行，引入了electron，同时引入了electron的两个基本东西，app和BrowserWindow。app主要是控制整个桌面应用的生命周期，BrowserWindow则是代表一个窗口。

后边的代码做了两件事，一是在app就绪的时候创建一个窗口，并指定窗口加载百度。如果是构建桌面应用，此处应该加载一个本地html文件的url而不是线上的，之后就是开发web前端的世界了。
二是指定在所有窗口都被关闭了的时候将app退出。官方的例子针对MacOS在所有窗口关闭时，并没有退出，而是将应用隐藏在Docker上，所以这一块代码会比较复杂。

## 调试工具
做web开发，调试永远是必要的，现在的浏览器（包括新IE）都有一个调试工具，来帮助工程师查看网页上的各种问题。Electron因为使用的是libwebkit，所以直接提供了chrome的调试工具。打开的方法如下
```
app.on('ready', function () {
    var window = new BrowserWindow();
    window.loadURL('https://www.baidu.com');
    window.webContents.openDevTools();//打开开发者工具
});
```
