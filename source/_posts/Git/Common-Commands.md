---
title: Git常用命令
sticky: true
date: 2021-05-11 17:05:26
categories: Git
tags: Git
---

# Git常用命令

- master：默认开发分支
- origin：默认远程版本库
- barnch：开发分支

## 创建版本库

```bash
# 克隆远程仓库到本地,默认下载项目的完整历史版本
$ git clone <url>

# 初始化仓库
$ git init
```

```bash
# 下载远程仓库项目最新的一次版本
$ git clone --depth=1 <url>

# 下载远程仓库指定的分支
$ git clone -b <branch> <url>

# 下载最新一次版本后，可获取完整历史信息
$ git fetch --unshallow
```

## 修改和提交

```bash
# 查看状态
$ git status

# 查看变更内容
$ git diff

# 跟踪指定文件添加至缓存区
$ git add <file>

# 跟踪所有改动的文件添加至缓存区
$ git add .

# 修改文件名
$ git mv <old> <new>

# 删除文件
$ git rm <file>

# 停止跟踪指定文件，但不删除文件
$ git rm --cached <file>

# 提交所有更新过的文件
$ git commit -m "commit message"

# 修改最后一次提交
$ git commit --amend
```

## 查看提交历史

```bash
# 查看提交历史
$ git log

# 查看指定文件的提交历史
$ git log -p <file>

# 以列表方式查看指定文件的提交历史
$ git blame <file>
```

## 撤销

```bash
# 撤销工作目录中所有未提交文件的修改内容
$ git reset --hard HEAD

# 撤销指定的未提交文件的修改内容
$ git checkout HEAD <file>

# 撤销指定的提交
$ git revert <commit>
```

## 分支与标签

```bash
# 显示当前分支
$ git branch

# 显示所有本地分支
$ git branch -a

# 创建新分支
$ git branch <new-branch>

# 切换指定的分支或标签
$ git checkout <branch/tag>

# 删除本地分支
$ git branch -d <branch>

# 列出所有本地分支
$ git tag

# 基于最新提交创建标签
$ git tag <tag>

# 删除标签
$ git tag -d <tag>
```

```bash
# 给指定的某个commit提交打标签，比如某次提交id为039bf8
$ git tag v1.0.0 039bf8

# or加注释
$ git tag v1.0.0 039bf8 -m "add tags info"

# or
$ git tag v1.0.0 -m "add tags info" 039bf8
```

## 合并与衍合

```bash
# 合并指定分支到当前分支，合并分支到新的branch commit分支节点，保留历史分支记录
$ git merge <branch>

# 衍合指定分支到当前分支，消除历史分支记录，直接合并分支，不建议用在主分支
$ git rebase <branch>
```

## 远程操作

```bash
# 查看远程版本库信息
$ git remote -v

# 查看指定远程版本库信息
$ git remote show <remote>

# 添加远程版本库
$ git remote add <remote> <url>

# 从远程库获取代码
$ git fetch <remote>

# 下载远程库代码并快速合并
$ git pull <remote> <branch>

# 上传代码至远程库并快速合并
$ git push <remote> <branch>

# 删除远程仓库分支或标签
$ git push <remote> :<branch/tag>

# 上传所有标签
$ git push --tags
```
