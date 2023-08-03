# 在阿里云上搭建自己的git服务器

这篇文章我就来介绍一下如何在一台全裸的阿里云主机上搭建自己的git服务器。

输入命令：
~~~
$ rpm -qa git 
~~~
![](images/QQ截图20180425144520.png)

表示已经安装

## 1. 安装git

在linux系统中,git安装只需要简单命令就可以完成,只需要打开终端,输入
~~~
$ yum install git # centos
$ apt-get install git # ubuntu
~~~

## 2.创建git用户及权限

建立用户组的目的在于对于这个git服务器，赋予多人访问权限时，可以统一管理。
~~~
$ groupadd git
~~~

在用户组git下创建一个用户,名字也为git
~~~
$ adduser git -g git
~~~

> 执行这条命令之后，你发现在/home目录下多了一个git目录，按理来说，现在，你的系统中多了这个git用户，并且家目录在/home/git。但是，我们并不希望这个用户通过ssh连接到服务器上面去，所以，我们要禁止这个用户使用ssh连接上去进行操作。我们通过编辑一个权限文件来处理：

~~~
$ vi /etc/passwd
~~~
找到类似于
~~~
git:x:1001:1001:,,,:/home/git:/bin/bash
~~~
这样的行，你看到那个末尾的/bin/bash，就是允许ssh连接操作的权限，我们把它改为/user/bin/git-shell，结果如下：
~~~
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
~~~
这样处理好，git就不能ssh连上去了（实际上是可以的，只不过会闪退）。

我们还得给git分配一个密码，执行：
~~~
$ passwd git 123456(你的密码)
~~~
这个密码用在你后面提交代码的时候使用。


## 3.初始化一个git仓库

我习惯把这类东西丢到/var下去，所以，我们在/var下面创建一个git目录

~~~
$ cd /var
$ mkdir git
$ chown -R git:git git
$ chmod 777 git
$ cd git
~~~
> chown -R git:git git ,第一个git是用户名，第二个git是组，第三git是文件夹名

接下来，我们用git命令初始化一个仓库：
~~~
$ git init --bare arepoforyourproject.git
~~~
repoforyourproject是仓库名，名字可以自己定义

初始化完成之后，这个空的仓库就OK了。


>[warning] 这里有一个细节，就是.git目录必须要有可读写权限，因为当我们在push的时候，是使用git用户推送到服务器上面去，会有一个写入的过程，如果不赋予可写权限，push就会失败。

>[warning]记得修改git库的 `所有者` 把root:root改成git:git了

远程仓库的地址一般组成的格式是：

~~~
<用户名>@<服务器地址>:<仓库全路径>
~~~
用户名就是当前登录的账号的名称，比如我当前用的是 git账号，用户名就是git
服务器地址就是远程服务器的地址，比如 120.21.11.21
仓库全路径这个也不难理解，直接在project.git目录下输入pwd,获取project.git的全路径。

## 4.获取远程仓库地址
远程仓库的地址一般组成的格式是：
~~~
<用户名>@<服务器地址>:<仓库全路径>
~~~
用户名就是当前登录的账号的名称，比如我当前用的是 git账号，用户名就是git
服务器地址就是远程服务器的地址，比如 120.21.11.21
仓库全路径这个也不难理解，直接在project.git目录下输入pwd,获取project.git的全路径。

比如：
/home/git/gitRepository/pythonServer/project.git

那么整个远程仓库的地址就是：
~~~
git@120.21.11.21:/home/git/gitRepository/pythonServer/project.git
~~~

## 5.公钥

这个是git里面比较特殊的一步操作，通信的时候，客户端与服务器需要一个证书进行验证。操作方法很简单，首先在你自己的电脑上（ubuntu）生成自己的一个公钥：
~~~
$ cd ~
$ ssh-keygen -t rsa
~~~
这时你自己电脑上就有一个公钥了，但是在哪里呢？在.ssh目录下，.开头的文件夹都是隐藏的，但是可以cd进去。
~~~
$ cd .ssh
$ vi id_rsa.pub
~~~
这样就能看到你的公钥了，把所有的内容复制下来。接下来，我们去回服务器上面操作。
~~~
$ cd /home/git/
$ mkdir .ssh
$ cd .ssh
$ vi authorized_keys
~~~
如果是裸机，服务器上面/home/git目录下应该没有.ssh目录，所以我们自己创建，打开（自动创建）authorized_keys之后，把刚才复制下来的公钥黏贴进去，ok了，保存退出。

使用证书，主要是为了无需密码就可以提交代码
具体请看[《使用SSH证书远程登陆你的服务器》。](https://www.tangshuang.net/1697.html)


## 6.多用户和权限管理

如果团队很小，把每个人的公钥收集起来放到服务器的/home/git/.ssh/authorized_keys文件里就是可行的。如果团队有几百号人，就没法这么玩了，这时，可以用[Gitosis](https://github.com/res0nat0r/gitosis)来管理公钥。

## 7.服务端授权ssh公钥
~~~
# cd /home/git/
# mkdir .ssh
# chmod 700 .ssh
# touch .ssh/authorized_keys
# chmod 600 .ssh/authorized_keys
# chown -R git:git .ssh  
~~~
其中`/home/git`目录为服务器上用户`git`的主页目录，上诉操作相当于在`/home/git/.ssh/`目录下新建一个`authorized_keys`文件。并把目录`.ssh`的权限设置为700，`authorized_keys`文件权限设置为600.
因为git的pull相当于读操作，push相当于写操作，所以需要读写权限。


接下来要做的是将客户端的公钥注册到服务端中,打开服务端控制台，输入：
~~~
cat "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC9+VK1abwzJg+VjxGwpwKnsYU3eBEjXolQKUfKxEAMO9DREvdFrvIF5KhBE9HJTp7CEFcAfgP6xkJdxchQcEUyPyda9mIz6M4OOeuuLcxJcrqqJWTN0Jj78I/kDUZUJZF7bcV4q0CyeZG1fo5ckjxOmFaYkCGcq8vmeuFySFpco71UMkudzclrtGa53AvfmuOP1u96CEud78p2gYrPP5qr9ZYBNM+E290ddGMV61rnEiL7taAsXMGpuCQSbI0/zBJ3YXvN/lJSOVHFSeMFbKv7WDSJFSiBVHXjtcDa5RvzzWaFMBV8+SW4zluhijp6Dvb7pHBaLhLg/JvOixmR1/or OboBear" >> ~/.ssh/authorized_keys 
~~~
这双引号里面一大串的就是你之前复制的公钥,整句命令所做的事情就是将客户端的公钥添加到服务端的ssh授权表中。

## 客户端授权

[win10生成SSH keys](win10生成SSHkeys.md)


* * * * *
