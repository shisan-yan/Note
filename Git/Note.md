git的存储机制

​	每次commit对应一棵树，这棵树就是当时版本文件的一个快照，也就是当时文件的状态

​	Git数据存储在blob中，相同内容的文件会有相同的校验值，校验值相同的文件只会对应一个blob，也就是及时有重复的文件，那么他们对应的blob也是完全相同的。Git的blob只针对文件内容，不管文件名称是否相同。

分离头指针

​	比如我们checkout到一个commitID中去，这个时候我们是处于一个没有分支的状态的，这就叫做分离头指针。此时我们做的所有操作 如果我们没有进行与分支的绑定，而直接切换到其他分支的时候，即使我们做了commit提交，Git也不会保留我们刚才的那次提交的改动，在定时清理的过程中，我们刚才的所有变动都会被清空掉。如果我们想要保留刚才的commit提交，我们需要手动的git branch newname  name  来绑定一个分支

head与branch

​	其实一个branch就是一个commit的快照。所以不管heade处于分离头指针状态还是指向分支的状态。他其实最终都是指向一个commit

清除不想要的分支

​	git branch -d branchName  <-D强制删除>

修改最近一次提交的message

​	git commit --amend

commit合并

​	如果我们有几个commit是做同样的事情或者是在做同一个项目。为了让commit看起来更加的简洁，我们可以将这几个commit进行合并

​	git rebase -i  commitID<此处的commitID要注意，他应该是你想要合并的commit范围中，最前一个commit的上一个commitID，如果我们需要合并的commit就是从第一个开始的，那么我们应该写第一个commit的ID，然后在文件中手动的将第一次的commit信息按照格式写入进去。如果这几次的commit是不连续的，我们需要将第一个commit选择出来放到第一，然后将其他的commit依次排序到他的后面，并将pick改为s，保存退出>

​	此时会进入到一个编辑状态，我们选择第一个需要合并的commit作为pick，然后其他所有的需要合并的都作为squach，即将文件中的pick替换成s即可。修改保存退出后，会进入下一个文件的编辑，我们此时只要在第一个注释的后面添加一个此次合并的原因即可<如果是不连续的话，我们需要执行git rebase  --continue 才会进入到这个状态>

git diff

​	如果不加任何参数，默认比较工作区跟暂存区之间的差异。如果想要指定比较哪个文件，可以使用git diff -- filename来比较

撤销暂存区的改动

​	git rest HEAD <撤销暂存区的所有改动>

撤销暂存区<add>的改动

​	git checkout --  file

将工作区、暂存区、head指向全部到某个commit

​	git reset --hard commitID

比较两个分支的差别

​	git diff temp master <-- filename 不指定表示所有文件>

删除git文件

​	git rm filename

保存暂存区状态

​	git stash 

​	git stash list

​	git stash  apply 



忽略不需要管理的文件

​	在项目根目录下添加一个gitignore文件。	

​	规则： 目录以/结尾，比如 doc/   匹配所有用* 比如 *.class

git的备份

​	远端备份可以直接git clone  本地的可以直接使用本地协议 /路径 或者 file://路径 协议



​	







.





