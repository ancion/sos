# npm是一个配合nodejs使用的包管理工具
 - 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
 - 允许用户从NPM服务器下载并安装别人编写的命ä¤行程序到本地使用。
 - 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使。

## 查看自己npm版本
```
npm -v
```
1、新版的nodejs中已经集成了npm  
2、旧版本的npm 可以使用命令进行升级
```
sudo npm install npm -g    
```
  💕windows 用户
```
npm install npm -g 
```
## 更换淘宝镜像源，安装速度更快
```
npm install -g cnpm --registry=https://registry.npm.taobao.org

npm install -g nrm

nrm ls   # 可以查看当前可用的镜像源列表，当前使用的镜像后又`*` 标记

nrm use mirrorname   # 可以切换当前使用的镜像地址， 
```
安装上述cnpm之后，执行时可用`cnpm`代替`npm`命令

## 使用npm安装模块
```
npm install <Module name>
```
安装好之后，该模块会出现在node_modules目录中，因此在代码中只需要通过require('<module name>')
就可以直接使用。(以安装express为例)
```
var express = require('express');
```
## 安装模块分为全局安装与当前项目目录中安装
 - 这两种安装方式在执行命令中只有一个参数的差异，
 * 全局安装需要添加参数-g,表示global 全局安装之后在所有的项目中都可以使用这个模块，可以直接
   在命令行中使用,安装的模块会在你的`/usr/local` 目录里面或者在你安装Node的目录中，
 * 在本地安装的只在当前项目中可用，不会影响其他项目，该安装的模块会在执行npm命令的目录中
   生成`node_modules`,使用时直接require（）引入项目中，
```
npm install express        #本地安装
npm install express -g     #全局安装
```
* 查看安装的信息
```
npm list -g                #查看全局安装的模块信息
npm list <module name>     #查看安装的模块的信息
```
## 使用package.json 
package.json位于模块目录下，定义包的属性，例如express的packsge.json位于`node_modules/express/`

## 卸载模块
```
npm uninstall express   
```
卸载后可以到`node_modules`中查看该模块是不是已经没有了,或者使用
```
npm ls  #查看当前node_modules中安装的模块
```
## 更新模块
```
npm update express
```

## 搜索模块
```
npm search express
```
## 清理缓存

```
npm cache clean
```
* 当遇到使用npm安装node_modules的时候总是提示: npm resource busy or locked ....
  这个方法就比较实用

## 使用npm创建模块，具体参见
[npm使用介绍](https://www.runoob.com/nodejs/nodejs-npm.html)

