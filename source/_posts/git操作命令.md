---
title: git操作命令集合
categories: git
tags:
     - git
---
### 常用命令

克隆#  git clone 仓库地址
克隆指定分支#   git clone -b 分支名 仓库地址
切换到某个分支#  git branch 分支名
把修改文件添加到本地暂存区#  git add .
合并到当前分支#  git merge 分支名
提交修改文件#  git commit -m “修改内容”
更新#  git pull
提交到远程分支#  git push origin 分支名

<!-- more -->
查看/修改用户名和邮箱地址
$ git config user.name

$ git config user.email

$ git config --global user.name "username"

$ git config --global user.email "email"
