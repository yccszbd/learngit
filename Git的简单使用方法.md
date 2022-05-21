# Git的简单使用方法

`cd`：进入某文件文件夹

`mkdir`：创建文件夹

`pwd`：显示当前路径

`rm`:删除某文件

`cat`：可以查看当前文件夹下某文件的内容

`ls`:列出当前文件夹的目录

> - -a 显示所有文件及目录 (. 开头的隐藏文件也会列出)
> - -l 除文件名称外，亦将文件型态、权限、拥有者、文件大小等资讯详细列出
> - -r 将文件以相反次序显示(原定依英文字母次序)
> - -t 将文件依建立时间之先后次序列出
> - -A 同 -a ，但不列出 "." (目前目录) 及 ".." (父目录)
> - -F 在列出的文件名称后加一符号；例如可执行档则加 "*", 目录则加 "/"
> - -R 若目录下有文件，则以下之文件亦皆依序列出

`git init`：把当前文件夹变成由Git管理的仓库(`ls -ah`显`.git`文件)

`git add read.txt`：把文件加入暂存区（stage）

`git commit -m"xxx"`:进行一次提交，生成一个新的版本，该次提交信息为`xxx`

> `git add`可以在一次`git commit`中执行很多次

`git status`:查看当前的仓库状态，是否发生了改动

`git diff read.txt`:查看仓库状态==版本库最新状态包括暂存区==和主机状态的区别

`git log`:记录每一次递交的版本的信息

> 从近到远，版本信息类似于：
>
> commit 34pxo9UsVkDWRNu5XTCXWuLq3BQEm1kwzo477073 (HEAD -> master) //版本号 head表示这是最近的版本
> Author: Michael Liao <askxuefeng@gmail.com>  //作者和邮箱
> Date:   Fri May 18 21:06:15 2018 +0800   //日期
>
> append GPL //提交信息
>
> 

`git reset -- hard HEAD^`：版本回退

> -- hard 后续解释
>
> HEAD\^就是上一个版本，同理HEAD\^\^就是上两个版本 ，上一百个版本就是HEAD~100

`git reset -- hard 1094a`:版本移到以1094a开头的版本，可以前可以后

`git reflog`:查看每一次命令，在这里可以看到每一次命令的版本号以进行`reset`

`git checkout -- read.txt`：撤销修改,总之就是撤销到最近一次`git add`或者`git commit`的状态

> 一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
>
> 一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

对于已经在暂存区的文件，我们想要它回到工作区的状态，用``checkout`，办不到

`git reset HEAD read.txt`：把暂存区的文件回退到工作区

> 使用`reset`会退到工作区，再使用`checkout`回退到修改前

`git rm`:删除某文件

> `git rm`和`git add`类似，都需要`git commit`来确认
>
> 对于错误删除的文件可以使用`git checkout -- read.txt`来复原，事实上`git checkout`就是用版本中的文件提出按工作的文件

` ssh-keygen -t rsa -C "youremail@example.com"`:在本地生成SSh key

`git remote add origin git@github.com:michaelliao/learngit.git`:与远程库形成绑定

`git push -u orgin master`:把本地的把主机仓库推送到远程库

> `origin`是远程库的默认名称
>
> `master`参数时把本地的master分支推送给远程库的`master`分支
>
> `-u`把本地的`master`和远程库的`master`连接起来，简化以后的指令
>
> 以后只需要`git push origin main`就把本地库的main的最新修改提交到Github上去了

`git remote -v`:查看远程库信息

`git remote rm orgin`:接触远程库和本地库的绑定关系

`git clone git@github.com:michaelliao/gitskills.git`:在当前命令行的文件夹下克隆一个本地库

![](C:\Users\yccszbd\Desktop\Picture\GIt\capture_stepup1_1_1.png)

![](C:\Users\yccszbd\Desktop\Picture\GIt\capture_stepup1_1_2.png)

`git checkout -b dev`:创建并切换到分支dev,约等于

> git brancn dev
>
> git checkout dev

`git merge dev`:把指定分支的工作成果合并到当前分支上

`git branch -d dev`:将dev分支删除



:heavy_exclamation_mark::在Git中更常使用`switch`来切换分支

`git switch -c dev`:创建并切换至分支dev

`git switch master`:切换至已有的分支master



:heavy_exclamation_mark::在Git中如果当前分支和目标分支的内容有冲突的化不能合并，必须手动解决冲突，通过`git status`可以查看冲突状态

`git log --graph`:可以看到分支合并图



:heavy_exclamation_mark::git中默认的合并分支策略尾Fast forward模式，在历史记录里看不到这次合并的消息

`git --no-ff -m"master with no-ff" dev`:合并dev分支到当前分支并提交一个commit，此时再使用git log时可以查看到合并分支的记录

![](C:\Users\yccszbd\Desktop\Picture\GIt\0.png)

:heavy_check_mark::在实际开发中，``master`一般为发行版本，不在其上进行开发，`dev`为开发版本，每个人在其提交自己的开发进度，每个人自己会有一个`ycc`是自己的开发版本，一般自己在这里进行开发



`git stash`:把当前的工作状态存放在当前分支的`stash`区

`git stash list`:查看暂存记录

> 在需要暂停目前的工作时使用，此时使用git status查看工作区状态为干净的状态，此时可以跳转到别的分支解决别的问题，等再次回到该分支时可以复原

`git stash apply`:恢复但是stash的内容并不删除，需要在使用`git stash drop`来删除

`git stash pop`:恢复的同时删除stash内容

`git cherry-pick 4c90`:把4c90这个提交在当前分支再次提交一遍

`git branch -D feature`:把feature分支强制删除

> 对于没有合并的分支Git不允许删除并给出提醒，想要删除要使用`-D`



`git remote`:查看远程库信息

`git remotr -v`:查看远程库详细信息

`git push origin master`:把本地的master分支提交到远程库的origin上去

>- `master`分支是主分支，因此要时刻与远程同步；
>- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
>- `bug`分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
>- `feature`分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

`git checkout -b dev origin/dev`:创建一个dev并把远程的dev分支与本地的dev分支连接

`git branch --set-upstream-to=origin/dev dev`:把本地的dev分支与远程库origin/dev分支相连接

:heavy_exclamation_mark::只有连接了才可以方便的`git push`和`git pull`

`git pull`：抓取远程库最新的提交与本地的当前分支合并，若出现冲突则解决冲突再合并,此时再commit和push



`git tag`:查看标签

> 标签与一个一次commit绑定，表示某一次版本,标签建立不可改变

`git tag v1.0`:再当前分支的最近一次版本上绑定标签`v1.0`

`git tag v0.9 f52c633`:把f52c633这次提交绑定标签`v0.9`

`git log --pretty=oneline --abbrev-commit`:显示简略commit记录

> 12a631b (HEAD -> master, tag: v1.0, origin/master) merged bug fix 101
> 4c805e2 fix bug 101
> e1e9c68 merge with no-ff
> f52c633 add merge
> cf810e4 conflict fixed

`git show v0.9`:查看某标签所绑定commit内容

`git tag -a v0.1 -m"version 0.1 realeased 1094adb`:创建一个带说明的标签，在tag show的时候额外展示这次绑定标签的内容

`git tag -d v0.1`：本地删除标签v0.1

`git push origin v1.0`:把本地的v1.0标签推送到远程

`git push --tags`：把本地所有标签都推送到远程

`git push origin :refs/tags/v0.9`：把远程库里的v0.9tag删除



### Github使用

─ GitHub  ────────────────────────────────────┐
│                                             │
│ ┌─────────────────┐     ┌─────────────────┐ │
│ │ twbs/bootstrap  │────>│  my/bootstrap   │ │
│ └─────────────────┘     └─────────────────┘ │
│                                  ▲          │
└──────────────────────────────────┼──────────┘
                                   ▼
                          ┌─────────────────┐
                          │ local/bootstrap │
                          └─────────────────┘

1.在别人的仓库下Fork到自己的仓库

2.在自己的本地git clone并增加自己的修改

3.pull request给原仓库

