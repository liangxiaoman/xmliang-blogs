---
title: git代码管理流程
abbrlink: 13a7de0
date: 2021-12-04 00:23:15
author: xmliang
top: true
cover: false
toc: false
mathjax: false
summary: 
categories:  软件工具
tags:
  - Git
# img: /source/images/xxx.jpg
# coverImg: /images/1.jpg
# password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
---

# git代码管理流程

一、git基本流程图

![](13a7de0/1.jpg)

```
Workspace：工作区
Index / Stage：暂存区
Repository：仓库区（或本地仓库）
Remote：远程仓库
```

二、Git本地操作

1、新建代码库

```bash
$ git init
```

说明：在当前目录新建一个Git代码库，对项目进行版本管理

2、对暂存区的文件增加/删除

```bash
# 添加当前目录的所有文件到暂存区
$ git add .

# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]
```

3、代码提交

```bash
# 提交暂存区到仓库区
$ git commit -m [message]

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]

# 切换到某个快照
$ git checkout [commit]
```

4、分支

```bash
# 列出所有本地分支
$ git branch

# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 合并指定分支到当前分支
$ git merge --no-ff [branch]

# 删除分支
$ git branch -d [branch-name]
```

5、查看信息

```bash
# 显示有变更的文件
$ git status

# 显示当前分支的版本历史
$ git log

# 显示暂存区和工作区的差异
$ git diff

# 显示暂存区和上一个commit的差异
$ git diff --cached

# 显示工作区与当前分支最新commit之间的差异
$ git diff HEAD

# 显示某次提交的元数据和内容变化
$ git show [commit]

# 显示当前分支的最近几次提交
$ git reflog
```

6、撤销

```bash
# 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]

# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop
```

三、远程操作

```bash
# 显示所有远程仓库
$ git remote -v

# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]

# 上传本地指定分支到远程仓库
$ git push [remote] [branch]

#删除远程没有本地有的分支
$ git remote prune origin
```

四、分支管理策略

![](13a7de0/2.png)

![](13a7de0/3.png)

1、拉取远程分支master | dev

![](13a7de0/4.png)

2、在需要修改的分支上创建Fixbug-x | feature-x分支（x代表修改的名称）

![](13a7de0/5.png)

​	修改内容并commit

![](13a7de0/6.png)

3、切换回要修改的分支，拉取分支代码

​	注意：确定Fixqiehuanbug-x | feature-x都commit后

![](13a7de0/7.png)

4、合并分支

![](13a7de0/8.png)

5、提交分支

![](13a7de0/9.png)

6、删除临时分支

![](13a7de0/10.png)

五、合并时 `--no-ff` 参数

1、默认是直接将分支指向需要合并的分支

<img src="13a7de0/11.png" style="zoom:67%;" />

2、使用 `--no-ff` 参数

<img src="13a7de0/12.png" style="zoom:67%;" />

3、说明

​	上面两个最大的区别是是否搅乱 master 的提交历史，没有`--no-ff` 参数回退版本会回退到分支的上一个版本

六、分支合并出错处理

```bash
解决方法：

1.找到最后一次提交到master分支的版本号，即【merge前的版本号】

2.会退到某个版本号
$ git reset --hard 【merge前的版本号】

这个时候已经会退到了上一次提交的版本，但是之后的修改还是存在master分支上，以下步骤很关键

3.重新创建一个分支,这时候的分支就是上一次提交的代码
$ git checkout -b newmaster

4.推到对应的远程newmaster
$ git push

5.这个时候相当于备份做好了，接下来就可以删除本地及远端的master分支
$ git branch -d master
$ git push --delete origin master

6.从newmaster分支，重新在创建master分支，并推向远端
$ git checkout -b master
$ git push
```

七、资源

- [《Git 原理入门》](https://www.ruanyifeng.com/blog/2018/10/git-internals.html)

- [《Git分支管理策略》](http://www.ruanyifeng.com/blog/2012/07/git.html)

- [《Git 使用规范流程》](https://www.ruanyifeng.com/blog/2015/08/git-use-process.html)

- [《常用 Git 命令清单》](https://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)

- [《Git 远程操作详解》](https://www.ruanyifeng.com/blog/2014/06/git_remote.html)

- [《Git 工作流程》](https://www.ruanyifeng.com/blog/2015/12/git-workflow.html)

