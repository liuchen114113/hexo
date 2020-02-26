---
title: 用hexo搭建个人博客
date: 2019-08-16 18:37:05
tags:
---
这两天一直在捣鼓hexo，它是一款快速、简洁而且高效的博客框架，使用Markdown(或者其他渲染引擎)解析文章，非常适合搭建个人博客。

上手hexo之前，你需要做的准备工作有：
+ node ：https://nodejs.org/en/
+ git

node和git都安装好之后就可以安装hexo了:
`$ sudo npm install -g hexo`

# 1、初始化
创建一个目录，在该目录下执行：
`$hexo init`

一个hexo项目就初始化好了，他会自动创建网站所需要的文件。

执行如下命令启动hexo项目：
`$hexo server/hexo s`

在浏览器中打开 http://localhost:4000， 能看到博客首页.

来看下项目目录：
+ scaffolds //模板文件夹，当新建文章时，Hexo 会根据 scaffold 来建立文件。模板是指在新建的markdown文件中默认填充的内容。例如，如果您修改scaffold/post.md中的Front-matter内容，那么每次新建一篇文章时都会包含这个修改。
+ source //资源文件夹，存放用户资源的地方，除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件
+ themes // 主题文件夹，Hexo 会根据主题来生成静态页面。
+ _config.yml //网站配置信息
+ package.json //HEXO项目依赖

# 2、部署到github
现在的hexo项目只能跑在本地，如果希望别人可以访问，我们需要将项目部署到github上。
登陆git，创建仓库，仓库名为用户名.github.io（这个项目名是固定写法）
创建好之后，在本地项目中修改_config.yml配置文件为如下：

```yml
deploy:
  type: git
  repository: https://github.com/liuchen114113/liuchen114113.github.io.git
  branch: master
```

ps：在配置所有的_config.yml文件时（包括theme中的），在所有的冒号:后边都要加一个空格，否则执行hexo命令会报错

在blog文件夹目录下执行生成静态页面命令：
`$ hexo generate/hexo g`

再执行配置命令：
`$ hexo deploy/hexo d`

hexo deploy命令执行成功后，浏览器中打开网址 https://liuchen114113.github.io/ 能看到和打开 http://localhost:4000 时一样的页面。

# 3、发布博客
进入项目根目录下下，执行如下命令新建文章：
`$ hexo new blogName`

目录/blog/source/_posts下会新建名为blogName.md的markdown文件。

文章编辑完成后，终端cd到blog文件夹下，执行如下命令来发布:

```
$ hexo g             //生成静态页面
$ hexo d            //将文章部署到Github
```

# 4、配置主题
如果不喜欢默认配置的主题，可以在官网选择自己喜欢的主题：https://hexo.io/themes/#Material%20Design

点击喜欢的主题标题，进入github目录，以如下主题为例：

执行：
`$ git clone https://github.com/huyingjie/hexo-theme-A-RSnippet`
然后拷贝到themes目录下，并修改_config.yml里theme的名称landscape修改为A-RSnippet（你选择的主题名称），在项目根目录下执行如下命令(每次部署文章的步骤)：

```
$ hexo clean           //清除缓存文件 (db.json) 和已生成的静态文件 (public)
$ hexo g             //生成缓存和静态文件
$ hexo d             //重新部署到服务器
```
主题就更改配置完毕啦，撒花～

# 5、绑定个人域名
后续再补

# 6、评论功能
这里采用了gitment来实现评论功能。

## 注册github应用
首先需要注册一个OAuth Application，地址:https://github.com/settings/applications/new

+ Application name 随意就好
+ Homepage URL 你的博客地址，例如:https:/liuchen114113.github.io/
+ Application description 随意就好
+ Authorization callback URL 也是博客地址，例如:https://liuchen114113.github.io/
+ 输入完成之后，点击注册就OK了。成功之后就会拿到Client ID和Client Secret。

## 修改项目配置文件
修改项目根目录下_config.yml，添加如下代码：
```
# Comments
gitment:
  enable: true
  mint: true
  count: true
  lazy: false
  cleanly: false
  language:
  github_user: piggyzss
  github_repo: piggyzss.github.io/
  client_id: 09a2760e2209a8728279
  client_secret: 1162d7e456f9c8d3f15f164e63f5d253679445a5
  proxy_gateway:
  redirect_protocol:
```

将github_user和github_repo换为你自己的git账号和地址，client_id和client_secret换为刚刚生成的Client ID和Client Secret。

## 添加样式和layout
打开当前使用主题的目录，进入layout/_partial/comments/disqus.ejs，在原文件后加入如下代码：
```ejs
<% if(theme.gitment.enable) { %>
        <div id="gitment_title" class="gitment_title"></div>
        <div id="container" style="display:none"></div>
        <link rel="stylesheet" href="https://imsun.github.io/gitment/style/default.css">
        <script src="https://imsun.github.io/gitment/dist/gitment.browser.js"></script>
        <script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
        <script>
          const myTheme = {
            render(state, instance) {
              const container = document.createElement('div');
              container.lang = "en-US";
              container.className = 'gitment-container gitment-root-container';
              container.appendChild(instance.renderHeader(state, instance));
              container.appendChild(instance.renderEditor(state, instance));
              container.appendChild(instance.renderComments(state, instance));
              container.appendChild(instance.renderFooter(state, instance));
              return container;
            }
          }
          function showGitment() {
            $("#gitment_title").attr("style", "display:none");
            $("#container").attr("style", "").addClass("gitment_container");
            var gitment = new Gitment({
              id: decodeURI(window.location.pathname),
              theme: myTheme,
              owner: 'piggyzss',
              repo: 'piggyzss.github.io/',
              oauth: {
                client_id: '09a2760e2209a8728279',
                client_secret: '1162d7e456f9c8d3f15f164e63f5d253679445a5'
              }
            });
            gitment.render('container');
          }
          showGitment();
        </script>
    <% } %>
```
现在，一个带有评论功能的个人博客就完成啦～