---
title: Git基本操作详解
date: 2016-06-15 11:27:58
tags:
  - 我的Blog
  - Git
categories:
- 日志
- 笔记
---

### 基础学习
    1. git init	初始化
    2. git add <filename> 添加文件到本地仓库  (把文件添加进去，实际上就是把文件修改添加到暂存区)
    3. git commit -m '<DESC>' 将添加好的文件提交到本地仓库 (把暂存区的所有内容提交到当前分支)
    4. git log 查看commit 日志
    5. git log --pretty=oneline 以行的形式显示日子
    6. git rest --hard HEAD^ 回滚到上一个版本
    7. git reset --hard <版本号> 回滚到该版本(可以往前回滚也可以往后回滚)
    8. git reflog 记录每一次命令，查看命令历史，以便确定要回到未来的哪个版本(可以用这个命令早点每次操作的版本号)
    9. git checkout -- <文件名> 文件在工作区的修改全部撤销 (
        一种是 该文件 自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
        一种是 该文件 已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
      )
    10. git checkout -- file命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到git checkout命令
    11. git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区
    12. git rm <文件名> 用于删除一个文件
    13. git remote add origin git@github.com:michaelliao/learngit.git 关联远程仓库(用ssh公钥的情况)
    14. git push -u origin master 推送数据到远程仓库(第一次推送数据需要加上  -u , master 最新改动需要推送的分支)


### git的分支应用
    1. git checkout -b dev 创建 dev分支 并切换到该分支 (-b 参数表示创建并切换)
    2. git branch 列出所有分支，当前分支前面会标一个*号
    3. git merge dev 合并指定分支[dev]到当前分支
    4. git branch -d dev 删除 指定分支[dev]
    5. git checkout master 切换到指定分支[master]
    6. git log --graph 查看到分支合并图
    7. git log --graph --pretty=oneline --abbrev-commit 看到分支的合并情况
    8. git merge --no-ff -m "merge with no-ff" dev 强制禁用Fast forward模式合并分支(-m参数，把commit描述写进去)
    9. git stash “储藏”当前工作现场
    10. git stash list 查看被储存的工作现场
    11. 恢复储存现场
      1. git stash apply 恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除
      2. git stash pop，恢复的同时把stash内容也删了
      3. git stash apply stash@{0} 恢复到指定储存记录
    12. git branch -D <name> 强行删除一个没有被合并过的分支<name>
    13. git remote 查看远程仓库
    14. git remote -v 查看远程仓库 详细信息
    15. git push origin master 推送分支到远程仓库上 被推送的分支为 master
    16. git checkout -b dev origin/dev 创建远程origin的dev分支到本地
    17. git pull 抓取最新的数据
    18. 多人协作的工作模式通常是这样：
      1. 首先，可以试图用git push origin branch-name推送自己的修改
      2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并
      3. 如果合并有冲突，则解决冲突，并在本地提交
      4. 没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功
      5. 如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name
### 标签管理(tag)
    1. git tag v1.0 打上一个name为 v1.0的标签, 默认为HEAD，也可以指定一个commit id
    2. git tag 查看标签
    3. git tag v0.9 6224937 在历史版本号为<6224937> 打上标签
    4. git show <tagname>查看标签信息
    5. git tag -a v0.1 -m 'version 0.1 released' ddcb78c  创建带有说明的标签，用-a指定标签名，-m指定说明文字 <ddcb78c 表示版本号>
    6. git tag -d v0.1 删除标签为 v0.1
    7. git push origin v1.0 推送标签到远程仓库上
    8. git push origin --tags 一次推送所有标签到远程仓库
    9. 删除远程仓库的标签
      1. git tag -d v0.9
      2. git push origin :refs/tags/v0.9
    10. git add -f <filename> 强制添加被gitignore忽略的文件到git
    11. git check-ignore -v <filename> 检查.gitignore文件哪个规则出错
