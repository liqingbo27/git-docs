##### git status 出错 interactive rebase in progress； onto 796e78f


查看一番发现,由于之前使用过 git pull --rebase origin develop 命令拉取代码,使用过git rebase执行代码覆盖,但是上一次进程还没有完成导致

原因:

查看git 的提示, 大概意思是 你当前正在编辑的提交将要覆盖在 796e78 commitid 上

##### 两种解决方案

使用 git commit --amend 命令修订当前的提交
使用 git rebase --continue 命令继续代码的提交(推荐),执行之后,需要重新提交,解决一下当前的代码冲突之后重新提交直至没有rebase提示,就可以正常提交了