---
title: HEXO搭建个人博客
date: 2019-08-20 19:37:05
tags:
---

## 一、安装Hexo

Hexo的安装在其[官方文档](https://links.jianshu.com/go?to=https%3A%2F%2Fhexo.io%2Fzh-cn%2Fdocs%2F)中有很详细的说明。下面将简单介绍Hexo的安装。

### 1.1 安装前提

- 安装[ Node.js ](https://links.jianshu.com/go?to=https%3A%2F%2Fnodejs.org)，请进入Node.js 的官网下载安装。
- 安装[ Git ](https://links.jianshu.com/go?to=http%3A%2F%2Fgit-scm.com)，前面已经说明，不再赘述。

### 1.2 安装Hexo

上面两个工具安装完整之后，打开 `Git Bash` ，只需要使用npm即可完成Hexo的安装。



```powershell
$ npm install -g hexo-cli
```

安装Hexo完成之后，执行下面的命令，Hexo将会在你制定的文件夹中新建所需要的文件。



```powershell
$ hexo init <folder>
$ cd <folder>
$ npm install
```

新建完成后，文件夹下的目录如下：



```powershell
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

- **_config.yml** 文件是网站的配置文件，可以在其中配置网站的大部分参数。
- **package.json** 文件是应用程序的信息。
- **source** 是资源文件夹，是用来存放用户资源的地方。
- **themes** 是[主题](https://links.jianshu.com/go?to=https%3A%2F%2Fhexo.io%2Fthemes)文件夹，Hexo会根据主题来生成不同的静态页面。
- **scaffolds**是模板件夹，当新建文章的时候，Hexo会根据模板来建立文件。

### 1.3 修改主题

主题可以在Hexo Themes、github里面寻找。

先进入你的Hexo文件夹。然后使用下面的命令`clone`下`XX`主题。



```powershell
$ git clone https://github.com/XX
```

然后打开Hexo主文件夹下的**_config.yml**，修改其中的`theme` 属性。`theme:` 后面要加空格。



```ruby
theme: XX
```

### 1.4 本地查看调试



```powershell
$ hexo g #生成静态页面，生成的内容在public文件夹下
$ hexo s #启动本地服务，进行文章预览调试。hexo s --debug 命令可以用来调试
```

### 1.5 部署到Github Pages

先使用下面的命令对Git进行初始配置。



```powershell
$ git config --global user.name "your name"
$ git config --global user.email "email@email.com"
```

然后打开Hexo主文件夹下的**_config.yml**，设置其中的`deploy` 参数，详细请查看Hexo官方文档中[部署部分](https://links.jianshu.com/go?to=https%3A%2F%2Fhexo.io%2Fzh-cn%2Fdocs%2Fdeployment.html)。

我的设置如下所示：



```powershell
deploy:
  type: git
  repository: https://github.com/liuchen114113/liuchen114113.github.io.git
  branch: master
```


```powershell
$ npm install hexo-deployer-git --save
```

安装完插件之后使用下面的命令进行部署：



```powershell
$ hexo g #生成静态文件
$ hexo d #部署到github 
```

还有一个更快捷的命令：



```powershell
$ hexo d -g #在部署前先生成
```

## 二、Hexo常用命令

下面仅列出几种常用的命令。更详细的命令请查看[Hexo官方文档](https://links.jianshu.com/go?to=https%3A%2F%2Fhexo.io%2Fzh-cn%2Fdocs%2Fcommands.html)。



```powershell
$ hexo clean #清理之前生成的内容，即public文件
$ hexo g #生成静态文件
$ hexo d #部署
$ hexo s #启动本地服务，可以通过http://localhost:4000查看
$ hexo s --debug #使用debug模式启动服务
```