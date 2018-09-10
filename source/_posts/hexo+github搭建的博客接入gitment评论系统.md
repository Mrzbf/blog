---
title: 博客接入gitment系统
date: 2018-08-15 16:04:52
tags:
- gitment
- 
categories:
- blog
---
### 为博客接入评论系统
1.[OAuth Application注册](https://github.com/settings/applications/new)
![image](https://github.com/Mrzbf/Mrzbf.github.io/blob/master/assets/img/js/2018-08-15-01.png?raw=true)
+ Application name随意填 
+ Homepage URL填博客地址我的填的就是(https://mrzbf.github.io/)
+ Application description随便填
+ Authorization callback URL填博客地址(https://mrzbf.github.io/)
2.OAuth Application注册成功以后，将得到的Client ID和Client Secret填入对应主题文件夹下的_config.yml中，
![image](https://github.com/Mrzbf/Mrzbf.github.io/blob/master/assets/img/js/2018-08-15-02.jpg?raw=true)
+ gitment_owner填入自己的GitHub账户名
+ gitment_repo填入自己储存评论的仓库名称
+ client_id和client_secret则填入申请OAuth Application得到的对应的Client ID
3.信息填写完毕后访问自己的博客地址，然后使用自己的GitHub账号登录评论系统，登录后点击Initialize Comments初始化评论即可。