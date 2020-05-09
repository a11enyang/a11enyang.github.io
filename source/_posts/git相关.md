---
title: git相关
date: 2020-05-05 17:13:07
tags:
	- git
---

### git origin ?

https://www.zhihu.com/question/27712995

origin只的是远程仓库的别名



### git remote

https://blog.csdn.net/abo8888882006/article/details/12375091

https://www.jianshu.com/p/09bb9a6a13e6





### 给终端加上代理

git config --global http.https://github.com.proxy socks5://127.0.0.1:1086
git config --global https.https://github.com.proxy socks5://127.0.0.1:1086



# 问题合集

> fatal: refusing to merge unrelated histories

远程仓库和本地仓库是两个无相关的仓库

解决办法：`git pull origin master --allow-unrelated-histories`

