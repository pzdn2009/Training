# Git-合并两个不同的仓库

* 添加远程源 
* 拉取代码
* 迁出分支
* 迁出本地的分支
* merge
* 冲突解决
* 是否删除远程分支

远程仓库：originB
```
# 添加originB
git remote add originB git@github.com:pzdn2009/XXX.git

# 拉取代码
git fetch originB
# 迁出分支
git checkout -b OBfeature-1.0 originB/feature-1.0

# 迁出本地分支
git checkout localBranch

# 将远程分支合并到本地分支
git merge OBfeature-1.0

# 解决冲突
git add .
git commit -m "remote to local"
git branch -d OBfeature-1.0
```