---
title: Hexo搭建Github静态博客
date: 2016-04-28 12:51:10
tags: git
---
1. 环境环境
1.1 安装Git

请参考【1】

1.2 安装node.js

下载：http://nodejs.org/download/

可以下载 node-v0.10.33-x64.msi

安装时直接保持默认配置即可。

2. 配置Github
1.1 建立Repository

建立与你用户名对应的仓库，仓库名必须为【your_user_name.github.io】

1.2 配置SSH-Key

参考【1】

3. 安装Hexo
关于Hexo的安装配置过程，请以官方Hexo【2】给出的步骤为准。

3.1 Installation

打开Git命令行，执行如下命令

$ npm install -g hexo
3.2 Quick Start

 1. Setup your blog

在电脑中建立一个名字叫「Hexo」的文件夹（比如我建在了D:\Hexo），然后在此文件夹中右键打开Git Bash。执行下面的命令

$ hexo init
[info] Copying data
[info] You are almost done! Don't forget to run `npm install` before you start b
logging with Hexo!
Hexo随后会自动在目标文件夹建立网站所需要的文件。然后按照提示，运行 npm install（在 /D/Hexo下）

npm install
会在D:\Hexo目录中安装 node_modules。

2. Start the server

运行下面的命令（在 /D/Hexo下）

$ hexo server
[info] Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
表明Hexo Server已经启动了，在浏览器中打开 http://localhost:4000/，这时可以看到Hexo已为你生成了一篇blog。

你可以按Ctrl+C 停止Server。

3. Create a new post

新打开一个git bash命令行窗口，cd到/D/Hexo下，执行下面的命令

$ hexo new "My New Post"
[info] File created at d:\Hexo\source\_posts\My-New-Post.md
刷新http://localhost:4000/，可以发现已生成了一篇新文章 "My New Post"。

NOTE：

有一个问题，发现 "My New Post" 被发了2遍，在Hexo server所在的git bash窗口也能看到create了2次。

$ hexo server
[info] Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
[create] d:\Hexo\source\_posts\My-New-Post.md
[create] d:\Hexo\source\_posts\My-New-Post.md
经验证，在hexo new "My New Post" 时，如果按Ctrl+C将hexo server停掉，就不会出现发2次的问题了。

所以，在hexo new文章时，需要stop server。

4. Generate static files

执行下面的命令，将markdown文件生成静态网页。

$ hexo generate
该命令执行完后，会在 D:\Hexo\public\ 目录下生成一系列html，css等文件。

5. 编辑文章

hexo new "My New Post"会在D:\Hexo\source\_posts目录下生成一个markdown文件：My-New-Post.md

可以使用一个支持markdown语法的编辑器（比如 Sublime Text 2）来编辑该文件。

6. 部署到Github

部署到Github前需要配置_config.yml文件，首先找到下面的内容

# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type:
然后将它们修改为

# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: github
  repository: git@github.com:zhchnchn/zhchnchn.github.io.git
  branch: master
NOTE1:

Repository：必须是SSH形式的url（git@github.com:zhchnchn/zhchnchn.github.io.git），而不能是HTTPS形式的url（https://github.com/zhchnchn/zhchnchn.github.io.git），否则会出现错误：

$ hexo deploy
[info] Start deploying: github
[error] https://github.com/zhchnchn/zhchnchn.github.io is not a valid repositor URL!
使用SSH url，如果电脑没有开放SSH 端口，会致部署失败。

fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
NOTE2：

如果你是为一个项目制作网站，那么需要把branch设置为gh-pages。

7. 测试

当部署完成后，在浏览器中打开http://zhchnchn.github.io/（https://zhchnchn.github.io/） ，正常显示网页，表明部署成功。

8. 总结：部署步骤

每次部署的步骤，可按以下三步来进行。

hexo clean
hexo generate
hexo deploy
9. 总结：本地调试

1. 在执行下面的命令后，

$ hexo g #生成
$ hexo s #启动本地服务，进行文章预览调试
浏览器输入http://localhost:4000，查看搭建效果。此后的每次变更_config.yml 文件或者新建文件都可以先用此命令调试，尤其是当你想调试新添加的主题时。

2. 可以用简化的一条命令

hexo s -g
3.3 命令总结

3.3.1 常用命令

复制代码
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #将.deploy目录部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本
复制代码
3.3.2 复合命令

hexo deploy -g  #生成加部署
hexo server -g  #生成加预览
命令的简写为：

hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
4 配置Hexo
4.1 配置文件介绍

下面的各个部分的介绍，请直接参考【3】。

1. 默认目录结构介绍

2. _config.yml配置文件介绍

NOTE：在修改_config.yml配置文件时，按照【3】的介绍进行修改后，重新 hexo clean 或者hexo deploy时，可能会出现如下错误：

复制代码
$ hexo clean
[error] { name: 'HexoError',
  reason: 'can not read a block mapping entry; a multiline key may not be an imp
licit key',
  mark:
   { name: null,
     buffer: '# Hexo Configuration\n## Docs: http://hexo.io/docs/configuration.h
tml\n## Source: https://github.com/hexojs/hexo/\n\n# Site\ntitle: Zhchnchn\nsubt
itle: Coding on the way\ndescription: Zhchnchn\'s blog\nauthor: Zhchnchn\nemail:
115063497@qq.com\nlanguage:zh-CN\n\n# URL\n## If your site is put in a subdirect
......
,
     position: 249,
     line: 12,
     column: 0 },
  message: 'Config file load failed',
  domain:
   { domain: null,
     _events: { error: [Function] },
     _maxListeners: 10,
     members: [ [Object] ] },
  domainThrown: true,
  stack: undefined }
复制代码
我的_config.yml配置文件是一个空行，所以错误肯定在前面，经过对比发现，我前面修改了一下 # Site的各项设置，在冒号:后面没留空格导致了该问题，请对比一下下面的区别：

错误的设置：

author:Zhchnchn
email:XXX@qq.com
language:zh-CN
正确的设置：

author: Zhchnchn
email: XXX@qq.com
language: zh-CN
3. 各个主题下的目录介绍（hexo\themes\下的modernist主题为例）

4.2 安装主题

Hexo提供了很多主题，具体可参见Hexo Themes【4】。这里我选择使用Pacman主题。具体设置方法如下【5】

4.2.1 安装

1. 将Git Shell 切到/D/Hexo目录下，然后执行下面的命令，将pacman下载到 themes/pacman 目录下。

$ git clone https://github.com/A-limon/pacman.git themes/pacman
2. 修改你的博客根目录/D/Hexo下的config.yml配置文件中的theme属性，将其设置为pacman。

3. 更新pacman主题

cd themes/pacman
git pull
NOTE：先备份_config.yml 文件后再升级

4.2.2 配置

如果pacman的默认设置不能满足需要的话，你可以修改 /themes/pacman/下的配置文件_config.yml来定制。

各个config的含义，请参考【5】中的介绍。

4.2.3 评论框

静态博客要使用第三方评论系统，pacman配置了多说评论系统（/themes/pacman/_config.yml），默认关闭，只要将其打开即可:false->true。直接用你的微博/豆瓣/人人/百度/开心网帐号登录多说，即可发表平评论。

#### Comment
duoshuo: 
  enable: true  ## duoshuo.com
  short_name:    ## duoshuo short name.
4.2.3 统计

1.  pacman配置了google analysis系统（/themes/pacman/_config.yml），默认关闭，将其打开。

2. 需要注册google analysis服务，以获得 跟踪 ID。

如果已有google账户的话，可以直接注册。注册时，需要正确填写 网站的URL。注册成功后，会得到一个跟踪ID，以及一段跟踪代码。

3. pacman配置了google analysis系统，将其打开

#### Analytics
google_analytics:
  enable: true
  id: UA-57032437-1  ## e.g. UA-1766729-8 your google analytics ID.
  site: auto ## e.g. yangjian.me your google analytics site or set the value as auto.
4. 在themes\pacman\layout\_partial\google_analytics.ejs 中，已经将google的跟踪代码添加进来了【3】。

复制代码
<% if (theme.google_analytics.enable){ %>
<script type="text/javascript">
(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
})(window,document,'script','//www.google-analytics.com/analytics.js','ga');
ga('create', '<%= theme.google_analytics.id %>', '<%= theme.google_analytics.site %>');  
ga('send', 'pageview');
</script>
<% } %>
复制代码
而且会将/themes/pacman/_config.yml中的id和site值读取进来。

5. 如果设置不起作用，请试试在\themes\pacman\layout\_partial\head.ejs文件中最后，</head>之前，添加上下面的语句试试。

<%- partial('google_analytics') %>
4.3 Custom 404页面

1. 网上大多数教程都将其说的极其简单：“直接在根目录下创建自己的 404.html 就可以”。但我却在这儿废了不少时间，究其原因是大家觉得太简单而说的不够明白。“根目录下”指的不是Hexo目录下，而是Hexo/source目录下。

2. 404.html的内容可以设置为下面的内容【6】（NOTE： _config.yml中的permalink_defaults属性不需要修改）。
4.4 安装插件

4.4.1 sitemap插件

1. 可以将你站点地图提交给搜索引擎，文件路径\sitemap.xml。

2. 安装

$ npm install hexo-generator-sitemap
3. 启用，修改Hexo\_config.yml，增加以下内容

复制代码
# Extensions
Plugins:
- hexo-generator-sitemap

#sitemap
sitemap:
  path: sitemap.xml
复制代码
4. 使用方法

（1）访问 http://localhost:4000/sitemap.xml，即可看到站点地图。

（2）那么怎么将它显示在页面中呢【7】？

可以修改themes/pacman（也就是你正在使用的那个theme）下的 _config.yml，在 menu 节点下添加下面的内容（下面要介绍的RSS插件也同样）

menu:
  Home: /
  Archives: /archives
  Rss: /atom.xml
  Sitemap: /sitemap.xml
修改后的效果如图所示：



5. 如何向google提交sitemap

Sitemap 可方便管理员通知搜索引擎他们网站上有哪些可供抓取的网页。向google提交自己hexo博客的sitemap，有助于让别人更好地通过google搜索到自己的博客。

如何向google提交sitemap，请参考【8】。

6. 升级插件

$ npm update
7. 卸载插件

$ npm uninstall hexo-generator-sitemap
4.4.2 feed插件

1. RSS的生成插件，你可以在配置显示你站点的RSS，文件路径\atom.xml。

2. 安装

$ npm install hexo-generator-feed
3. 启用，修改Hexo\_config.yml，增加以下内容

复制代码
# Extensions
Plugins:
- hexo-generator-feed
- hexo-generator-sitemap

#Feed Atom
feed:
  type: atom
  path: atom.xml
  limit: 20
复制代码
4.使用方法

参见sitemap插件介绍

5. 优化Hexo
5.1 添加“Fork me on Github” ribbon

给blog主页添加一个“Fork me on Github”的绶带（ribbon）【9】，比如选择了红色的ribbon，将相应代码复制到Hexo正在使用的theme下layout.ejs中。比如我使用的pacman theme，那么将下面的代码（注意将you改为你自己的github上的注册名）

<a href="https://github.com/zhchnchn"><img style="position: absolute; top: 0; left: 0; border: 0;" src="https://camo.githubusercontent.com/82b228a3648bf44fc1163ef44c62fcc60081495e/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f6c6566745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_left_red_aa0000.png"></a>
粘贴到 themes\pacman\layout\layout.ejs中，放置在 最后，标签</body>之前即可。

 

 

6 其他
6.1 中文乱码

在md 文件中写中文内容，发布出来后为乱码，原因是md的编码不对，将md文件另存为“UTF-8”编码的文件即可解决问题。

References
【1】Windows下Git安装指南（http://www.cnblogs.com/zhcncn/p/3787849.html）

【2】Hexo （https://github.com/hexojs/hexo）

【3】hexo你的博客（http://ibruce.info/2013/11/22/hexo-your-blog/）

【4】Hexo All Themes（https://github.com/hexojs/hexo/wiki/Themes）

【5】Pacman主题介绍（http://yangjian.me/pacman/hello/introducing-pacman-theme/）

【6】hexo添加404页面(http://ruocaiwu.github.io/2014/08/14/hexo%E6%B7%BB%E5%8A%A0404%E9%A1%B5%E9%9D%A2/)

【7】如何搭建一个独立博客——简明Github Pages与Hexo教程（http://cnfeat.com/2014/05/10/2014-05-11-how-to-build-a-blog/）

【8】如何向google提交sitemap（详细）(http://fionat.github.io/blog/2013/10/23/sitemap/)

【9】GitHub Ribbons(https://github.com/blog/273-github-ribbons)

分类: Browser Dev
标签: Browser Dev
好文要顶 关注我 收藏该文  
金石开
关注 - 9
粉丝 - 33
+加关注
8
« 上一篇：VirtualBox中安装CentOS-6.6虚拟机
» 下一篇：Sublime Text 3安装与使用
posted @ 2014-11-14 17:57 金石开 阅读(29881) 评论(8) 编辑 收藏
评论列表
  
#1楼 2015-06-20 21:00 刘岩石  
NOTE2：

如果你是为一个项目制作网站，那么需要把branch设置为gh-pages
请问这句话是什么意思？要是我想建立一个 读书 的栏目呢？
支持(0)反对(0)
  
#2楼 2015-06-20 21:26 刘岩石  
还有的就是我写了branch：gh-pages部署在github分支也成功了，但是怎么通过github.io访问的时候显示失败，也就是怎么显示出来分支的内容
支持(0)反对(0)
  
#3楼 2015-06-20 21:28 刘岩石  
请问我在本地还要生成一个gh-pages分支吗？
支持(0)反对(0)
  
#4楼 2015-06-20 21:35 刘岩石  
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
这里的新建页面的意思是？和新建文章有什么区别吗？
支持(0)反对(0)
  
#5楼 2015-06-20 21:37 刘岩石  
http://segmentfault.com/q/1010000000618915 解决了
支持(0)反对(0)
  
#6楼 2015-09-11 15:52 Vsdrop  
1
2
3
4
5
6
使用SSH url，如果电脑没有开放SSH 端口，会致部署失败。
 
fatal: Could not read from remote repository.
 
Please make sure you have the correct access rights
and the repository exists.


请问这个问题在 win7 下如何解决 卡在这里了
支持(0)反对(0)
  
#7楼[楼主] 2015-09-14 16:57 金石开  
@ Vsdrop
你是在公司配置的吗？公司的网络一般在域中已经把SSH端口禁掉了。你最好在自己的机器上配置。
支持(0)反对(0)
  
#8楼 2016-03-07 11:37 我不是管哥  
@ 刘岩石
请问你这个问题是怎么解决的呢？我现在想把public下面的文件放在master分支下，其他的文件都放在新建的hexo分支下，这个怎么弄呢？
什么情况下需要新建gh-page分支呢?
支持(0)反对(0)
刷新评论刷新页面返回顶部
注册用户登录后才能发表评论，请 登录 或 注册，访问网站首页。
【推荐】50万行VC++源码: 大型组态工控、电力仿真CAD与GIS源码库
【推荐】融云即时通讯云－豆果美食、Faceu等亿级APP都在用

最新IT新闻:
· Gear VR太小儿科 三星宣布研发不插手机独立虚拟现实头盔
· 三星电子第一季度净利润45.6亿美元 同比增长13.6%
· 你不知道的关于计算机大师Dijkstra的事情
· 手机销号后五类捆绑须解除
· Facebook第一季度净利润15.1亿美元 同比增长195%
» 更多新闻...

最新知识库文章:
· 架构漫谈（九）：理清技术、业务和架构的关系
· 架构漫谈（八）：从架构的角度看如何写好代码
· 架构漫谈（七）：不要空设架构师这个职位，给他实权
· 架构漫谈（六）：软件架构到底是要解决什么问题？
· 架构漫谈（五）：什么是软件
» 更多知识库文章...
历史上的今天:
2012-11-14 Little Tips
随笔档案(48)

2015年9月 (1)