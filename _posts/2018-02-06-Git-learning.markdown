---
layout:     post
title:      "Git 学习"
subtitle:   "Git 实例与常用指令 "
date:       2018-02-06
author:     "Xiaotong CHEN"
header-img: 
catalog:
tags:
    - Git
---

* 目录
{:toc}

# 基础
*以在GitLab上创建一个project为例*

## 1. SSH秘钥
生成SSH秘钥指令：
`$ ssh-keygen -t rsa -C "votre.email@telecom-sudparis.eu" -b 4096`
保存在`$HOME/.ssh/id_rsa.pub`
将获得的SSH秘钥填入GitLab网站
**SSH秘钥用于GitLab网站识别Unix账号**

## 2. 创建project并添加合作者
命名规则：课程+年份+创作者
*给合作者Master权限*

## 3. 克隆project到本地
`git clone git@gitlab.tem-tsp:votreprenom.votrenom/votrenomdeproject.git`

## 4. 查看git状态
`git status`

## 5. 创建第一个文件README.md并提交
1. `git add README.md`
2. `git commit -m "注释"`
3. `git push origin master`
4. 其他合作者`git pull origin`

- git add：
i） `git add .` 或 `git add --all` 提交所有修改

- git commit：
i） `git commit`后会生产一个hash码，用于标记此次commit操作

- git push：
i） `git push`默认将master上传
ii）origin是远程仓库名，master是分支名

- git pull：
i）当有其他人想master分支推送了更新，则服务器上的master向前推进，本地的Master落后服务器版本，需要运行 `git fetch`来同步本地并用`git merge`合并，或直接用`git pull`。`git pull`=`git fetch` + `git merge`

---

# 分支操作
*branch*

## 1. 创建分支
`git checkout -b module1`

i） 创建并转到module1分支
ii）`git checkout 分支名` 转到已存在的分支

## 2. 查看分支
`git branch`

i）一般而言，Master分支是默认创建的。
**但若是在项目一开始，即项目为空时，创建分支，会丢失master分支。**

## 3. 在分支上进行操作
- 与在[基础部分](# 例子1-基础)操作相同
- 在每个分支上做的add,commit操作是独立的

## 4. 查看日志
- `git log --graph --oneline --decorate`
- 使用工具gitg/gitk

## 5. 合并（Fusion）分支
1. 转到Master`git checkout master`
2. `git merge --no-ff moudle1 -m "Merge branch 'modele1'"`

i)  有`--no-ff`则moudle1会保留，没有则不保留module1分支

---

# 冲突conflict

## 设置冲突管理工具
`git config merge.tool meld`

## 冲突前提
分支module1和Master中有同一个文件，且分别在不同分支修改了文件，并做了`add commit`操作。做第二次`commit`时冲突发送。

解决方法：
用`git mergetool`调用meld，查看修改冲突的地方，修改保存后会生成很多不必要的文件
用`rm 文件名.文件类型.* 文件名_* *~`删除这些文件。

---

# Git-flow

## Git-flow原则
1. 两条长期分支
**master**只保留官方的，实用的代码版本
**develop**在这个分支进行开发，只有想正式对外发布且经过确认，才会和Master合并。
- 新建develop分支：`git checkout -b develop master`
- 将develop分支发布到Master分支：`git checkout Master`
                                 `git merge --no-ff develop `
2. 创建release(预发布)分支
当要进行develop分支和Master合并时，先从develop分支上创建一个release分支，加上标签（tag)用于标记新的版本。
*建议创建一个release分支，这个分支里commit只用于修改bugs。*

3. 在develop的子分支（feature分支）上编码
只有子分支状态满足，develop上的合并操作才会执行

4. hotfix
当master上发现bug，就在master上引出一个hotfix分支。
这个分支有两个作用：
1. 为Master提供一个新版本
2. 为develop修复整个project的bug

*注意：*release,feature和hotfix分支都是临时分支，使用完后，应该删除，使代码库的常设分支始终只有Master和Deve。

![Git-flow图](/img/git/git-model.png)


## Git-flow例子
1. 在本地建立工作文件夹，初始化库

```
mkdir exercice-git-flow  
cd exercice-git-flow  
git init  
touch README.md  
git add README.md  
git commit -m "commit initial"
```

2. 新建develop分支
`git checkout -b develop`

3. 新建feature分支，以功能名命名
`git checkout -b navire1`

4. 在这个分支中写一个文本文档

```
gedit navire.txt  
git add navire.txt  
git commit -m "navire version 1`
```

5. 修改和二次提交
为navire.txt中加入新的几行

```
gedit navire.txt  
git add navire.txt  
git commit -m "navire version 2"
```

现在我们已经实现了一定功能，合并这个分支和develop

```
git checkout develop  
git merge --no-ff navire1 -m "Merge branch 'navire1' into develop"
```

6. 假设经过多次3-5，我们已经实现了很多功能，想把develop和master合并

```
git checkout master  
git merge --no-ff develop -m "commit release 1.0"
```

因为这是一个正式版本，我们加上标签tag
`git tag -a v1.0 -m "release 1.0"`

# 版本控制
=有待完成=
