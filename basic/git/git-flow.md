# Git-flow

* big picture
* master
* feature
* develop
* release
* hotfix
* tools

# 1. big picture

![](/assets/git-flow.png)

* master。这个分支最近发布到生产环境的代码，最近发布的Release， 这个分支只能从其他分支合并，不能在这个分支直接修改
* develop。这个分支是我们是我们的主开发分支，包含所有要发布到下一个Release的代码，这个主要合并与其他分支，比如Feature分支
* feature。这个分支主要是用来开发一个新的功能，一旦开发完成，我们合并回Develop分支进入下一个Release
* release。当你需要一个发布一个新Release的时候，我们基于Develop分支创建一个Release分支，完成Release后，我们合并到Master和Develop分支
* hotfix。当我们在Production发现新的Bug时候，我们需要创建一个Hotfix, 完成Hotfix后，我们合并回Master和Develop分支，所以Hotfix的改动会进入下一个Release.

# 2. master分支

只有一个原则，只能做tag

![](/assets/git-flow2.png)

# 3. feature分支

feature分支做完后，必须合并回develop分支, 合并完分支后一般会删点这个feature分支，但是我们也可以保留

![](/assets/git-flow3.png)

开始一个新的功能的开发
```git
git checkout -b some-feature develop
# Optionally, push branch to origin:
git push -u origin some-feature    

# 做一些改动    
git status
git add some-file
git commit    
```

开发完成:

```git
git pull origin develop
git checkout develop
git merge --no-ff some-feature
git push origin develop

git branch -d some-feature

# If you pushed branch to origin:
git push origin --delete some-feature    
```

## 4. release分支

分支名 release/*


release分支基于develop分支创建，打完release分支后，我们可以在这个release分支上测试，修改bug等。同时，其它开发人员可以基于开发新的feature，一旦打了release分支之后不要从develop分支上合并新的改动到Release分支

发布release分支时，合并release到master和develop， 同时在master分支上打个tag记住release版本号，然后可以删除release分支了(当然，你可以选择不删除)。

![](/assets/git-flow4.png)
开始release：
```sh
git checkout -b release-0.1.0 develop

# Optional: Bump version number, commit
# Prepare release, commit
```

完成release：
```sh
git checkout master
git merge --no-ff release-0.1.0
git push

git checkout develop
git merge --no-ff release-0.1.0
git push

git branch -d release-0.1.0

# If you pushed branch to origin:
git push origin --delete release-0.1.0   


git tag -a v0.1.0 master
git push --tags
```

## 5. hotfix

分支名 hotfix/*

hotfix分支基于master分支创建，开发完后需要合并回master和develop分支，同时在master上打一个tag
![](/assets/git-flow5.png)

开始hotfix
```sh
git checkout -b hotfix-0.1.1 master
```

完成hotfix
```
git checkout master
git merge --no-ff hotfix-0.1.1
git push


git checkout develop
git merge --no-ff hotfix-0.1.1
git push

git branch -d hotfix-0.1.1

git tag -a v0.1.1 master
git push --tags
```

# 6. tools

IDEA：Git Flow Integration

![](/assets/git-flow6.png)

* 初始化: `git flow init`
* 开始新Feature: `git flow feature start MYFEATURE`
* Publish一个Feature(也就是push到远程): `git flow feature publish MYFEATURE`
* 获取Publish的Feature: `git flow feature pull origin MYFEATURE`
* 完成一个Feature: `git flow feature finish MYFEATURE`

* 开始一个Release: `git flow release start RELEASE [BASE]`
* Publish一个Release: `git flow release publish RELEASE`
* 发布Release: `git flow release finish RELEASE`
  
  别忘了`git push --tags`
* 开始一个Hotfix: `git flow hotfix start VERSION [BASENAME]`
* 发布一个Hotfix: `git flow hotfix finish VERSION`

Ref：https://yq.aliyun.com/articles/137035
