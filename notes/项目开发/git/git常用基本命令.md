# git常用命令

### git branch 

- git branch命令可以查看本地仓库的分支
- git branch 分支名：可以切换到对应的分支，如果没有对应的分支，那么新建一个分支
- git branch -m 分支名：改变当前分支的分支名

### git checkout

- git checkout 分支名  这个命令可以在git branch创建的分支之间进行切换
- git checkout -b 分支名  这个命令可以创建一个新的分支，并切换过去

### git merge 

- git merge 分支名，可以将对应的分支合并到当前分支，注意merge后边的分支名是期望合并到自己当前分支里边的分支名
- git merge --abort，撤销当前分支的merge。

### git remote

- git remote查看远程主机名
- git remote -v查看远程主机名及其具体的url

### git push

- git push命令可以把本地仓库的代码推到远程仓库，更新远程代码
- git push --set-upstream 远程仓库名 本地分支名。如果本地仓库的分支没有与远程仓库的分支建立连接，那么push的时候就需要用--set-upstream这个标志