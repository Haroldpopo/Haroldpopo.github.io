---
title: Git提交流程
date: 2021-05-10 13:00:00
categories: Git
tags: Git
---

# Git 提交一般流程

## 创建全局用户名、邮箱

```bash
# 只用一个账号配置全局用户较好
$ git config --global user.name "Your name"
$ git config --global user.email email@xx.com
```

## 初次创建仓库

### 克隆远程仓库到本地

```bash
# Gitee
$ git clone https://gitee.com/Harold_popo/Harold_popo.git

# Github
$ git clone https://github.com/Haroldpopo/Harold-Cnblogs-Theme-SimpleMemory.git
```

*github网站为国外网站，clone很慢，速度几kb/s不等，使用GitHub国内镜像网站clone速度更快*

```bash
# Github镜像网站github.com.cnpmjs.org
# 好像失效了
$ git clone https://github.com.cnpmjs.org/Haroldpopo/Harold-Cnblogs-SimpleMemory.git
```

```bash
# 镜像网站最新可用gitcode.net/mirrors
$ git clone https://gitcode.net/mirrors/Haroldpopo/Harold-Cnblogs-SimpleMemory.git
```

*注意：使用了镜像网站，如要推送到仓库则需更改关联库，否则会push失败*

```bash
$ git remote set-url --push origin  https://github.com/Haroldpopo/Harold-Cnblogs-SimpleMemory.git
```

### 直接建立本地仓库

```bash
# 输入内容并新建到README.md文件
$ echo "# Harold-Cnblogs-Theme-SimpleMemory" >> README.md

# 初始化仓库
$ git init

# 查看本地库的代码状态
$ git status

# 添加README.md文件到缓存区
$ git add README.md

# 提交文件到缓存区
$ git commit -m "First commit"

# 创建分支master
$ git branch -M master

# 关联远程仓库
$ git remote add origin https://github.com/Haroldpopo/Harold-Cnblogs-Theme-SimpleMemory.git

# 推送到远程仓库的master分支
$ git push -u origin master
```

## 查看分支

```bash
# 1、查看所有分支
$ git branch -a

# 2、- 查看当前使用分支(结果列表中前面标*号的表示当前使用分支)
$ git branch

# 3、切换分支
$ git checkout 分支名
```

## 推送到远程仓库

参数`-u`说明：指定将本地分支推送到origin主机，同时指定origin为默认主机，后面可不加此参数，直接`git push`

```bash
# 第一次将本地仓库推送到远程仓库
$ git push -u origin master
```

- 可能会推送失败，出现红色提示error报错，远程不为空存在README.md文件，所以需要先pull

```bash
$ git pull origin master
```

- 如果显示警告，可能是本地库与远程库有不相干的历史记录而无法合并，需加参数

```bash
# 同步远程仓库与本地仓库，消除文档间的差异，可能会修改合并文件，下载远程仓库的文件更新到本地
$ git pull origin master --allow-unrelated-histories

# 再次push提交
$ git push -u origin master
```

- 强制推送覆盖远程仓库

```bash
# 注：最好不要使用，团队项目的话可能会有生命危险
$ git push -f origin master
```
