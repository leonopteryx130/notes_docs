# git常用命令

### git branch 

- ```git branch``` 命令可以查看本地仓库的分支
- ```git branch 分支名``` 可以切换到对应的分支，如果没有对应的分支，那么新建一个分支
- ```git branch -m 分支名``` 改变当前分支的分支名

### git checkout

- ```git checkout 分支名```  这个命令可以在git branch创建的分支之间进行切换
- ```git checkout -b 分支名```  这个命令可以创建一个新的分支，并切换过去
- ```git checkout .```  这个命令可以撤回当前分支工作区的所有改动，这里的撤回的前提是没有执行git add命令，本质相当于把暂存区覆盖到工作区
- ```git checkout --filename```  这个命令可以撤回指定文件工作区的改动，filename是文件名

### git merge 

- ```git merge 分支名```  可以将对应的分支合并到当前分支，注意merge后边的分支名是期望合并到自己当前分支里边的分支名
- ```git merge --abort```  撤销当前分支的merge。

### git remote

- ```git remote```查看远程主机名
- ```git remote -v```查看远程主机名及其具体的url

### git push

- ```git push```命令可以把本地仓库的代码推到远程仓库，更新远程代码
- ```git push --set-upstream 远程仓库名 本地分支名```  如果本地仓库的分支没有与远程仓库的分支建立连接，那么push的时候就需要用--set-upstream这个标志

### git reset 

- ```git reset``` 命令用于回退版本，可以指定退回某一次提交的版本。
- ```git reset --mixed``` mixed是git reset的默认值，该命令等同于git reset，作用是重置暂存区的文件与上一次的提交(commit)保持一致，工作区文件内容保持不变。相当于撤销git add，但是不改变编译器中写过的代码
