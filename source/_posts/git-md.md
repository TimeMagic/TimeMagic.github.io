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

### 解决冲突

``` bash
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
```

Git还会自动提示我们当前master分支比远程的master分支要超前1个提交。

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用git log --graph命令可以看到分支合并图。

### 分支管理策略

通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

### bug修复（hotfix）

一种情况，在dev分支开发的时候，突然有紧急bug要修复，手头工作还没有完成。而且要切换到其他分支，这个时候怎么办。
可以通过下面命令

``` bash
$ git stash
```

把暂时修改的没有保存的文件缓存起来，此时工作区变成最近的版本内容。这个时候就可以切换了。修复完bug回来，dev暂时是不显示之前没有完成的工作内容的.

刚才的工作现场存到哪去了？用git stash list命令看看：

``` bash
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

另一种方式是用git stash pop，恢复的同时把stash内容也删了：

``` bash
$ git stash apply stash@{0}
```

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。

### 多人协作

当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。

要查看远程库的信息，用git remote：

``` bash
$ git remote
origin
```

用git remote -v显示更详细的信息：

``` bash
$ git remote -v
origin  git@github.com:TimeMagic/TimeMagic.git (fetch)
origin  git@github.com:TimeMagic/TimeMagic.git (push)
```

#### 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

``` bash
$ git push origin master
```

如果要推送其他分支，比如dev，就改成：

``` bash
$ git push origin dev
```

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

> * master分支是主分支，因此要时刻与远程同步；
> * dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
> * bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
> * feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！

#### 抓取分支

多人协作时，大家都会往master和dev分支上推送各自的修改。
现在，模拟一个你的小伙伴，可以在另一台电脑（注意要把SSH Key添加到GitHub）或者同一台电脑的另一个目录下克隆：

当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的master分支。不信可以用git branch命令看看：

``` bash
$ git branch
* master
```

现在，你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支：

``` bash
$ git checkout -b dev origin/dev
```

现在，他就可以在dev上继续修改，然后，时不时地把dev分支push到远程,你的小伙伴已经向origin/dev分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送。

``` bash
$ cat env.txt
env

$ git add env.txt

$ git commit -m "add new env"
[dev 7bd91f1] add new env
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt

$ git push origin dev
To github.com:michaelliao/learngit.git
 ! [rejected]        dev -> dev (non-fast-forward)
error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送：

``` bash
$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev
```

git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：

``` bash
$ git branch --set-upstream-to=origin/dev dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```

再pull：

``` bash
$ git pull
Auto-merging env.txt
CONFLICT (add/add): Merge conflict in env.txt
Automatic merge failed; fix conflicts and then commit the result.
```

这回git pull成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再push：

``` bash
$ git commit -m "fix env conflict"
[dev 57c53ab] fix env conflict

$ git push origin dev
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
   7a5e5dd..57c53ab  dev -> dev
```

因此，多人协作的工作模式通常是这样：

首先，可以试图用git push origin <branch-name>推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

> * 查看远程库信息，使用git remote -v；
> * 
> * 本地新建的分支如果不推送到远程，对其他人就是不可见的；
> * 
> * 从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
> * 
> * 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
> * 
> * 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/是branch-name；
> * 
> * 从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

注意，origin 是远程分支的名称，是可以随意起的。

#### Rabase

### 标签管理

发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

Git有commit，为什么还要引入tag？

“请把上周一的那个版本打包发布，commit号是6a5819e...”

“一串乱七八糟的数字不好找！”

如果换一个办法：

“请把上周一的那个版本打包发布，版本号是v1.2”

“好的，按照tag v1.2查找commit就行！”

所以，tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

#### 创建标签

在Git中打标签非常简单，首先，切换到需要打标签的分支上：然后，敲命令git tag <name>就可以打一个新标签：

``` bash
$ git tag v1.0
```

默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了：

``` bash
$ git tag v1.1 f23fedc
```
注意，标签不是按时间顺序列出，而是按字母排序的。可以用git show <tagname>查看标签信息：

``` bash
$ git show v0.9
commit f52c63349bc3c1593499807e5c8e972b82c8f286 (tag: v0.9)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:56:54 2018 +0800

    add merge

diff --git a/readme.txt b/readme.txt	
```	

还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：

``` bash
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
$ git show v0.1
tag v0.1
Tagger: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 22:48:43 2018 +0800

version 0.1 released

commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (tag: v0.1)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

diff --git a/readme.txt b/readme.txt
```

注意：**标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签**。

> * 命令git tag <tagname>用于新建一个标签，默认为HEAD，也可以指定一个commit id；
> * 命令git tag -a <tagname> -m "blablabla..."可以指定标签信息；
> * 命令git tag可以查看所有标签。

#### 操作标签

如果标签打错了，也可以删除
因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
然后，从远程删除。删除命令也是push，但是格式如下：

> * 命令git push origin <tagname>可以推送一个本地标签；
> * 命令git push origin --tags可以推送全部未推送过的本地标签；
> * 命令git tag -d <tagname>可以删除一个本地标签；
> * 命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

##  小科技

### git配置别名

有没有经常敲错命令？比如git status？status这个单词真心不好记。

如果敲git st就表示git status那就简单多了，当然这种偷懒的办法我们是极力赞成的。

我们只需要敲一行命令，告诉Git，以后st就表示status：

``` bash
$ git config --global alias.st status
```


