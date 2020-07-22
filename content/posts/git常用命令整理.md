---
title: git常用命令整理
id: 610
categories:
  - 开发笔记
date: 2016-03-21 18:20:13
tags:
  - git
---

*   拉取分支代码：`git clone -b [branch_name] [remote_url]`
*   查看所有远程分支：`git branch -a`
*   查看本地分支：`git branch`
*   创建分支：`git branch [branch_name]`
*   切换到分支：`git checkout [branch_name]`
*   删除本地分支：`git branch -d [branch_name]`
*   删除远程分支：`git push origin --delete [branch_name]`
*   拉取远程分支：`git fetch origin [branc_name]`
*   合并本地分支：`git merge [branch_name]`
*   撤销合并：`git reset --hard [commit_id]`
*   重命名本地分支：`git branch -m [old_branch_name] [new_branch_name]`
*   丢弃未提交的更改：`git clean -df`
*   丢弃所有已commit的修改：`git checkout .`
*   将更改暂存：`git stash`
*   拉取代码: `git pull`
*   恢复暂存：`git stash pop`
*   将远程仓库与本地仓库关联：`git remote add origin [remote_url]`
*   删除远程仓库与本地仓库的关联：`git remote rm origin`
*   将本地的代码推送到关联的远程仓库：`git push -u origin [branch_name]`
[http://developer.51cto.com/art/201512/502921.htm](http://developer.51cto.com/art/201512/502921.htm)
[http://www.kuqin.com/shuoit/20141213/343854.html](http://www.kuqin.com/shuoit/20141213/343854.html)