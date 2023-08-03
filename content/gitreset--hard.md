
# git reset -–hard

```
$ git reset --hard HEAD^         回退到上个版本
$ git reset --hard HEAD~3        回退到前3次提交之前，以此类推，回退到n次提交之前
$ git reset --hard commit_id     退到/进到 指定commit的sha码
```

**git reset -–hard**
彻底回退到某个版本，本地的源码也会变为上一个版本的内容，此命令 **慎用**！

**git fsck --lost-found**
这个命令可以恢复git add过的文件


## 相关命令
* git reset --mixed：此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息
* git reset --soft：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可
