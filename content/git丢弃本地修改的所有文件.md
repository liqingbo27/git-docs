# git丢弃本地修改的所有文件（新增、删除、修改）

本地修改了许多文件，其中有些是新增的，因为开发需要这些都不要了，想要丢弃掉，可以使用如下命令：

**本地所有修改的。没有的提交的，都返回到原来的状态**
~~~
git checkout . 
~~~

**把所有没有提交的修改暂存到stash里面。可用git stash pop回复。**
~~~
git stash #
~~~

**返回到某个节点，不保留修改。**
~~~
git reset --hard HASH
~~~

**返回到某个节点。保留修改**
~~~
git reset --soft HASH
~~~

**返回到某个节点**
~~~
git clean -df
~~~

**git clean**
~~~
    -n 显示 将要 删除的 文件 和  目录
    -f 删除 文件
    -df 删除 文件 和 目录

~~~
也可以使用：
~~~
git checkout . && git clean -xdf
~~~