---
title: git使用教程
date: 2017-04-17 09:50:54
tags:
---

## 前言
Git是什么？
Git是目前世界上最先进的分布式版本控制系统。
能做什么？
自动记录文件的改动，生成修改日志，可以回退到每一次修改之前。

## git安装

从 https://git-for-windows.github.io 下载最新版的git,然后默认选项安装即可。
安装完成后，还需要最后一步设置，在命令行输入：

	git config --global user.name "Your Name"  
	git config --global user.email "email@example.com"

## 创建版本库
什么是版本库呢？
版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

	1. 创建一个空目录
	2. git init 把这个目录变成git可以管理的仓库

## 把文件提交到版本库
创建一个readme.txt文件，内容如下：
Git is a version control system.
Git is free software.

	1. 用命令git add告诉Git，把文件添加到仓库：`git add readme.txt`
	2. 用命令git commit告诉Git，把文件提交到仓库：`git commit -m "wrote a readme file"`

## 版本回退
在git中，用HEAD表示当前版本，上一个版本就是HEAD^,上上一个版本就是HEAD^^,如果往上100个版本写100个^比较容易数不过来，所以写成HEAD~10。

	1. 使用git log 命令显示从最近到最远的提交日志
	2. 使用git reset --hard HEAD^  或者git reset --hard 版本号
	3. 使用git reflog查看所有的提交历史

## 工作区与暂存区

当我们把文件往Git版本库里添加的时候，是分两步执行的：
第一步、用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
第二步、用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

当我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。
可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。
工作区：git使用的本地目录
暂存区：git add提交到的目录

![图解](http://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

## 远程仓库

##### 从远程仓库克隆
要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。`git clone git@github.com:path/repo_name.git`
Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

##### 推送到远程仓库
关联远程库：`git remote add origin git@server-name:path/repo_name.git`;
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；



## 分支管理策略
在实际开发中，我们应该按照几个基本原则进行分支管理：
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了
![图解](http://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)

## bug分支管理
在软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。
当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支issue-101来修复它，但是，当前正在dev上进行的工作还没有提交.
并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？
幸好，Git还提供了一个stash功能，可以把当前工作现场“暂存”起来，等以后恢复现场后继续工作：

	git stash  暂存工作区
	git stash list 查看 暂存列表
	一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
	另一种方式是用git stash pop，恢复的同时把stash内容也删了：

## 创建与合并分支

	- 查看分支：git branch

	- 创建分支：git branch <name>

	- 切换分支：git checkout <name>

	- 创建+切换分支：git checkout -b <name>

	- 合并某分支到当前分支：git merge <name>

	- 删除分支：git branch -d <name>


## 分支冲突

在新建分支上修改后提交完，切换到master分支上继续修改完提交，
相当于在两个分支上都有了修改，merge时就会提示文件存在冲突。



## 常用命令
任何操作都需要以 git 命令为开头

	本地操作：

	git init  初始化一个本地仓库  新建为 master主分支
	git status  查看当前分支状态
	git add  <文件名>   将文件更改添加到分支状态中 相当于文件等待被提交
	git commit -m <"描述信息">  提交并添加描述信息
	git branch  查看分支   前面带*号的为当前所在分支
	git branch --all 查看所有分支
	git branch <分支名称>  新建分支
	git checkout -b <分支名>  新建分支并切换到此分支
	git checkout <分支名>  切换分支
	git merge <分支名>   将指定分支名合并到当前分支  一般为切换到主分支使用此命令
	git merge --no-ff -m "提交描述" <分支名>   合并分支并提交
	git branch -D <分支名>  强制删除分支，不管分支是否有未提交合并的代码

	git tag 查看所有标签
	git tag <标签名> 在当前状态下新建一个标签，可用来当作版本号使用
	git tag -a <标签名称> -m <"标签描述"> <提交id>  在指定的提交状态下新建一个标签
	git show <标签名称>   查看标签的详情
	git tag -d <标签名> 删除标签
	git push origin <标签名>   推送标签到远程仓库
	git push origin --tags  推送所有未推送的标签
	git push origin :refs/tags/<标签名>   删除远程标签，本地要先删除后才可以


	git checkout <标签名> 切换到标签名指定的状态
	git diff <文件名> 查看文件修改内容

	git log      查看提交日志   --pretty=oneline  此参数减少输出信息  穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
	git reflog   要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。
	git log --graph --pretty=oneline --abbrev-commit   查看分支合并图
	git reset --hard <HEAD^||提交ID> 穿梭到指定提交版本
	HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。

	git checkout -- <文件名>  将指定的文件恢复到最近一次 commit或add操作时候的状态
	git reset HEAD <文件名>   将指定的文件从暂存区的修改撤销掉（unstage），重新放回工作区
	git rm <文件名>		 删除指定的文件

	git stash  把当前工作现场“储藏”起来，等以后恢复现场后继续工作
	git stash list 查看暂存状态
	git stash apply 恢复暂存状态
	git stash drop  删除暂存状态
	git stash pop   恢复并删除暂存状态
	git stash apply <stash@{0}>  恢复指定的暂存状态

	远征仓库操作:
	git clone <远程地址>  从远征仓库拷贝过来代码，相当于建立本地分支
	git pull 将最新的提交从远程仓库抓取下来
	如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to=origin/master
	git push  将本地修改后的代码提交到远程仓库
	git push <远程仓库名，默认origin> <本地分支名>  将指定的分支推送到远程分支上
	
	git push origin :dev 删除远程仓库dev分支
	git remote -v 查看远程仓库  -v 为详细信息
	git checkout -b dev origin/dev 创建分支dev 并与远程dev分支关联起来
	git checkout -b <本地支分支名> <远程仓库名，默认origin>/<远程支分支名> 拉取远程主分支下的支分支。。。
	git branch --set-upstream <本地支分支名> <远程仓库名，默认origin>/<远程支分支名>  将本地分支与远程指定的分支关联起来

	//以下为先有本地库，再建立远程库操作所用的命令
	git remote add origin <URL地址> 本地库与远征库关联
	git push -u origin master 关联后，使用命令第一次推送master分支的所有内容，   -u参数为推送当前分支所有内容

## 总结
Git虽然极其强大，命令繁多，但常用的就那么十来个，掌握好这十几个常用命令，已经可以得心应手地使用Git了。	
	





