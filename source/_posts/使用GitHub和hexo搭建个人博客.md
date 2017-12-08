---
title: 使用GitHub和hexo搭建个人博客
date: 2017-12-07 16:04:52
tags:
- hexo
- git 
categories:
- blog
---
2. 在github上新建一个 你的账号名.github.io的仓库
3. 实现ssh免密登录
```
1.生成秘钥对（一个电脑执行一次就行）  
ssh-keygen -t rsa
    一路回车，冒泡就成功
2.在github的设置中 添加一个ssh key
3.key中的内容就是将生成的公钥内容粘贴进去就可以了
4.以后使用github的时候，直接（只有）使用ssh连接就可以实现免密
```
4.hexo的使用
```
1.安装hexo
npm i hexo -g
2.初始化hexo
hexo init
3.生成静态页面
hexo g
4.本地预览
hexo s
5.修改配置
修改config.yml文件(：后面有一个空格)
deploy:

     type: git

     repo: git静态仓库的名称

     branch: master
6.安装插件
npm install hexo-deployer-git --save
7.上传至GitHub静态仓库
9.访问 你的GitHub账户名.github.io就可以了
10.hexo主题的修改
从GitHub上拷贝主题包放入themes文件夹中
将根目录下config.yml文件的theme名称修改成对应的主题名称
修改主题包下config.yml文件的配置进行个性化的主题配置(所有的静态图片资源放在source文件夹下即可，在配置文件中修过其对应的路径)
------------------------------------------------------
每次部署的步骤，可按以下三步来进行。
hexo clean

hexo generate

hexo deploy

--------------------------------------------------------
一些常用命令：
hexo new"postName" #新建文章

hexo new page"pageName" #新建页面

hexo generate #生成静态页面至public目录

hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）

hexo deploy #将.deploy目录部署到GitHub

hexo help # 查看帮助

hexo version #查看Hexo的版本

```
