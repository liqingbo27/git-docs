# Git里设置大小写敏感
Windows上的Git默认是大小写不敏感的，这样多平台写作就可能会出现问题。

讲Win上的Git设置为大小写敏感的命令如下

~~~
git config core.ignorecase false
~~~