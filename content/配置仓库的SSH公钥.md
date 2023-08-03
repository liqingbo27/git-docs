# 配置仓库的SSH公钥

你可以按如下命令来生成 sshkey:

~~~
ssh-keygen -t rsa -C "xxxxx@xxxxx.com"  
# Generating public/private rsa key pair...
~~~

> 按照提示完成三次回车，即可生成 ssh key。


通过查看`~/.ssh/id_rsa.pub`文件内容，获取到你的 public key
~~~
cat ~/.ssh/id_rsa.pub
# ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC6eNtGpNGwstc....
~~~

![](images/screenshot_1552893425860.png)