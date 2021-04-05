---
title: gulp+browserify 构建react开发环境
date: 2016-10-31 13:23:07
tags:
---

最近准备学习一下前端火热的框架—react ，好扩充一下自己增加一些相关经验。
因为之前学过一点 `gulp` 又听说前端构建 `browserify`和`gulp`更配哦，虽说最近webpack最近风头正热，但是前端变化那么快，一味的追求最新技术反而会适得其反，适合自己的才是最好的。所以准备用`gulp+browserify`的组合搭建（前者进行打包，后者进行任务构建）。

1. 安装gulp和browerify  
- 全局安装gulp
命令行输入 `npm install –g gulp`，然后`gulp -v`查看gulp版本信息，如果出现
```
[16:01:50] CLI version 3.9.1
[16:01:50] Local version 3.9.1
```
证明已经安装好，只有在全局安装好gulp才能执行gulp命令。

- 项目本地安装gulp、browerify  
进入项目根目录 n`pm install --save-dev gulp browerify`
这里`--save-dev`是为了把依赖信息保存到package.json的 devDependencies里，这样项目协作的时候只需要提交`package.json`,到时候其他人更新以后直接n`pm install`就可以下载依赖了。
由于npm镜像在国外，下载缓慢，容易出现一些不知道的错误，采用cnpm可以下的更快（据说最近火热的yarn更牛逼）

2. 安装其他依赖
除了上述两个基本的工具，还需要安装其他辅助工具
+ `gulp-less`: 将less编译成对应的css;
+ `browser-sync`: 神器！和gulp结合自动，修改css,js,html自动刷新浏览器;
+ `vinyl-source-stream`: 用于将Browserify的bundle()的输出转换为Gulp可用的[vinyl][]（一种虚拟文件格式）流。;
+ `babelify`: 由于项目想尝试ES6语法加上react的jsx语法，需要通过babel进行转化保证兼容
+ `babel-preset-es2015`: es6语法转换babel预设插件
+ `babel-preset-react`: react jsx语法转换babel预设插件
+ `babel-core`: babel相关
+ `browserify-shim`: 把非 CommonJS 的模块转换成 CommonJS 模块

对应的devDependencies
```json
"devDependencies": {
    "babel-core": "^6.18.0",
    "babel-preset-es2015": "^6.18.0",
    "babel-preset-react": "^6.16.0",
    "babelify": "^7.3.0",
    "browser-sync": "^2.17.5",
    "browserify": "^13.1.1",
    "browserify-shim": "^3.8.12",
    "gulp": "^3.9.1",
    "gulp-less": "^3.1.0",
    "http-server": "^0.9.0",
    "react": "^15.3.2",
    "react-dom": "^15.3.2",
    "vinyl-source-stream": "^1.1.0"
  }
```
3. 编写gulpfile.js
- 首先引入模块：
```js
var gulp = require("gulp"), 
  less = require("gulp-less"),
  browserify = require("browserify"), 
  browserSync = require("browser-sync").create(),
  shim = require('browserify-shim'),
  babelify = require("babelify"),
  source = require("vinyl-source-stream");
var reload = browserSync.reload;
```
- 然后编写任务：
less-css任务：
```js
gulp.task("less",function(){
  gulp.src("src/less/index.less")
    .pipe(less())
    .pipe(gulp.dest("./css"))
    .pipe(reload({stream: true}));
});
```
react es6和jsx语法转化任务：
```js
gulp.task("jsx", function() {
  browserify('src/main.js')
    .transform(babelify, {
      presets: ['es2015', 'react']
    })
    .transform(shim)
    .bundle()
    .pipe(source('bundle.js'))
    .pipe(gulp.dest('js'));
});
``` 
为了让任务自动执行，实现真正自动化解放双手，当修改产生用`browser-sync`结合gulp的watch来自动刷新页面（这里只是用到它的一小部分功能，它还有许多其他神奇功能可以探索）

``` js
gulp.task('browser-sync', function() {
  browserSync.init({
    files: "**",  
      server: {  
        baseDir: "./"  
      }  
  });
});
gulp.task('watch', ['browser-sync'], function () {
  gulp.watch("src/main.js",["jsx"]);
  gulp.watch("src/less/index.less",["less"]);
  gulp.watch("src/main.js").on('change', reload)
});
```
最后输入`gulp watch`就可以自动运行，修改相应`jsx`和less就会自动刷新页面了。

> 参考资料

[使用gulp进行React任务的构建](http://syaning.com/2015/11/09/gulp-react-task-build/?utm_source=tuicool&utm_medium=referral)  
[Gulp系列教程：监听改变](http://www.tuicool.com/articles/iEnumu)  
[browsersync官方文档](https://browsersync.io/docs/gulp/)