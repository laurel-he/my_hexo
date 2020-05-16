---
title: gitflow command version
date: 2019-04-24 21:03:10
tags: 工作
categories: git
---


## 新建feature/hotfix进行项目开发
------------------------
更新本地代码仓库缓存
`git remote update`
`git checkout develop`
`git merge origin/develop`
`git branch feature/20171012_hotel_enhance`

##补充
1 删除分支
git branch --delete --remotes origin/feature/20190214_monthly
git push origin :feature/20190214_monthly
2 add之后回退
git reset HEAD <file>
3 撤销commit 
git commit --amend
4 撤销已经commit但是没有push
git log 找到commit id
git reset --hard commit_id    完成撤销,同时将代码恢复到前一commit_id 对应的版本  
git reset commit_id      完成Commit命令的撤销，但是不对代码修改进行撤销，可以直接通过git commit 重新提交对本地代码的修改
## git fetch

