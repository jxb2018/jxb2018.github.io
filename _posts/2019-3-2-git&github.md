---
layout:     post
title:      Git命令行操作
subtitle:   使用Git进行版本控制
date:       2019-3-2
author:     jxb2018
header-img: img/post-git&github.png
catalog: 	 true
tags:
    - Git
    - GitHub
---
#### 一.本地库初始化

```
git init
```
#### 二. 设置签名
- **方式一：项目级别\仓库级别**

```
git config user.name jidianxiaobai
git config user.email 1369375419@qq.com
```
==注意==：信息保存在 ```./.git/config```文件中

- **方式二：系统用户级别**

```
git config --global user.name jidianxiaobai
git config --global user.email 1369375419@qq.com
```
==注意==：信息保存在 ```~/.gitconfig```文件中

#### 三. 基本操作
##### 1.状态查看 --> 查看==工作区、暂存区==状态 

```
git status
```
##### 2.添加 --> 将==工作区==的"新建/修改"添加到==暂存区==
```
git add [filename]
```
##### 3.提交 --> 将暂存区的内容提交到本地库
```
git commit -m "commit message" [filename]
```
##### 4.查看历史纪录
-  ```git log```
-  ```git log --pretty=oneline```
-  ```git log --oneline```
-  ```git reflog```
##### 5.版本前进后退
- 本质
 
插入图片
- 基于索引值操作

```
git reset --hard [局部索引值]
```

- 使用^符号，只能后退(==有几个回退几步==)

```
git reset --hard HEAD^
```

- 使用~符号，只能后退(==n表示回退n步==)
```
git reset --hard HEAD~n
```
==***注：reset命令的三个参数比较***==

参数 | 作用 | 效果
---|---|---
--soft | 仅仅在本地库移动HEAD指针
--mixed | 在本地库移动HEAD指针、重置暂存区
--hard | 在本地库移动HEAD指针、重置暂存区、重置工作区

##### 6.删除文件并找回
- 前提：删除前，文件存在时的状态已经提交到了本地库
- 操作：``` git reset --hard [指针位置] ```
  - 删除操作已经提交到本地库：指针位置指向历史记录
  - 删除操作尚未提交到本地库：指针位置使用HEAD
##### 7.比较文件差异
- 将工作区中的文件和暂存区进行比较
 ```
git diff [文件名] 
```
- 将工作区中的文件和本地库历史记录比较
 ```
git diff [本地库中的历史版本] [文件名]
```
- 不带文件名比较多个文件
``` 
git diff 
```
##### 8.推送 
###### 方式一：每次都需要输入用户名密码
- 查看当前所有远程地址别名 
``` 
git remote -v 
``` 
- 创建别名
```
git remote add [别名] [远程地址]
```
- 推送
```
git push [别名] [分支名]
```
- 输入用户名及密码
###### 方式二：修改配置文件，一劳永逸
- 修改```./.git/config ```文件，
```
[remote "origin"]
url = https:// username:password@github.com/jidianxiaobai/testgit.git
fetch = +refs/heads/*:refs/remotes/origin/*
```
- ``` git push origin [分支名] ```
###### 方式三：ssh登陆
- 在用户家目录下运行命令生成``` .ssh ```密钥目录
```
cd ~
ssh-keygen -t rsa -C [标记]
```
- 进入``` .ssh ```目录下,查看``` id_rsa.pub ```的文件内容
```
cat id_rsa.pub
```
- 复制文件内容，在github上创建key
```
graph LR
登陆GitHub-->目标仓库
目标仓库-->Settings
Settings-->Deploy_keys
Deploy_keys-->add_deploy_key
add_deploy_key-->Add_key
```
- 创建远程仓库别名
```
git remote add origin_ssh git@github.com:用户名/仓库
```
- 推送
```
git push origin_ssh [分支]
```
##### 9.拉取 ==(pull=fetch+merge)==
```
git fetch [远程仓库别名] [远程分支名]
git merge [远程地址别名/远程分支名]
git pull [远程仓库别名] [远程分支名]

```
##### 10.克隆
```
git clone [远程地址]
```
#### 四. 分支管理
##### 分支管理概念

##### 分支操作
- ==创建==分支
```
git branch [分支名]
```
- ==查看==分支
```
git branch -v
```
- ==切换==分支
```
git checkout [分支名]
```
- ==合并==分支
  1. ``` git checkout [要合并在的分支] ```
  2. ``` git merge [要被合并的分支] ```
 

- 解决冲突
  1. ``` vim [filename] ``` ,删除特殊符号
  2. 编辑满意之后保存退出
  3. ``` git add [文件名] ```
  4. ``` git commmit -m "commit message" ```