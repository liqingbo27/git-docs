# Git 命令

## git config

查看系统config
```
git config --system --list
```
查看当前用户（global）配置
```
git config --global  --list
```
查看当前仓库配置信息
```
git config --local  --list
```

```
$ git config --list
user.name=Scott Chacon
user.email=schacon@gmail.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```
## git init

## git status

## git log

```
git log
```

## git reflog
```
git reflog
```

## git branch

## git checkout

放弃掉还没有加入到暂存区的``修改``。

此命令不会影响新创建的文件，因为新创建的文件还没有被跟踪（tracked）。

## git clean

从你的工作目录中删除所有没有 tracked，没有被管理过的文件。

##### 参数说明：

n ：显示将要被删除的文件   
d ：删除未被添加到 git 路径中的文件（将 .gitignore 文件标记的文件全部删除）     
f ：强制运行     
x ：删除没有被 track 的文件  

```
git clean 和 git reset --hard 结合使用。
clean 影响没有被 track 过的文件（清除未被 add 或被 commit 的本地修改）
reset 影响被 track 过的文件 （回退到上一个 commit）
所以需要 clean 来删除没有 track 过的文件，reset 删除被 track 过的文件
结合两命令 → 让你的工作目录完全回到一个指定的 <commit> 的状态
```

## git commit

```
git commit -m [message] //[message] 可以是一些备注信息。
```

提交暂存区的指定文件到仓库区：
```
$ git commit [file1] [file2] ... -m [message]
```

-a 参数设置修改文件后不需要执行 git add 命令，直接来提交
```
$ git commit -a
```
> 经测试发现和git add后有点区别

## git add 

## git diff 

##### git diff用来比较文件之间的不同，其基本用法如下：
* git diff：当工作区有改动，临时区为空，diff的对比是“工作区与最后一次commit提交的仓库的共同文件”；当工作区有改动，临时区不为空，diff对比的是“工作区与暂存区的共同文件”。
* git diff --cached 或 git diff --staged：显示暂存区(已add但未commit文件)和最后一次commit(HEAD)之间的所有不相同文件的增删改(git diff --cached和git diff –staged相同作用)
* git diff HEAD：显示工作目录(已track但未add文件)和暂存区(已add但未commit文件)与最后一次commit之间的的所有不相同文件的增删改。
    - git diff HEAD~X或git diff HEAD^^^…(后面有X个^符号，X为正整数):可以查看最近一次提交的版本与往过去时间线前数X个的版本之间的所有同(3)中定义文件之间的增删改。


##### git diff <分支名1> <分支名2> ：比较两个分支上最后 commit 的内容的差别

* git diff branch1 branch2 --stat    显示出所有有差异的文件(不详细,没有对比内容)
* git diff branch1 branch2              显示出所有有差异的文件的详细差异(更详细)
* git diff branch1 branch2 具体文件路径 显示指定文件的详细差异(对比内容)


## git fetch

一旦远程主机的版本库有了更新（Git术语叫做commit），需要将这些更新取回本地，这时就要用到git fetch命令。

* [git fetch](fetch.md)
* [git中pull和fetch的区别](git中pull和fetch的区别.md)


## git rm
> 命令用于删除文件。

如果只是简单地从工作目录中手工删除文件，运行 ``git status`` 时就会在 ``Changes not staged for commit`` 的提示。

###### git rm 删除文件有以下几种形式：

将文件从暂存区和工作区中删除：
```
git rm <file>
```
以下实例从暂存区和工作区中删除 runoob.txt 文件：
```
git rm runoob.txt 
```
如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f。

强行从暂存区和工作区中删除修改后的 runoob.txt 文件：
```
git rm -f runoob.txt 
```
如果想把文件从暂存区域移除，但仍然希望保留在当前工作目录中，换句话说，仅是从跟踪清单中删除，使用 --cached 选项即可：
```
git rm --cached <file>
```
以下实例从暂存区中删除 runoob.txt 文件：
```
git rm --cached runoob.txt
```

## git reset
```
git reset –mixed + 版本号
```
暂存区(add/index区)和提交区(commit区)会回退到某个版本，但代码不改变。 

```
git reset –soft + 版本号 
```
提交区(commit区)会回退到某个版本，暂存区(add/index区)不会回退，代码不改变。 

```
git reset –hard + 版本号 
```
暂存区(add/index区)和提交区(commit区)会回退到某个版本，代码会改变。(推荐)

## git revert 
```
git revert + 版本号
```
暂存区(add/index区)和提交区(commit区)会回退到某个版本，代码会改变。

##### 实例
删除 hello.php 文件：
```
$ git rm hello.php 
rm 'hello.php'
```
文件从暂存区域移除，但工作区保留：
```
git rm --cached README 
```
可以递归删除，即如果后面跟的是一个目录做为参数，则会递归删除整个目录中的所有子目录和文件：
```
git rm –r * 
```
进入某个目录中，执行此语句，会删除该目录下的所有文件和子目录。

## git remote

```java
git remote -v //查看远程地址
git remote rm origin //删除
git remote remove origin //删除
git remote show [remote] //显示某个远程仓库的信息
git remote add origin https://gitee.com/liqingbo/test.git //添加远程版本库
git remote rename old_name new_name  # 修改仓库名
```
