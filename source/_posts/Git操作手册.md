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

## 配置

```bash
# 查看
git config user.name
git config user.email

# 修改（局部）
git config user.name ${your_name}
git config user.email ${email@example.com}

# 修改（全局）
git config --global user.name ${your_name}
git config --global user.email ${email@example.com}
```

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

```shell
# 查看所有分支
git branch -a

# 只查看远程分支
git branch -r

# -p/--prune: 删除不存在的远程分支
git fetch -p
```

### 合并多次提交

```shell
# 合并最近的n次提交
git rebase -i HEAD~n

# 合并当前HEAD到指定commit id
git rebase $commit-id
```

默认编辑器为nano，可以用`git config --global core.editor "vim"`修改为vim。

## 修改历史提交信息

```bash
git filter-branch -f --env-filter '
OLD_EMAIL="old_email"
CORRECT_NAME="your_name"
CORRECT_EMAIL="email@exmaple.com"
if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

