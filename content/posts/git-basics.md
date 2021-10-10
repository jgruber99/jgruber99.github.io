---
title: Git命令使用记录-基础篇
date: 2016-08-16
lastmod: 2017-04-09
draft: false
summary: Git基础命令的使用记录
description: 文章描述
categories: ["DevTools"]
tags: ["Git"]
---

# Git命令使用记录－来源自[官方文档][a01f5501]

## 1. Git起步

### 1.1 配置文件

> Git 自带一个 git config 的工具来帮助设置控制 Git 外观和行为的配置变量。 这些变量存储在三个不同的位置：
> 
* /etc/gitconfig 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果使用带有 --system 选项的 git config 时，它会从此文件读写配置变量。
- ~/.gitconfig 或 ~/.config/git/config 文件：只针对当前用户。 可以传递 --global 选项让 Git 读写此文件。
- 当前使用仓库的 Git 目录中的 config 文件（就是 .git/config）：针对该仓库。

#### 用户信息

> 设置用户名称与邮件地址

```shell
git config --global user.name "BruceMaa"
git config --global user.email BruceMaa@example.com
```

#### 文本编辑器

> 默认是VIM，可配置为emacs

```shell    
git config --global core.editor emacs
```

#### 检查配置信息

> 列出所有Git当时能找到的配置

```shell
git config --list
```

> Git会将三个位置的配置信息都列出来，所以可能会有重复，如果只想查看当前信息

```shell
git config user.name
```

> 打开当前仓库的配置文件

```shell
git config -e
```

### 1.2 获取帮助

> 若你使用Git时需要获取帮助，有三种方法可以找到Git命令的使用手册:

```shell
git help <verb>
git <verb> --help
man git-<verb>
``` 
    

> 例如，要想获得config命令的手册，执行

```shell
git help config
```

## 2. Git基础

### 2.1 获取Git仓库

#### 在现有目录中初始化仓库

> 初始化

```shell
git init
```

#### 克隆仓库

> 克隆

```shell
git clone [url]
```

#### 克隆仓库，自定义本地仓库名字

> 克隆，自定义本地仓库名字

```shell
git clone [url] [newRepo]
```

### 2.2 记录每次更新到仓库

#### 检查当前文件状态

```shell
git status  
```

#### 跟踪新文件，并将文件暂存

```shell
git add file
```
    
#### 交互式暂存

```shell
git add --interactive
```
等同于

```shell
git add -i
```

#### 状态简览

```shell
git status -s
```

> ?? 表示新添加文件，但未跟踪
>
> A 表示新添加文件，并放入暂存区中
>
> 左M 表示暂存区的文件已被修改，并放入暂存区中
>
> 右M 表示暂存区的文件已被修改，并未放入暂存区中

#### 忽略文件
> 创建.gitignore文件，列出要忽略的文件模式，可用正则表达式

#### 查看尚未暂存的文件更新了哪些部分

```shell
git diff
```

#### 查看已暂存的文件未提交的更新部分

```shell
git diff --staged
```

#### 提交更新
> 此命令提交，会显示core.editor设置的编辑器，填写提交信息

```shell
git commit
```

> 将提交信息于命令放在同一行

```shell
git commit -m "new commit"
```

> 不使用git add命令，将已跟踪过的文件暂存起来一并提交

```shell
git commit -a -m "new commit"
```

#### 移除文件

```shell
git rm file
```

> 记得从工作目录中删除，这样就不会出现在未跟踪文件清单中了,如果已经放到暂存区的话，则要用强制删除选项 -f

> 从仓库中删除，但本地保留

```shell
git rm --cached file
```

#### 移动文件

```shell
git mv file_from file_to
```

### 2.3 查看提交历史

```shell
git log
```

> -p 显示每次提交的内容差异，-2 显示最近两次提交

```shell
git log -p -2
```

> --stat 查看每次提交的简略的统计信息

```shell
git log --stat
```

> --pretty 格式化显示内容，内建子选项 oneline, short, full和fuller

```shell
git log --pretty=oneline
```

> 定制记录格式

```shell
git log --pretty=format:"%h - %an, %ar : %s"
```

> 显示精简SHA－1值

```shell
git log --abbrev-commit --pretty=oneline
```

#### 引用日志

```shell
git reflog
```
	
> 查看5次前所指向的提交

```shell
git show HEAD@{5}
```

> 查看master分支昨天指向哪个提交

```shell
git show master@{yesterday}
```
	
#### 父引用
> 需要在引用的后面加上一个^
	
>> HEAD的第一次父提交
> 
```shell
git show HEAD^
```  

>> HEAD的第二次父提交
>
```shell
git show HEAD^2
```   
	
>> HEAD的第一次父提交的第一次父提交
>
```shell
git show HEAD^^
```   

>> 等同于
>
```shell
git show HEAD~2
```   
	
#### 提交区间

##### 双点
> 查看experiment分支上有哪些提交尚未合并到master分支上

```shell
git log master..experiment
```   

> 查看在master分支中而不在experiment分支中的提交

```shell
git log experiment..master
```   

> 查看即将推送到远端的内容,下面两个相同

```shell
git log origin/master..HEAD
git log origin/master..
```
   

##### 多点
> 查看所有被refA或refB包含但是不被refC包含的提交，下面两个相同

```shell
git log refA refB ^refC
git log refA refB --not refC
```   

##### 三点
> 查看master或者experiment中包含，但不是两者共有的的提交

```shell
git log master...experiment
```   

> 还可以加参数--left-right，它会显示每一个提交属于哪一个分支

```shell
git log --left-right master...experiment
```	

#### format常用选项
> 
选项  |  说明
 :-- | :--
 %H  | 提交对象（commit）的完整哈希字串
 %h  | 提交对象的简短哈希字串
 %T  | 树对象（tree）的完整哈希字串
 %t  | 树对象的简短哈希字串
 %P  | 父对象（parent）的完整哈希字串
 %p  | 父对象的简短哈希字串
 %an | 作者（author）的名字
 %ae | 作者的电子邮件地址
 %ad | 作者修订日期（可以用 --date= 选项定制格式）
 %ar | 作者修订日期，按多久以前的方式显示
 %cn | 提交者(committer)的名字
 %ce | 提交者的电子邮件地址
 %cd | 提交日期
 %cr | 提交日期，按多久以前的方式显示
 %s  | 提交说明

> 当oneline或format与另一个log选项--graph结合使用时尤其有用，这个选项添加了一些ASCII字符串来形象的展示分支、合并历史：

```shell
git log --pretty=format:"%h %s" --graph
```   

#### git log 的常用选项

选项              |  说明
 :--              | :--
-p                | 按补丁格式显示每个更新之间的差异。
--stat            | 显示每次更新的文件修改统计信息。
--shortstat       | 只显示 --stat 中最后的行数修改添加移除统计。
--name-only       | 仅在提交信息后显示已修改的文件清单。
--name-status     | 显示新增、修改、删除的文件清单。
--abbrev-commit   | 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。
--relative-date   | 使用较短的相对时间显示（比如，“2 weeks ago”）。
--graph           | 显示 ASCII 图形表示的分支合并历史。
--pretty          | 使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式）。


#### 限制git log 输出的选项

选项             | 说明
  :--            | :--
  -(n)           | 仅显示最近的 n 条提交
--since, --after | 仅显示指定时间之后的提交。
--until, --before| 仅显示指定时间之前的提交。
--author         | 仅显示指定作者相关的提交。
--committer      | 仅显示指定提交者相关的提交。
--grep           | 仅显示含指定关键字的提交
-S               | 仅显示添加或移除了某个关键字的提交


> 如果要查看 Git 仓库中，2008 年 10 月期间，Junio Hamano 提交的但未合并的测试文件，可以用下面的查询命令：

```shell
git log --pretty="%h - %s" --author=gitster --since="2008-10-01" --before="2008-11-01" --no-merges 
```

### 2.4 撤销操作

#### 更改上次提交
> 上次提交后发现有漏提交文件，将漏提交文件放到暂存区，然后执行命令

```shell
git commit --amend
```

#### 取消暂存的文件

```shell
git reset HEAD <file>
```

#### 撤消对文件的修改

```shell
git checkout -- <file>
```
    
#### 撤消提交
> 撤消最近一次commit，数字可变。在这之后的commit全部舍弃

```shell
git reset --hard HEAD~1
```
	
> 撤消最近一次commit，数字可变，在这之后的commit都变成暂存状态，等待提交

```shell
git reset --soft HEAD~1
```
	
> 回滚最后一次提交

```shell
git reset --soft HEAD^
```

### 2.5 远程仓库

#### 查看原创仓库

```shell
git remote
git remote -v
```

#### 添加远程仓库

```shell
git remote add <shortname> <url>
```   

#### 从远程仓库拉取信息

```shell
git fetch <shortname>
```

#### 从远程仓库抓取信息然后合并远程分支到当前分支

```shell
git pull
```

#### 推送到远程仓库

```shell
git push [remote-name] [branch-name]
```

#### 查看远程仓库

```shell
git remote show origin
```

#### 重命名远程仓库

```shell
git remote rename <oldName> <newName>
```

#### 移除远程仓库

```shell
git remote rm <remote-name>
```

### 2.6 打标签

#### 列出标签

```shell
git tag
```

> 可以过滤结果

```shell
git tag -l 'v1.0.1*'
```

#### 创建标签

##### 附注标签

```shell
git tag -a v1.0 -m 'version 1.0'
```

##### 轻量标签

```shell
git tag v1.0-rc
```

#### 查看标签信息

```shell
git show <tagName>
```

#### 后期打标签

```shell
git log --pertty=oneline
```

> 然后在打标签的命令后面加上该次提交的校验值(可以是部分)

```shell
git tag -a V1.2 9fceb02
```

#### 共享标签

```shell
git push origin <tagname>
```

> 如果想要一次推送很多标签

```shell
git push origin --tags
```

#### 检出标签

```shell
git checkout -b <branchname> <tagname>
```
    
#### 删除标签

```shell
git push origin --delete tag <tagname>
```
	
> 或者，先本地删除标签，再将空标签推送到远程标签，等同于删除标签

```shell
git tag -d <tagname>
git push origin :refs/tags/<tagname>
```



### 2.7 Git 别名

```shell
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
```

## 3 分支

### 3.1 分支简介

#### 分支创建
> 在当前所在的分支基础上创建新分支

```shell
git branch testing
```

#### 分支切换

```shell
git checkout testing
```

#### 查看分叉历史

```shell
git log --oneline --decorate --graph --all
```

### 3.2 分支的新建与合并

#### 新建分支

```shell
git checkout -b iss53
```

> 等同于下面两条命令

```shell
git branch iss53
git checkout iss53
```

#### 分支的合并
> 将iss53分支合并到master

```shell
git checkout master
git merge iss53
```   

#### 合并时有冲突
> 修改冲突后，需要查看文件状态，把未提交的提交

```shell
git status
```   

#### 删除分支

```shell
git branch -d hotfix
```
    
### 3.3 分支管理

#### 分支列表

```shell
git branch
```
	
#### 查看每一个分支的最后一次提交

```shell
git branch -v
```
	
#### 查看哪些分支已经合并到当前分支

```shell
git branch --merged
```
	
#### 查看所有包含未合并工作的分支

```shell
git branch --no-merged
```
	
#### 强制删除未合并过的分支

```shell
git branch -D testing
```
	
#### 重命名本地分支

```shell
git branch -m <oldBranchName> <newBranchName>
```

### 3.3 远程分支

#### 查看远程仓库的远程分支

```shell
git ls-remote origin
```
	
#### 推送本地分支到远程仓库

```shell
git push (remote) (branch)
```
	
#### 推送本地分支到远程仓库，并且取不同名字

```shell
git push (remote) (local_branch):(new_branch)
```
	
#### 跟踪分支

```shell
git checkout --track origin/serverfix
```
	
#### 修改正在跟踪的上游分支

```shell
git branch -u origin/newBranch
```

或

```shell
git branch --set-upstream-to origin/newBranch
```
	
#### 查看所有跟踪分支

```shell
git branch -vv
```
	
> 如果有显示远程分支显示，并且有``ahead``提示，则有未推送到远程分支，有``behind``则表示远程服务器有提交未合并到本地，都没有则表示和远程分支同步；
> 如果没有显示远程分支，则表示未跟踪远程分支。
	
> 如果想查看最新情况，则需要先抓取所有远程仓库，再查看:

```shell
git fetch --all
```
	
#### 拉取

```shell
git pull
```
	
等同于

```shell
git fetch;git merge
```
	
#### 删除远程分支

```shell
git push origin --delete <delBranch>
```
	
或者，推送一个空分支到远程分支，等同于删除远程分支

```shell
git push origin :<delBranch>
```
	
#### 删除本地仓库中，远程已经删除的分支
> 远程删除了某个分支，本地执行抓取或者拉取时，并不会将本地对应的该分支也一并删除，使用

```shell
git remote show origin
```
	
> 命令查看时，正常的跟踪分支状态为``tracked``，已删除的分支状态为``stale``，并且Git提示可以使用命令删除

```shell
git remote prune
```
	
> 还有一种简单的命令

```shell
git fetch -p
```

### 3.4 变基
	
#### 变基的基本操作

> 检出新分支，coding

```shell
git checkout experiment
```
	
> 变基到想要合并的分支

```shell
git rebase master
```
	
> 回到master分支

```shell
git checkout master
```
	
> 快进合并

```shell
git merge experiment
```
	
#### 多重分支变基

> 从master分支创建server分支，再从server创建client分支，现在想先将client分支变基到master分支,再快进合并：

```shell
git rebase --onto master server client
git checkout master
git merge client
```
	 
> 再将server分支变基到master分支，再快进合并

```shell
git rebase master server
git merge server
```
	 
> 不需切换分支，直接变基某个分支

```shell
git rebase [basebranch] [topicbranch]
```
	 



  [a01f5501]: https://git-scm.com/book/zh/v2 "中文文档"