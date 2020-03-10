# Git-merge


* git merge –no-ff 可以保存你之前的分支历史。能够更好的查看 merge历史，以及branch 状态。
* git merge 则不会显示 feature，只保留单条分支记录。
* --no-ff指的是强行关闭fast-forward方式。
* fast-forward方式就是当条件允许的时候，git直接把HEAD指针指向合并分支的头，完成合并。属于“快进方式”，不过这种情况如果删除分支，则会丢失分支信息。因为在这个过程中没有创建commit
* git merge --squash 是用来把一些不必要commit进行压缩，比如说，你的feature在开发的时候写的commit很乱，那么我们合并的时候不希望把这些历史commit带过来，于是使用--squash进行合并，此时文件已经同合并后一样了，但不移动HEAD，不提交。需要进行一次额外的commit来“总结”一下，然后完成最终的合并。
* --no-ff：不使用fast-forward方式合并，保留分支的commit历史
* --squash：使用squash方式合并，把多次分支commit历史压缩为一次


![](/assets/Git-merge)