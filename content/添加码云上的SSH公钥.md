# 码云上添加SSH公钥

## 一、打开终端输入
~~~
ssh-keygen -t rsa -C "xxxxx@xxxxx.com"  
#Generating public/private rsa key pair...
~~~
> 三次回车即可生成 ssh key
![](images/screenshot_1566808635716.png)

## 二、查看你的 public key
~~~
cat ~/.ssh/id_rsa.pub
# ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC6eNtGpNGwstc....
~~~

![](images/screenshot_1566808775427.png)

## 三、码云上添加个人公钥
复制以上公钥，添加到马云的个人私钥里面
## 四、查看状态
**添加后，在终端（Terminal）中输入**
~~~
ssh -T git@gitee.com
~~~
若返回以下信息，则证明添加成功。
~~~
Hi liqingbo! You've successfully authenticated, but GITEE.COM does not provide shell access.
~~~
![](images/screenshot_1566809297994.png)

## 五、配置git pull/push 免密码
进入项目根目录，也就是.git所在目录
终端输入
~~~
vi .git/config
~~~
文件代码
~~~
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = git@gitee.com:liqingbo/项目.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        merge = refs/heads/master
~~~
修改配置文件

HTTPS传输的地址，你需要改成SSH的传输地址
~~~
url = https://@gitee.com/liqingbo/项目.git
~~~
改成
~~~
url = git@gitee.com:liqingbo/项目.git
~~~
>[warning] 注：https://改成git和gitee.com，后面的“/”改成“:”

这样使用命令 git pull/push 就不用输入密码了，这是因为刚才在生成公钥时，没有输入密码，所以当你选择SSH地址传输时，就可免密码使用命令 git pull/push。
