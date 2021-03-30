# Git操作

## 版本管理

版本管理是一种记录文件的方式，以便将来查阅特定版本的文件内容

人为维护的问题：

1. 文档数量多且命名不清晰导致文档版本混乱
2. 每次编辑文档需要复制，不方便
3. 多人同时编辑同一个文档，容易产生覆盖

## Git是什么

Git是目前世界上最先进的分布式版本控制系统

## Git的相关配置

1. 配置提交人姓名：git config --global user.name "your name"

2. 配置提交人邮箱：git config --global user.email "your email"

3. 查看配置信息：    git config --list
4. 配置命令别名：    git config --global alias.自定义的命令别名 命令
5. 配置命令行界面：git config --global color.ui true
6. 撤销暂存区修改：git config --global alias.unstage 'reset HEAD'
7. 显示最后一次提交：git config --global alias.last 'log -1'

因为Git是分布式版本控制系统，所以每台机器都必须配置：名字和邮箱

如果要修改配置信息，重复命令即可

## 创建版本库

版本库又名仓库，英文名repository，可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能够跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

在合适的位置，创建一个空目录（在Windows系统上，为了避免各种莫名奇妙的问题，请确保目录名不包含中文，图片、视频、word文件无法被追踪）

## 版本库文件相关操作命令

### 基础操作

1. 初始化Git仓库：               git init     	   

2. 查看文件的状态：            git status  

3. 查看文件具体修改内容： git diff file                  

4. 文件追踪：                        git add file                      

5. 全部文件追踪：                git add .                          

6. 向仓库中提交：                git commit -m "提交说明"

7. 查看提交记录：                git log 或者 git log --pretty=oneline

   提交记录中有每一次提交版本的commit_id

### 版本回退

在Git中用HEAD表示当前版本，上一个版本是HEAD^ ，上上个版本是HEAD^^ ,往上100个版本是HEAD~100

1. 版本回退：                            git reset --hard HEAD^ 或者 git rest --hard commit_id（这个commit_id不一定要写全）
2. 查看回退后文件：                cat file
3. 查看输入过的每一次命令： git reflog（后悔药，版本切换时有大用处，重返未来）

### 工作区和暂存区

工作区（Working Directory）：在电脑里能看到的目录

版本库（Repository）：工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD` 

git add 将要提交的所有修改放到暂存区

git commit 将暂存区的所有内修改提交到当前分支

### 管理修改

Git跟踪并管理的是修改，而非文件

只会提交暂存区中的所有修改

每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中

查看工作区和版本库里面最新版本的区别：git diff HEAD -- file

### 撤销修改

1. 丢弃工作区的修改：git checkout -- file

   让这个文件回到最近一次 `git commit` 或 `git add` 时的状态

2. 把暂存区的修改撤销：git reset HEAD file

### 删除文件

版本库中有文件，工作区中删除了该文件且确实要删除

1. 删除文件：git rm file
2. 提交：git commit -m "提交说明"

误删文件

1. 恢复文件到最新版本：git checkout -- file（会丢失最近一次提交后修改的内容）

## 远程仓库

创建SSH Key： 在Git Bash下 ssh-keygen -t rsa -C "邮箱"

找到 .ssh文件，将id_rsa.pub的文件内容粘贴到Github的”Account setting“的”Add SSH Key“

### 添加远程仓库

在本地初始化一个仓库，在Github上创建一个相同的仓库

1. git remote add origin 远程仓库地址（origin是默认习惯名称）

2. git push -u origin master
3. git push origin master
4. git push

之后把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送到远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

### 删除远程仓库

1. 查看远程仓库信息：git remote -v
2. 删除：git remote rm origin（仅仅是解除本地仓库与远程仓库的联系）

### 从远程仓库克隆

在Github上创建好远程仓库后

1. git clone 远程仓库地址

## 分支管理

### 分支相关命令

- 查看分支：                    git branch     
- 查看分支：                    git branch --list  
- 查看包含远程的分支： git branch -a 
- 创建分支：                     git branch 分支名称     
- 切换分支：                     git checkout 分支名称
- 切换分支：                     git switch 分支名称（推荐，易理解）    
- 创建并进入分支：         git checkout -b 分支名称 
- 创建并进入分支：         git switch -c 分支名称（推荐，易理解）
- 合并分支：                     git merge 来源分支     
- 删除分支：                     git branch -d 分支名称（分支被合并后才允许删除）（-D强制删除）

### 解决冲突

1. 查看冲突文件：git status

2. 查看分支合并情况：git log --graph --pretty=oneline --abbrev-commit
3. 手动修改冲突的文件

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图

### 分支管理策略

准备合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward` 

* git merge --no-ff -m "merge with no-ff" dev

因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去

* git log

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

### Bug分支

* 在当前未完成的分支上：git stash

* 修复完其他分支的Bug后在未完成的分支上查看stash内容：git stash list

* 需要恢复未完成的分支：
  * git stash apply恢复后，删除stash内容git stash drop
  * 恢复的同时删除stash内容git stash pop

* 恢复指定的stash： git stash apply stash@{0}
* 未完成的分支上也会存在Bug
  * 复制一个特定的提交到当前分支：git cherry-pick 特定提交的id

### Rebase

* git rebase

- rebase操作可以把本地未push的分叉提交历史整理成直线；
- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比

## 标签管理

### 创建标签

* 创建一个标签：git tag 标签名（v 1.0)
* 查看标签：git tag
* 对历史提交打标签：
  * 查看提交记录：git log --pretty=oneline --abbrev-commit
  * 通过commit id为历史提交打上tag： git tag 标签名 commit_id
  * 查看标签：git tag
  * 查看标签详细信息：git show 标签名
  * 创建带说明的标签：git tag -a 标签名 -m "标签说明文字" commit_id

### 操作标签

* 未推送到远程时删除标签：git tag -d 标签名
* 推送到远程：git push origin 标签名
* 全部推送：git push origin --tags
* 如果标签已经推送到远程：
  * 先删除本地标签：git tag -d 标签名
  * 然后从远程删除：git push origin :refs/tags/标签名

## 忽略文件

.gitignore文件编写规则，需推送到远程

强制添加文件到Git： git add -f file 

检查规则：git check-ignore -v file

































































































