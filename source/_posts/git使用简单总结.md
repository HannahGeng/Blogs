---
title: git使用简单总结
date: 2017-01-16 23:07:02
tags: iOS学习
---

**几个概念：**

工作区：仓库文件夹里除.git目录以外的内容

版本库：.git目录，用于存储记录版本信息

暂缓区（stage）

分支（master）：git自动创建的第一个分支

HEAD指针：用于指向当前分支

    注：显示隐藏文件命令：
    defaults write com.apple.finder AppleShowAllFiles -bool true(false)

#### 一、个人演练（命令行演练）
1. 进入到工作目录中，初始化一个代码仓库：git init

2. 给该git仓库配置一个用户名和邮箱：
git config user.name "gengbinghan"
git config user.email "9777@qq.com"

3. 初始化代码：
touch main.m -> git status -> Untracked files:（红色文件）：新添加或者新修改的文件在工作区中，没有被添加到暂缓区中
git add main.m -> git status -> Changes to be committed:（绿色文件） :将工作区的代码已经添加到暂缓区，可以提交到代码仓库中了

4. 查看文件的状态：git status

    * Untracked files:(红色文件)：新添加或者新修改的文件在工作区中，没有被添加到暂缓区中
    
    * Changes to be committed:（绿色文件） :将工作区的代码已经添加到暂缓区，可以提交到代码仓库中了

5. git commit -m "xxxx"（提交代码）
[master (root-commit) 949712f] 初始化项目
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 main.m

6. 修改文件：
open main.m

7. git add .
将工作区所有的文件添加到暂缓区里面

8. 给git命令其别名:
git config alias.要起的别名 “要给谁其别名”
例: git config alias.ci "commit -m"

9. 查看历史版本
git  log
*git 版本号：61d5d0c23e7e1b772c32829ed2feb130728fae66
*git版本号是由sha1加密算法生成的一个40位的哈希值
git reflog

10. 版本回退
git reset --hard HEAD：回退当前版本（当前版本未提交）
git reset --hard 版本号

11. --global的作用（配置全局的用户名和密码，其他地方可以不配置）
    
    * git config --global user.name "gengbinghan"
    
    * git config --glabal user.email "9777@qq.com"

#### 二、团队开发（共享版本库）

文件夹/U盘/Github/oschina

1. 创建一个代码共享库（让一个文件夹成为共享库）
git init -- bare

2. 经理将我们的共享代码仓库中的内容clone下来
git clone 地址

3. 项目经理初始化项目
* 忽略文件：在我们的.git 同级目录下创建一个.gitignore文件，在该文件中指定需要忽略的文件
* github上搜索：.gitignore -> 打开ObjectiveC -> 复制文件内容到新建的.ignore文件中(https://github.com/github/gitignore/blob/master/Objective-C.gitignore)
* git add . -> git commit --m "注释"，将.gitignore添加到本地仓库管理中

4. OSChina和Github的使用

