# 分支管理规范

使用：GITFLOW + GITLAB

## GITFLOW

* **分支操作**：
 - merge：
 - push：只有任务分支或者bug分支才可以，其他分支均为merge操作。Eg：feature-1.0#task1234。
* **分支保护**：
 - 除了任务和BUG分支，其他分支均要保护，只有项目负责人才可以进行Merge。
* **主要分支线**：
 - master
 - release
 - develop
 - feature-XX

## GITLAB最佳实践

* Issue + feature’s branch实现功能的不断增强；
  
  在开发Sigma框架的过程中用得比较多。
* Protect Branches：master、release-*、feature-*、hotfix

  多人协作得时候，广泛使用。
* Set default branch
  
  方便切分支。
* Set Member Access，限制非管理角色，不能合并到受保护的分支

  让Code Review有专门的人做。
* 及时Rebase
 
  保证最小的冲突解决
* 提交之前要合并，本地解决冲突，比如，将feature-1.0合并到feature-1.0#task1234
自治合并
* 一个任务或一个BUG或一个ISSUE，对应一个分支
* 同一个分支可以发起多次合并请求。
在* 可估计的提交次数以内，优先使用rebase。
