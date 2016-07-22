---
layout: post
title: Git常用命令
category: 好文
tags: 技文
keywords: Git常用命令
description: Git常用命令
---
注：本文转载，[第一部分](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)，[第二部分](http://yanhaijing.com/git/2014/11/01/my-git-note/)，作者：阮一峰，颜海镜  

&nbsp;  
&nbsp;  

### 第一部分 常用Git命令清单  
作者：阮一峰  
日期：2015年12月9日  

&nbsp;  

我每天使用 Git ，但是很多命令记不住。  

一般来说，日常使用只要记住下图6个命令，就可以了。但是熟练使用，恐怕要记住60～100个命令。  

![Git常用命令](https://github.com/NemoTec/NemoTec.github.io/raw/master/public/img/7/2016-07-07/git-basic-01.png)  

下面是我整理的常用 Git 命令清单。几个专用名词的译名如下。  
```
Workspace：    工作区
Index/Stage：  暂存区
Repository：   仓库区（或本地仓库）
Remote：       远程仓库
```

#### 一、新建代码库  

```
# 在当前目录新建一个Git代码库
$ git init

# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]

# 下载一个项目和它的整个代码历史
$ git clone [url]
```  

&nbsp;  

#### 二、配置  
Git的设置文件为``.gitconfig``，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。  
```
# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```

&nbsp;  

#### 三、增加/删除文件  

```
# 添加指定文件到暂存区
$ git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
$ git add [dir]

# 添加当前目录的所有文件到暂存区
$ git add .

# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
$ git add -p

# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
```

&nbsp;  

#### 四、代码提交  

```
# 提交暂存区到仓库区
$ git commit -m [message]

# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a

# 提交时显示所有diff信息
$ git commit -v

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...
```

&nbsp;  

#### 五、分支  

```
# 列出所有本地分支
$ git branch

# 列出所有远程分支
$ git branch -r

# 列出所有本地分支和远程分支
$ git branch -a

# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]

# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 新建一个分支，指向指定commit
$ git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 切换到上一个分支
$ git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]

# 合并指定分支到当前分支
$ git merge [branch]

# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]
```

&nbsp;  

#### 六、标签  

```
# 列出所有tag
$ git tag

# 新建一个tag在当前commit
$ git tag [tag]

# 新建一个tag在指定commit
$ git tag [tag] [commit]

# 删除本地tag
$ git tag -d [tag]

# 删除远程tag
$ git push origin :refs/tags/[tagName]

# 查看tag信息
$ git show [tag]

# 提交指定tag
$ git push [remote] [tag]

# 提交所有tag
$ git push [remote] --tags

# 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]
```

&nbsp;  

#### 七、查看信息  

```
# 显示有变更的文件
$ git status

# 显示当前分支的版本历史
$ git log

# 显示commit历史，以及每次commit发生变更的文件
$ git log --stat

# 搜索提交历史，根据关键词
$ git log -S [keyword]

# 显示某个commit之后的所有变动，每个commit占据一行
$ git log [tag] HEAD --pretty=format:%s

# 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
$ git log [tag] HEAD --grep feature

# 显示某个文件的版本历史，包括文件改名
$ git log --follow [file]
$ git whatchanged [file]

# 显示指定文件相关的每一次diff
$ git log -p [file]

# 显示过去5次提交
$ git log -5 --pretty --oneline

# 显示所有提交过的用户，按提交次数排序
$ git shortlog -sn

# 显示指定文件是什么人在什么时间修改过
$ git blame [file]

# 显示暂存区和工作区的差异
$ git diff

# 显示暂存区和上一个commit的差异
$ git diff --cached [file]

# 显示工作区与当前分支最新commit之间的差异
$ git diff HEAD

# 显示两次提交之间的差异
$ git diff [first-branch]...[second-branch]

# 显示今天你写了多少行代码
$ git diff --shortstat "@{0 day ago}"

# 显示某次提交的元数据和内容变化
$ git show [commit]

# 显示某次提交发生变化的文件
$ git show --name-only [commit]

# 显示某次提交时，某个文件的内容
$ git show [commit]:[filename]

# 显示当前分支的最近几次提交
$ git reflog
```

&nbsp;  

#### 八、远程同步  

```
# 下载远程仓库的所有变动
$ git fetch [remote]

# 显示所有远程仓库
$ git remote -v

# 显示某个远程仓库的信息
$ git remote show [remote]

# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]

# 上传本地指定分支到远程仓库
$ git push [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force

# 推送所有分支到远程仓库
$ git push [remote] --all
```

&nbsp;  

#### 九、撤销  

```
# 恢复暂存区的指定文件到工作区
$ git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
$ git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
$ git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]

# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop
```

&nbsp;  

#### 十、压缩  

```
# 生成一个可供发布的压缩包
$ git archive
```

&nbsp;  
&nbsp;  

### 第二部分 我的git笔记  
作者：颜海镜  
日期：2014年11月1日  

&nbsp;  

![Git笔记](https://github.com/NemoTec/NemoTec.github.io/raw/master/public/img/7/2016-07-07/git-basic-02.png)  

#### 一、安装
在 Windows 上安装 Git 同样轻松，有个叫做 msysGit 的项目提供了安装包：  
```
http://msysgit.github.io/
```
完成安装之后，就可以使用命令行的 git 工具（已经自带了 ssh 客户端）了，另外还有一个图形界面的 Git 项目管理工具。  

&nbsp;  

#### 二、配置
配置帐号信息  
```
git config -e [--global] # 编辑Git配置文件

git config --global user.name yanhaijing
git config --global user.email yanhaijing@yeah.net

git config --list #查看配置的信息

git help config #获取帮助信息
```

配置自动换行（自动转换坑太大）  
```
git config --global core.autocrlf input #提交到git是自动将换行符转换为lf
```

配置密钥  
```
ssh-keygen -t rsa -C yanhaijing@yeah.net #生成密钥

ssh -T git@github.com #测试是否成功
```

配置别名，git的命令没有自动完成功能，有点坑哈，别名派上了用场  
```
git config --global alias.st status #git st
git config --global alias.co checkout #git co
git config --global alias.br branch #git br
git config --global alias.ci commit #git ci
```

&nbsp;  

#### 三、仓库  
```
git init #初始化
git status #获取状态
git add [file1] [file2] ... #.或*代表全部添加
git commit -m "message" #此处注意乱码
git remote add origin git@github.com:yanhaijing/test.git #添加源
git push -u origin master #push同事设置默认跟踪分支
git clone git://github.com/yanhaijing/data.js.git	
git clone git://github.com/schacon/grit.git mypro#克隆到自定义文件夹
```

&nbsp;  

#### 四、本地
```
git add * # 跟踪新文件
git add -u [path] # 添加[指定路径下]已跟踪文件

rm *&git rm * # 移除文件
git rm -f * # 移除文件
git rm --cached * # 停止追踪指定文件，但该文件会保留在工作区
git mv file_from file_to # 重命名跟踪文件

git log # 查看提交记录

git commit # 提交更新	
git commit [file1] [file2] ... # 提交指定文件	
git commit -m 'message'
git commit -a # 跳过使用暂存区域，把所有已经跟踪过的文件暂存起来一并提交
git commit --amend#修改最后一次提交
git commit -v # 提交时显示所有diff信息

git reset HEAD *#取消已经暂存的文件
git reset --mixed HEAD *#同上
git reset --soft HEAD *#重置到指定状态，不会修改索引区和工作树
git reset --hard HEAD *#重置到指定状态，会修改索引区和工作树
git reset -- files#重置index区文件

git revert HEAD #撤销前一次操作
git revert HEAD~ #撤销前前一次操作
git revert commit ## 撤销指定操作

git checkout -- file#取消对文件的修改（从暂存区——覆盖worktree file）
git checkout branch|tag|commit -- file_name#从仓库取出file覆盖当前分支
git checkout -- .#从暂存区取出文件覆盖工作区

git diff file #查看指定文件的差异
git diff --stat #查看简单的diff结果
git diff #比较Worktree和Index之间的差异
git diff --cached #比较Index和HEAD之间的差异
git diff HEAD #比较Worktree和HEAD之间的差异
git diff branch #比较Worktree和branch之间的差异
git diff branch1 branch2 #比较两次分支之间的差异
git diff commit commit #比较两次提交之间的差异

git log #查看最近的提交日志
git log --pretty=oneline #单行显示提交日志
git log --graph # 图形化显示
git log --abbrev-commit # 显示log id的缩写
git log -num #显示第几条log（倒数）
git log --stat # 显示commit历史，以及每次commit发生变更的文件
git log --follow [file] # 显示某个文件的版本历史，包括文件改名
git log -p [file] # 显示指定文件相关的每一次diff

git stash #将工作区现场（已跟踪文件）储藏起来，等以后恢复后继续工作。
git stash list #查看保存的工作现场
git stash apply #恢复工作现场
git stash drop #删除stash内容
git stash pop #恢复的同时直接删除stash内容
git stash apply stash@{0} #恢复指定的工作现场，当你保存了不只一份工作现场时。
```
&nbsp;  

#### 五、分支  
```
git branch#列出本地分支
git branch -r#列出远端分支
git branch -a#列出所有分支
git branch -v#查看各个分支最后一个提交对象的信息
git branch --merge#查看已经合并到当前分支的分支
git branch --no-merge#查看为合并到当前分支的分支
git branch test#新建test分支
git branch branch [branch|commit|tag] # 从指定位置出新建分支
git branch --track branch remote-branch # 新建一个分支，与指定的远程分支建立追踪关系
git branch -m old new #重命名分支
git branch -d test#删除test分支
git branch -D test#强制删除test分支
git branch --set-upstream dev origin/dev #将本地dev分支与远程dev分支之间建立链接

git checkout test#切换到test分支
git checkout -b test#新建+切换到test分支
git checkout -b test dev#基于dev新建test分支，并切换

git merge test#将test分支合并到当前分支
git merge --squash test ## 合并压缩，将test上的commit压缩为一条

git cherry-pick commit #拣选合并，将commit合并到当前分支
git cherry-pick -n commit #拣选多个提交，合并完后可以继续拣选下一个提交

git rebase master#将master分之上超前的提交，变基到当前分支
git rebase --onto master 169a6 #限制回滚范围，rebase当前分支从169a6以后的提交
git rebase --interactive #交互模式	
git rebase --continue# 处理完冲突继续合并	
git rebase --skip# 跳过	
git rebase --abort# 取消合并
```

&nbsp;  

#### 六、远端
```
git fetch origin remotebranch[:localbranch]# 从远端拉去分支[到本地指定分支]

git merge origin/branch#合并远端上指定分支

git pull origin remotebranch:localbranch# 拉去远端分支到本地分支

git push origin branch#将当前分支，推送到远端上指定分支
git push origin localbranch:remotebranch#推送本地指定分支，到远端上指定分支
git push origin :remotebranch # 删除远端指定分支
git push origin remotebranch --delete # 删除远程分支
git branch -dr branch # 删除本地和远程分支
git checkout -b [--track] test origin/dev#基于远端dev分支，新建本地test分支[同时设置跟踪]
```

&nbsp;  

#### 七、源  
git是一个分布式代码管理工具，所以可以支持多个仓库，在git里，服务器上的仓库在本地称之为remote。  
个人开发时，多源用的可能不多，但多源其实非常有用。
```
git remote add origin1 git@github.com:yanhaijing/data.js.git

git remote#显示全部源
git remote -v#显示全部源+详细信息

git remote rename origin1 origin2#重命名

git remote rm origin#删除

git remote show origin#查看指定源的全部信息
```
&nbsp;  

#### 八、标签
```
git tag  #列出现有标签

git tag v0.1 [branch|commit]  #[从指定位置]新建标签
git tag -a v0.1 -m 'my version 1.4'  #新建带注释标签

git checkout tagname#切换到标签

git push origin v1.5#推送分支到源上
git push origin --tags#一次性推送所有分支

git tag -d v0.1#删除标签
git push origin :refs/tags/v0.1#删除远程标签
```
&nbsp;  

#### 九、总结
其实还有两个最有用的命令还未提到。  
```
git help *#获取命令的帮助信息
git status#获取当前的状态，非常有用，因为git会提示接下来的能做的操作
```
最后再补一个救命的命令吧，如果你不小心删错了东西，就用下面的命令，可以看到你之前操作的id，大部分情况下是可以恢复的，记住git几乎不会删除东西。  
```
git reflog # 显示最近操作的commit id
```
&nbsp;  


