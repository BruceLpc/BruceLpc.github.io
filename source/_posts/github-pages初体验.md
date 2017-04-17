---
title: Github-Pages初体验
date: 2017-02-21 17:09:06
tags: github pages
---

## 前言
---
捣鼓了两天终于搞明白了 github pages 的使用，给自己搭建了个博客，过程中也遇到不少问题，趁现在还没忘赶紧记下：

## 是什么？
我的理解是github衍生品，免费提供存储空间，自带主题、支持自制页面，可以用来搭建博客或者托管网页，还可以把自己的域名解析到此空间，用域名访问。

搭建之前需要先了解
	>nodejs
	>git

## 开始搭建
---
##### 创建github pages:
登录github帐号 新创建一个仓库  仓库名格式必须是：`usename.github.io`,  usename 必须是github用户名，创建完成后，就可以使用https://usename.github.io在浏览器访问

##### 更改域名:
通过ping usename.github.io 获取到当前网址的ip，然后解析域名到当前ip，在当前主分支下创建文件CNAME,输入刚解析的域名，等一分钟就可以使用域名访问。

## 使用hexo
---
Hexo 是一个快速、简洁且高效的博客框架,使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页，用node.js开发,作者是台湾大学生tommy.

##### 本地安装：
假如本机已经安装npm 即可直接使用如下命令安装：
npm install -g hexo-cli
hexo网上教程很多，但很多都是过期的，建议直接看官网，[查看hexo中文文档](https://hexo.io/zh-cn/docs/);

##### 建站：
Hexo 安装好了之后，在本地新建目录，在终端cd到新建目录，使用如下命令初始化hexo：

	hexo init
	npm install

##### 配置：
你可以在 _config.yml 中修改大部份的配置。[配置详细介绍](https://hexo.io/zh-cn/docs/configuration.html)

##### 部署到github：
编辑器打开 _config.yml 文件，滚动到最下面添加如下配置信息：

	deploy:
	type: git
	repo: https://github.com/biggercoffee/biggercoffee.github.io.git
	branch: master

把其中 repo 字段的值替换成你的github pages提交代码的git地址，有时候可能会报错，使用命令 `npm hexo-deployer-git` 安装之后在执行 `hexo d` 提交.

## 更改主题
---
网上有很多hexo主题，我比较喜欢简洁干净，因此就选择了fexo。

##### 安装:
	cd hexofile
	git clone git@github.com:forsigner/fexo.git themes/fexo

##### 启用:
打开博客根目录的 _config.yml 设为 theme: fexo

##### 升级:
	cd themes/fexo
	git commit -am 'update'
	git pull

##### 配置:

主题配置全部在theme/fexo里面完成，配置文件在theme/fexo/_config.yml。详细配置可以查看官网[查看](http://forsigner.com/2016/03/10/fexo-doc-zh-cn/#开始)

## 常用命令
	- hexo init [folder] 新建一个网站。如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站
	- hexo new layout title 新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话，请使用引号括起来
	- hexo generate 生成静态文件  可以简写成`hexo g`
	- hexo publish [layout] <filename> 发布草稿
	- hexo server 启动服务器。默认情况下，访问网址为： http://localhost:4000/, 可以简写成 `hexo s`
	- hexo clean 清除缓存文件 (db.json) 和已生成的静态文件 (public)。








	


























