---
title: Git教程
password: 
date: 2018-12-06 09:11:10
tags: git
categories: Git
---

## 安装

window下安装过后,要进行全局配置,注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

``` bash
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

在进入到log界面下，退出只需要在英文状态下，按下Q键即可

## 创建版本库 repository

什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

``` bash
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用ls -ah命令就可以看见。

### 添加文件, 查看状态

> * git add file.txt | file1.txt file2.txt | .

``` bash
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

``` bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

git status命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，readme.txt被修改过了，但还没有准备提交的修改。

``` bash
$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
```

git diff顾名思义就是查看difference，显示的格式正是Unix通用的diff格式

### 版本回退

``` bash
$ git reset --hard HEAD^ | HEAD~100
```

> * HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
> * 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
> * 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

### 工作区 暂存区

#### 工作区（Working Directory）

就是你在电脑里能看到的目录

### 管理修改

每次修改，如果不用git add到暂存区，那就不会加入到commit中。 commit统一提交

### 撤销修改

> 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
> 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。
> 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

### 删除操作

一种情况是确定删除

``` bash
$ git add fileName 
$ git commit -m "entry"
```

一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

``` bash
$ git checkout -- pathFile/fileName
```

## 远程仓库

本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以需要一点设置。

- 第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

``` bash
$ ssh-keygen -t rsa -C "youremail@example.com"
```

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码
如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

- 第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：
然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：

- 第3步，登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库。

- 第4步，

``` bash
$ git remote add origin git@github.com:TimeMagic/xxx.git
```

远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。

- 第4步，推送内容到远程仓库

``` bash
$ git push -u origin master
```

把当前分支master推送到远程。由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

从现在起，只要本地作了提交，就可以通过命令：

``` bash
$ git push origin master
```

把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

### 克隆远程仓库

远程库已经准备好了，下一步是用命令git clone克隆一个本地库

``` bash
$ git clone git@github.com:michaelliao/gitskills.git
Cloning into 'gitskills'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
Receiving objects: 100% (3/3), done.
```
Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

## 分支管理

master分支。HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。

一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点：

原理：

内容待定

首先，我们创建dev分支，然后切换到dev分支：
 
``` bash
$ git branch -b dev
Switched to a new branch 'dev'
```

用git branch命令查看当前分支：

``` bash
$ git branch
* dev
  master
```

分支切换

``` bash
$ git checkout master
Switched to branch 'master'
```

合并分支

``` bash
$ git merge dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

合并完成后，就可以放心地删除dev分支了：

``` bash
$ git branch -d dev
Deleted branch dev (was b17d20e).
```