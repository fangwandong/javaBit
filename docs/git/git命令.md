#简介
> 记录下git命令


## 创建分支：
- origin/分支名： 远程分支

```bash
# 基于远程分支master创建分支
git checkout origin/master  -b fangwandong_dev

# 推送本地分支到远程
git push  origin  fangwandong_dev

# 关联本地分支和远程分支：
git branch --set-upstream-to=origin/fangwandong_dev fangwandong_dev

```

## 在本地创建分支
- 基于当前的分支，创建的分支

```bash
git checkout -b  <branchName>
```

## 查看远程分支

```bash 
git branch -a
```

## 删除本地分支
- -D 强制删除

```bash
git branch -d <branchName>
```

## 删除远程分支
```bash

git push origin --delete <branchName>

```

## 更新远程分支
- 如果你的remote branch不是在origin下，按你得把origin换成你的名字。

```bash
git remote update origin --prune

```

