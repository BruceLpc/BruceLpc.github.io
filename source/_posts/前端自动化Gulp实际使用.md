---
title: '前端自动化Gulp实际使用'
date: 2017-04-14 15:12:15
tags:
---

## 前言
---
在最开始接触自动化构建工具的时候，一直有一个疑问。假如我们只压缩js代码，那么原始路径和输出路径是肯定不能放在一起的，因为文件名的相同，会发生覆盖或者报错。

那么问题来了，如果只改变压缩后新JS文件的路径，而不改变html内部src的引用路径，那么事实上html中引用的还是之前未压缩的JS，只能手动修改路径。但我在想自动化构建工具本身是提高效率的，不可能出现这样麻烦的事。折腾了几天我发现是我理解错了，如果只是单纯的压缩CSS UI库和压缩JS框架，然后rename为min.css|min.js那倒没什么问题，因为本身是库和框架，没有引用没有依赖。但在实际的项目中，我们的js、css、img都是服务于html，或者说都是相互有引用关系的。所以单纯的把JS或者CSS压缩后输出到新的目录，而项目中的其他代码不迁移过来的话，想要使用压缩过后的代码就必须手工去调整。

## 实际应用
---
Gulp自动化构建工具的正确使用姿势应该是这样的。

我们有一个项目名称叫website，那么我们的项目的文件结构应该是这样。（也不一定是这样，结构可以自己调整）

website
　　|-package.json
　　|-gulpfile.js

　　|-dist
　　　　|-空
　　|-src
　　　　|-css
　　　　|-js
　　　　|-iamges
　　　　|-index.html
在项目文件中，website目录下包含有src项目实际文件夹，dist(distribution 发布)项目部署文件夹、以及package.json、gulpfile.js配置文件。

![gulp原理](http://upload-images.jianshu.io/upload_images/1245223-ec076d48fe356b00.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从图中可以大致看出在我们一共有2个文件夹分别应用不同的环境，src文件夹是我们开发中使用的文件夹，而dist文件夹是我们实际部署的文件。如果项目中有bug我们肯定是修改src文件夹的文件，然后一键自动化来构建部署文件。

这样我们已经基本懂了gulp的主要工作原理，但实际上gulp还有许多提高我们前端开发效率的插件，比如游览器自动刷新，自动开启服务器，合并js、css等等。

那么我们来看一个实际的gulpfile.js的配置文件，来更好的理解gulp这款自动化构建工具。

gulpfile.js

``` javascript
//引入gulp及各种组件;

var gulp = require('gulp'), //gulp插件

    uglify = require('gulp-uglify'), //js代码压缩

    minifyCSS = require('gulp-minify-css'),  //css代码压缩

    sass = require('gulp-sass'),  //sass代码压缩

    imagemin = require('gulp-imagemin'),  //图片压缩

    imageminJpegRecompress = require('imagemin-jpeg-recompress'),  //jpg图片压缩

    imageminOptipng = require('imagemin-optipng'),  //png图片压缩


//设置各种输入输出文件夹的位置;主要为html、css、js、image四类为主

var srcScript = './src/js/*.js',

    distScript = './dist/js',

    srcCss = './src/css/*.css',

    distCSS = './dist/css',

    srcSass = './src/css/*.scss',

    distSass = './dist/css',

    srcImage = './src/img/*.*',

    distImage = './dist/img',

    srcHtml = './src/*.html',

    distHtml = './dist';


//处理JS文件:压缩,然后换个名输出;

//命令行使用gulp script启用此任务;

gulp.task('script', function() {

    gulp.src(srcScript)

        .pipe(uglify())

        .pipe(gulp.dest(distScript));

});


//处理CSS文件:压缩,然后换个名输出;

//命令行使用gulp css启用此任务;

gulp.task('css', function() {

    gulp.src(srcCss)

        .pipe(minifyCSS())

        .pipe(gulp.dest(distCSS));

});


//SASS文件输出CSS,天生自带压缩特效;

//命令行使用gulp sass启用此任务;

gulp.task('sass', function() {

    gulp.src(srcSass)

        .pipe(sass({

            outputStyle: 'compressed'

        }))

        .pipe(gulp.dest(distSass));

});


//图片压缩任务,支持JPEG、PNG及GIF文件;

//命令行使用gulp jpgmin启用此任务;

gulp.task('imgmin', function() {

    var jpgmin = imageminJpegRecompress({

            accurate: true,//高精度模式

            quality: "high",//图像质量:low, medium, high and veryhigh;

            method: "smallfry",//网格优化:mpe, ssim, ms-ssim and smallfry;

            min: 70,//最低质量

            loops: 0,//循环尝试次数, 默认为6;

            progressive: false,//基线优化

            subsample: "default"//子采样:default, disable;

        }),

        pngmin = imageminOptipng({

            optimizationLevel: 4

        });

    gulp.src(srcImage)

        .pipe(imagemin({

            use: [jpgmin, pngmin]

        }))

        .pipe(gulp.dest(distImage));

});


//把所有html页面扔进dist文件夹(不作处理);

//命令行使用gulp html启用此任务;

gulp.task('html', function() {

    gulp.src(srcHtml)

        .pipe(gulp.dest(distHtml));

});


//监控改动并自动刷新任务;

//命令行使用gulp auto启动;

gulp.task('auto', function() {

    gulp.watch(srcScript, ['script']);

    gulp.watch(srcCss, ['css']);

    gulp.watch(srcSass, ['sass']);

    gulp.watch(srcImage, ['imgmin']);

    gulp.watch(srcHtml, ['html']);

});


//gulp默认任务(集体走一遍,然后开监控);

gulp.task('default', ['script', 'css', 'sass', 'imgmin', 'html', 'auto']);
```
配置文件中每一个命令都有一些解释，上面这个配置文件基本可以满足常见开发需求。特别注意的是html的代码事实上是没有经过任何处理就被输出了。这样的技巧可以运用在字体上，把字体文件也迁移过来。

## 总结
---
通过这几天的学习，弄明白了前端自动化的一些工作流程，能够把所学到的技能真正运用到实际项目中，在实际的使用过程中确实感到方便许多。大家可以参考一下这份配置文件，进行一些优化和调整。
