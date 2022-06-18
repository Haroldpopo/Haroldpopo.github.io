---
title: Git同时配置Github和Gitee
date: 2021-06-01 13:05:26
categories: Git
tags: Git
---

# Git同时配置Github和Gitee

## 配置

### 清除全局配置

- 查看是否设置过全局配置

```bash
$ git config --global --list
```

- 清除全局配置（如果无可跳过）

```bash
$ git config --global --unset user.name "Your Name"
$ git config --global --unset user.email You@example.com
```

### 创建ssh公钥

`ssh-keygen`说明

- 参数`-t`：公钥类型，如**rsa**、**dss**
- 参数`-C`：注释，为了添加一个可辨认的标识，以便识别
- 参数`-f`：公钥保存文件路径，及文件名可自行取名，如`-t`为**rsa**则公钥保存文件默认为**id_rsa**

```bash
$ ssh-keygen -t rsa -C "xxx@qq.com" -f ~/.ssh/github_id_rsa
# 接下来提示设置密码，无需设置密码，直接回车

$ ssh-keygen -t rsa -C "xxx@qq.com" -f ~/.ssh/gitee_id_rsa
```

***如没有反应，可能没有添加环境变量***，添加环境变量`Git安装目录/usr/bin`

在`~/.ssh/`目录下生成以下文件

- `github_id_rsa`
- `github_id_rsa.pub`
- `gitee_id_rsa`
- `gitee_id_rsa.pub`

### 识别SSH key新的密钥

默认只读取 `id_rsa`，为了让 SSH 识别新的私钥，需要将新的私钥加入到 `SSH agent` 中

```bash
$ ssh-agent bash
$ ssh-add ~/.ssh/github_id_rsa
$ ssh-add ~/.ssh/gitee_id_rsa
```

### 多账号配置config文件

1、创建config文件

```bash
$ touch ~/.ssh/config
```

2、config 中填写的内容，可用vim编辑器

```bash Git command:("$":1)
vim ~/.ssh/config
# Gitee
Host gitee.com
    HostName gitee.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/gitee_id_rsa

# GitHub
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/github_id_rsa
```

### 添加ssh

- [https://github.com/settings/keys](https://github.com/settings/keys)，将 `github_id_rsa.pub` 中的内容填进去，起名的话随意。


- [https://gitee.com/profile/sshkeys](https://gitee.com/profile/sshkeys)，将 `gitee_id_rsa.pub` 中的内容填进去，起名的话随意。


### 测试ssh连接

```bash
$ ssh -T git@github.com
$ ssh -T git@gitee.com
```

第一次连接会提示输入`yes/no`，输入`yes`即可

## 不同平台共用一个本地仓库

### 关联远程仓库

```bash
# Gitee
$ git remote add gitee git@gitee.com:xxx/xxx.git

# Github
$ git remote add github git@github.com:xxx/xxx.git
```

```bash
# example
$ git remote add github git@github.com:Haroldpopo/Blog.git
$ git remote add gitee git@gitee.com:Harold_popo/Harold_popo.git
```

### 推送远程仓库

```bash
# 先pull拉取远程仓库的文件更新到本地仓库
$ git pull gitee master

# 如果出现![rejected] master -> master(non-fast-forward) error:failed to push some refs to XXX，则
# git pull gitee master --allow-unrelated-histories

# Gitee
$ git push gitee master

# Github
$ git push github master
```

### 提交代码仍需用户名、邮箱

多平台账号下全局配置已清除，提交代码如果还需要我们的用户名、邮箱

在我们的工作目录（项目）里配置即可：

```bash
# Github
$ git config user.name 'Haroldpopo'
$ git config user.email 2417525822@qq.com

# Gitee
$ git config user.name 'Harold'
$ git config user.email 2417525822@qq.com
```

![vgy.me](https://i.vgy.me/WEgGEr.png)



### 使用流程

以Gitee的Harold_popo/Harold_popo.git为例

```bash
# 初始化当前仓库
$ git init

# 当前工作目录（项目）创建用户名、邮箱
$ git config user.name "Harold"
$ git config user.email 2417525822@qq.com

# 关联远程仓库
$ git remote add gitee git@gitee.com:Harold_popo/Harold_popo.git

# 同步远程仓库到本地仓库
$ git pull gitee master
# git pull gitee master --allow-unrelated-histories

# 查看所有分支
$ git branch -a

# 查看缓存区状态
$ git status .

# 将所有已添加、已删除或已修改文件添加至缓存区
$ git add .

# 发送提交信息
$ git commit -m "commit test"

# 推送到远程仓库
$ git push gitee master
```

