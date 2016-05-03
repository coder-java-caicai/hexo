---
title: hexo+github搭建个人博客
date: 2016-04-28 17:20:21
tags: nodejs
---
hexo+github搭建个人博客

字数841 阅读543 评论12 喜欢18
现在一个程序猿（媛）没有一个自己的博客都不好意思说自己是程序员，哈哈开玩笑的。是否有一个方法，可以让我们自己创建一个属于自己的博客，然后又不用花钱买服务器和域名，也不用自己找人去设计自己的网站呢。
这样的好东西还真的存在，而且配置还十分简单，下面我就详细的介绍如何用hexo+github搭建自己的（酷炫）博客。
前期准备

node.js
如果你是windows,请戳这里
如果你是mac,请戳这里
git账号
如果没有git帐号，请戳这里
安装hexo
npm install -g hexo
初始化hexo
hexo init
npm install hexo --save
生成静态页面至hexo\public\目录。
hexo g
本地启动服务
hexo server
这样，我们就可以在浏览器中输入http://localhost:4000/ 访问我们的博客啦（响应式的网站）。

初始界面-PC.png

初始界面-移动端.png
虽然博客基本的已经搭好了，但是我们只能在本地访问，其他人是看不到的，下面我们通过和git绑定来实现我们想要的效果。

配置github
新建一个仓库名（该仓库名和你的用户名对应），如我的git账户名是：coder-Yin，则我的仓库名为coder-Yin.github.io
编辑_config.yml文件，建立与git的关联(在.yml文件的最底部)
# Deployment## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/coder-Yin/coder-Yin.github.io.git
  branch: master
然后运行
npm install hexo-deployer-git --save
hexo g
hexo d
这样你就可以在你的 coder-Yin.github.io 上看到代码已经同步到git上了。
在浏览器中输入你的**.github..io(例如：http://coder-yin.github.io/）

访问效果.png
每次有新的修改需要部署同步，都可以按照下面的步骤来：
hexo clean
hexo g
hexo d
如果你觉得hexo默认的主题不好看，你可以通过以下方法来修改你的主题。

下面我通过修改一个主题来给大家做个介绍：
在git上找到你想要的主题
我这随意找了一个，比较适合女孩子（缺点:不是自适应的）
https://github.com/daisygao/hexo-themes-cover
进入你的hexo目录，执行命令，拷贝主题
git clone https://github.com/daisygao/hexo-themes-cover.git themes/cover
拷贝完成后，你会发现你的项目下的themes下多了一个cover文件夹
我们还需要修改_config.yml文件中的一处来应用新的主题
# Extensions## Plugins: http://hexo.io/plugins/## Themes: http://hexo.io/themes/
theme: cover
然后我们重启服务就可以在本地看到效果了
hexo server

hexo应用新的主题.png

注意：我们这样只是本地做了修改，git上并没有实现同步，我们需要按照上面所说的，依次执行以下命令实现部署同步：
hexo clean
hexo g
hexo d
在刷新你的http://***.github.io/ 就可以发现新的主题应用成功了，是不是很简单，快动手建立你自己的博客吧。
最后，附上更多的hexo主题，大家可以很戳这里选择你自己喜好的主题。