---
title: 如何备份hexo博客
date: 2020-04-20 08:37:33
tags:
	- hexo配置
	- 博客
---

![image-20200430091030706](https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200430091030706.png)

<!-- more -->

## 背景

你已经创建了一个github仓库，并且已经将博文上传到hexo了。假定你原来上传博客的文件夹叫blog。



## 如何备份

* 在你的github.io仓库中新建一个分支source，该分支在以后将会同来保存博客的源文件

* 将新建的source分支设置为默认分支

* 新建一个文件夹blog_backup，在文件夹中将github仓库克隆下来

* 然后将克隆下来的内容全部删除，只保存.git文件夹

  ```bash
  rm -r *
  ```

  使用如下命令查看是否删除干净或者查看finder文件夹下的隐藏文件是`⇧+⌘+.`

  ```bash
  ls -al
  ```

* 将原来blog文件夹如下文件复制到备份文件夹blog_backup/yourname.github.io文件夹中（记得上面的查看隐藏文件夹的命令）

  ```bash
  scaffolds/
  source/
  themes/
  .gitignore
  _config.yml
  package.json
  ```

* 然后在你的备份文件夹blog_backup/yourname.github.io文件夹中按顺序执行如下命令

  ```bash
  npm install
  npm install hexo-deployer-git
  ```

* 提交源文件到source分支

  ```bash
  git commit -am "提交备份"
  git push origin source
  ```

* 执行命令部署到hexo上

  ```bash
  hexo clean
  hexo g 
  hexo d
  ```

* 备份成功, 那么你的文件已经转移到了备份文件夹blog_backup中了，所以可以删除原来的blog文件夹了。



## 在新电脑上恢复

* 安装git node.js npm

* 将仓库克隆到本地

* 在文件夹中顺序执行

  ```bash
  npm install hexo-cli -g
  npm install
  npm install hexo-deployer-git
  ```

  

