# 第2节：Git

## Git简介

* 创建版本库

  > 创建文件夹：mkdir 目录名
  >
  > 查看文件目录：pwd
  >
  > 创建版本号：git init

* 修改文件添加仓库

  > 添加仓库：git add 文件名
  >
  > 提交仓库：git commit -m "提示信息"

---

## 版本管理

* 状态查看

  > 查看修改文件：git status
  >
  > 文件修改内容：git diff

* 版本回退

  > 历史记录：git log
  >
  > 回退版本：git reset --hard Head^（^代表回退版本级别)  
  >
  > 回到未来：git reset --hard commitId 
  >
  > 查看命令记录：git reflog

* 撤销修改

  > 撤销赞存区的文件：git reset Head <file>
  >
  > 撤销工作区的文件：git checkout -- <file>

* 删除文件

  > 1. 先从文件管理器中删除：rm <file>
  > 2. 查看当前工作区状态：git status
  > 3. 确认要从版本库删除：git rm
  > 4. 再次提交：git commit

---

## 远程仓库

* 添加远程仓库

  > 关联远程：git remote add origin <origin name>
  >
  > 推送内容：git push -u origin master

* 克隆远程

  > 远程克隆：git clone <远程仓库地址>

---

## 分支管理

* 创建与切换分支

  > 创建分支：git branch <name>
  >
  > 切换分支：git checkout <name>
  >
  > 创建并且切换：git checkout -b <name>

* 分支列表

  > 分支列表：git branch

* 合并分支

  > 合并分支：git merge <name>
  >
  > 删除分支：git branch -d <name>

* 分支管理策略

  > 合并策略：git merge --no-ff -m "" <name>

* Bug分支

  > 储存工作现场WIP：git stash
  >
  > 查看存储的工作现场：git stash list
  >
  > 恢复工作现场：git stash pop
  >
  > 恢复指定的工作现场： git stash apply <name>

* Feature分支

  > 强行删除分支：git branch -D <name>

* 多人协作

  > 查看远程库信息：git remote -v
  >
  > 本地推送分支：git push origin branch-name
  >
  > 本地创建和远程分支对应的分支：git checkout -b branch-name origin/branch-name
  >
  > 本地分支和远程分支关联：git branch --set-upstream branch-name origin/branch-name
  >
  > 从远程抓取分支：git pull

---

## 标签管理

* 创建标签

  > 1. 切换到需打tag的分支上：git checkout <name>
  > 2. 打上标签：git tag <tag-name>
  > 3. 指定commit打tag：git tag <tag-name> <commit-id>
  > 4. 制定标签信息：git tag -a <tag-name> -m ""

* 操作标签

  > 删除标签：git tag -d <tag-name>
  >
  > 推送某标签：git push origin <tag-name>
  >
  > 一次推送所有本地标签：git push origin --tags
  >
  > 删除远程标签：git tag -d <tag-name> 和git push origin:refs/tags/v0.9