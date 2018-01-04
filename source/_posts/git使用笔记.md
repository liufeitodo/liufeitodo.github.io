---
title: git学习笔记
copyright: true
top: 0
date: 2017-12-01 23:32:48
tags: [学习笔记,git,GitHub]
categories: GitHub
description: git学习笔记：基础命令代码，错误提示&解决方法
---

## git学习笔记



### 基础命令代码
#### 查看版本	
git --version
#### 查看状态
  git status
#### 初始化git 仓库 将当前目录初始化成git 仓库
  git init
#### 添加.md文件到仓库  会先提交到缓冲区
  git add a.md
#### 提交 a.md文件到仓库  从缓冲区确认提交
  git commit -m "first commit"    //first commit 是标记信息
#### 查看所有commit 本地上传日志记录
  git log
#### 新建分支  
//git init 初始化时即有新建分支会拥有和master一样的内容
  git branch b1
#### 查看分支 
显示出的 * 位置 为当前可操作的分支
  git branch 
#### 切换分支  *在此位置
  git chockout b1  
##### 简化：新建并切换分支
  git chockout -b b2
#### 合并分支  
在b1分支上改动的项目合并到master分支上
  //git chockout master 
  git merge b1
#### 删除分支
   git branch -d b1
#### 强制删除分支 分支未合并 git branch -d b2 不是删除 
   git branch -D b2
#### 新建 、 查看标签
   git tag v1.0
   git tag      //查看
#### 生成密匙 SSH key 
   ssh-keygen -t rsa    
   指定 rsa 算法生成密钥，接着连续三个回车键（不需要输入密码），
   然后就会生成两个文件 id_rsa 和 id_rsa.pub ，而 id_rsa 是密钥，id_rsa.pub 就是公钥。这两文件默认分别在如下目录里生成：Linux/Mac 系统 在 ~/.ssh 下，win系统在 /c/Documents and Settings/username/.ssh
#### SSH key 添加成功后验证
   ssh -T git@github.com 
#### clone 项目到本地
   git clone git@github.com:liufeitodo/test.git
#### push 将本地项目更新同步到远程仓库  
   git push origin master
#### pull 将远程仓库更新同步到本地仓库  一般在push之前先pull ,不容易冲突
   git pull origin master 
#### 提交代码方法：
* 第一种：
  1. clone 下 GitHub 上已创建的项目Test 到本地，
     git clone git@github.com:liufeitodo/test.git 
  2. 在本地修改添加，之后commit再push
     git push origin master

* 第二种：将本地完整的git仓库（test2） 提交GitHub上的项目（test）中
  1. 关联本地项目test2 和Github 上的test项目，切换到test2目录，执行命令：
     git remote add origin git@github.com:liufei/test.git  // git@github.com:liufei/test.git 远程项目的仓库地址
  2. 提交代码 push
     git push origin master

#### alias 设置快捷命令
   如： git status  可以简写为  git st 
   修改代码：  git config --global alias.st status
   修改组合代码：  git config --global alias.psm 'push origin master'

#### diff 查看改动
   git diff <$id1> <$id2>   # 比较两次提交之间的差异
   git diff <branch1>..<branch2> # 在两个分支之间比较 
   git diff --staged   # 比较暂存区和版本库差异
#### stash  将没有commit 的内容暂存
* git stash
  意思就是把当前分支所有没有 commit 的代码先暂存起来，这个时候你再执行 git status 你会发现当前分支很干净，几乎看不到任何改动，你的代码改动也看不见了，但其实是暂存起来了
* git stash list
  提示暂存区有记录
* git stash apply
  将暂存的内容还原
* git stash drop
  将最近的一条stash记录删除掉
* git stash pop
  将暂存的内容还原并删除stash记录
  代替 apply 命令，pop 跟 apply 的唯一区别就是 pop 不但会帮你把代码还原，还自动帮你把这条 stash 记录删除，省的自己再 drop 一次了
* git stash clear
  清空所有暂存区的记录，drop 是只删除一条，当然后面可以跟 stash_id 参数来删除指定的某条记录，不跟参数就是删除最近的，而 clear 是清空。

#### merge & rebase 合并
   将b1 合并到master上
* merge: 强制合并，知道出处
  git chockout master
  git merge b1 
* rebase: 合并整理有逻辑，不知道出处
  git chockout master
  git rebase b1

* rebase 跟 merge 的区别:
  可以理解成有两个书架，你需要把两个书架的书整理到一起去，
  第一种做法是 merge ，比较粗鲁暴力，就直接腾出一块地方把另一个书架的书全部放进去，虽然暴力，但是这种做法你可以知道哪些书是来自另一个书架的；
  第二种做法就是 rebase ，他会把两个书架的书先进行比较，按照购书的时间来给他重新排序，然后重新放置好，这样做的好处就是合并之后的书架看起来很有逻辑，但是你很难清晰的知道哪些书来自哪个书架的。
  只能说各有好处的，不同的团队根据不同的需要以及不同的习惯来选择就好。

#### 解决冲突
   2人分别对相同内容改动，在各自合并时产生冲突，git无法辨别法判断你们两个谁更改的对，但是这个时候他会智能的提示有 conflicts ，需要手动解决这个冲突之后再重新进行一次 commit 提交

### 错误提示&解决方法
#### 本地没有README文件
错误提示：
```
  $ git push origin master
  To git@github.com:liufeitodo/test.git
   ! [rejected]        master -> master (non-fast-forward)
  error: failed to push some refs to 'git@github.com:liufeitodo/test.git'
  hint: Updates were rejected because the tip of your current branch is behind
  hint: its remote counterpart. Integrate the remote changes (e.g.
  hint: 'git pull ...') before pushing again.
  hint: See the 'Note about fast-forwards' in 'git push --help' for details.
<code><pre>
```
原因：
出现错误的主要原因是，github中的 README.md 文件不在本地代码目录中。
解决方法：
可以通过如下命令进行代码合并。[注：pull=fetch+merge]
git pull --rebase origin master
执行代码后可以看到本地代码库中多了README.md文件
再执行语句 git push -u origin master即可完成代码上传到github
