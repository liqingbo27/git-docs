# Git pull 强制覆盖本地文件


**远程获取最新版本到本地**
~~~
# git fetch --all  
~~~

> 相当于是从远程获取最新版本到本地，不会自动merge
> 只是下载远程的库的内容，不做任何的合并 


**彻底回退到某个版本，本地的源码也会变为上一个版本的内容，此命令 *慎用* ！**
~~~
# git reset --hard origin/master 
~~~

>  删除所有git add的文件（当然这不包括未置于版控制下的文件 untracked files）
>  把HEAD指向刚刚下载的最新的版本
