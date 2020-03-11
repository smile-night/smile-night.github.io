---
title: 用github+hexo搭建博客常用命令（笔记）
date: 2018-05-13 23:29:44
tags:
---

#### 常用命令 ####
1.写新博客：hexo new blogname
2.发布博客：hexo d -g
3.删除博客：先在source/_posts文件夹下删除文件，然后hexo g, hexo d就可以了

#### pages服务 ####
1：如果想要让访问当前站点的网站自动重定向到一个特定的网站，在source文件夹中自定义文件CNAME,里面写上域名就可以了
