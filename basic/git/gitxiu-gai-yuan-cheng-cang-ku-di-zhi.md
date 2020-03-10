# Git-修改远程仓库地址

* set-url
* rm & add
* config
* 查看


## 直接修改

`git remote set-url origin url`

## 删除本地远程仓库地址，然后添加新的仓库地址

```
git remote rm origin
git remote add origin url
```

## 修改配置文件

每个仓库在初始化时，都会有一个 `.git` 的隐藏目录，修改其中的 `config` 文件中的 url

![](/assets/git-config-remote.png)

## 查看

```
git remote -v
```