---
title: Hexo搭建简易指南
date: 2016-10-27 21:06:24
tags: github  IT
---
# Hexo搭建简易指南

面向对象是程序员们，此教程主要以Windows下安装, Mac和linux安装就更简单了基本是输入安装Hexo的几个命令即可

## 搭建环境准备

### GIT环境搭建
建议安装一个git客户端软件(下载不了？因为你没翻墙啊)
    > 点击去下载吧： [GitBash客户端下载](https://git-scm.com/downloads/)

###安装Nodejs环境
直接去官网下载一个，无脑下一步就行
    > 点击去下载吧： [Nodejs安装下载](https://nodejs.org/en/)

### 注册一个GITHUB账号
这个就不用教了吧，相信作为一个合格的程序员，你早就有了。什么？ 你没有，那你离合格的程序员就差一个GITHUB账号了，还不赶紧戳[我去注册](https://github.com/join?source=login)

## 开始安装Hexo
1 安装Hexo命令行环境
```
 npm install hexo-cli -g
```

2 在本地创建一个目录，用来存放你的博客源码

3 进入新建的目录执行以下命令，进行初始化和安装工作
```
hexo init
```

4 接下来生成默认的博客并在本地测试下
```
hexo g  #生成默认的hello world
hexo s  # 启动本地服务 访问 localhost:4000查看效果
```

5 到这步说明本地已经安装好了，接下来是如何同步到github上用默认的域名访问
  在你的github上创建一个新仓库，仓库名格式必须是：{你的用户名}.github.io
  
6 配置配置文件_config.yml 内容如下（注意空格!）
```
deploy:
  type: git
  repo: https://github.com/{你的用户名}/{你的用户名}.github.io.git
  branch: master
```

7 安装一个发布插件并发布
```
npm install hexo-deployer-git --save  #安装发布插件

hexo deploy #发布到github
```

8 全部完成，你可以通过github提供的域名进行访问你的博客了！
   默认域名地址：{你的用户名}.github.io 当然你还可以绑定到你自己的域名上
   请参考教程: [Github上如何绑定自己的域名](http://www.hzqiuxm.com)