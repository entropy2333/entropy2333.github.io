---
title: Git操作手册
date: 2022-03-05 23:01:44
tags:
- Git
category:
- 备忘
---

Git常见操作备忘

<!--more-->

## Resources

- documentation: <https://git-scm.com/docs>

## 常见操作备忘

### 使用Access Token添加远程分支

```shell
git remote add origin https://<access-token-name>:<access-token>@gitlab.com/myuser/myrepo.git
```

### 创建分支并提交

```shell
# 添加远程分支
git remote add origin git@git.sjtu.edu.cn/user/repo.git

# 创建本地分支
git checkout -b local_branch

# 创建空分支
git checkout --orphan local_branch

# push到远程分支
git push origin local_branch
```

### git-lfs

```shell
# 安装
brew install git-lfs

# 开启lfs功能
git lfs install

# 追踪大文件，提交时需要添加.gitattributes
git lfs track

# 追踪指定文件
git lfs track *.pdf

# 查看追踪的文件列表
git lfs ls-files
```

### 删除远程仓库不存在的分支

```sh
# 查看所有分支
git branch -a

# 只查看远程分支
git branch -r

# -p/--prune: 删除不存在的远程分支
git fetch -p
```

