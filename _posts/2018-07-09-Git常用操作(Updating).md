---
layout: post
title: Git常用操作(Updating)
date: 2018-07-09
tags: 程序猿的自我修养
---

　十多年前，版本控制服务提供商BitMover决定停止BitKeeper对Linux核心开发的支持，顿时Linux核心开发受到严峻的挑战，Linus Torvalds大神整个周末不见人影，隔周却如变戏法般的带着Git出现，十多年后的今天，随着版本控制托管平台GitHub以75亿美元被Microsoft收购，Git早已经发展成为目前世界上最先进的分布式版本控制系统（没有之一）

### 创建版本库

- **把当前目录变成Git可以管理的仓库**

```
git init
```

- **把当前本地仓库关联远程仓库**

```
git remote add origin git@github.com:summerffly/BriCpp.git
```

### 克隆远程仓库

- **使用ssh协议克隆远程仓库**

```
git clone git@github.com:summerffly/BriCpp.git
```

- **使用https协议克隆远程仓库**

```
git clone https://github.com/summerffly/BriCpp
```

- **克隆远程分支**

```
git branch -r   // 查看远程分支
git checkout url
git checkout -b BRANCHNAME
```

### 版本管理

- **查看文件修改状态**

```
git status
```

- **查看版本历史提交记录**

```
git log
```

- **回退到某一个commitid的版本**

```
git reset --hard commitid
```

- **回退到某一个版本，HEAD表示最新版本，HEAD^表示最新的前一次版本，以此类推**

```
git reset --hard HEAD
git reset --hard HEAD^^
```

### 分支管理

- **上传本地项目的所有变化**

```
git add -A   // 提交所有变化
git add -u   // 提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)
git add .   // 提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件
```

### 本文参考链接

[廖雪峰的Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)


