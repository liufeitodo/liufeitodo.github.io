
---
title: Hexo + GitHub 搭建个人网站记录总结
copyright: true
top: 0
date: 2017-11-30 10:20:19
tags: [hexo,GitHub]
categories: GitHub
---

### 安装部署
**Hexo**
	    Hexo是款基于Node.js的静态博客框架，方便生成静态网页托管在GitHub上。Hexo的创建者是台湾同胞，所以对中文支持很友好。

**仓库创建和插件安装**
* 创建个人仓库
    注册登录GitHub 创建仓库;
	
* 安装Git
    Git是开源的分布式版本控制系统，高效处理项目。
    [Git下载地址](https://git-scm.com/downloads )
    [Git教程]( https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
  
<!--more-->

* 将本地Git和GitHub个人帐号关联：
    右键菜单Git Bash 设置 user.name 和 user.email 配置
    git config --global user.name "GitHub用户名"
    git config --global user.email "GitHub注册邮箱"

* 安装Node.js
    [Node.js下载地址](https://nodejs.org/en/download/)
    安装后测试，命令行键入：node -v ，若出现版本号，则安装成功；
* 安装Hexo 
     Hexo安装个人博客的框架，创建文件夹，此文件夹用来保管个人博客的相关。
     使用npm命令安装Hexo,输入： npm install -g hexo-cli
     安装完成后，初始化博客，输入： hexo init 
* 常用的Hexo命令
    npm install hexo -g  安装Hexo
    npm update hexo -g  升级
    hexo init   初始化博客
    hexo new "我的博客"   新建文章
    hexo generate   生成
    hexo server   启动服务预览
    hexo deploy  部署
    hexo clean   清除缓存


### 高效设置
**隐藏网页底部powered by Hexo / 强力驱动**
 打开 themes/next/layout/_partials/footer.swig ,   使用 "< !-- -- > "隐藏之间的代码即可，或者直接删除。如下：

```
<!-- {% if theme.footer.powered %}
  <div class="powered-by">{#
  #}{{ __('footer.powered', '<a class="theme-link" target="_blank" href="https://hexo.io">Hexo</a>') }}{#
#}</div>
{% endif %}

{% if theme.footer.powered and theme.footer.theme.enable %}
  <span class="post-meta-divider">|</span>
{% endif %}

{% if theme.footer.theme.enable %}
  <div class="theme-info">{#
  #}{{ __('footer.theme') }} &mdash; {#
  #}<a class="theme-link" target="_blank" href="https://github.com/iissnan/hexo-theme-next">{#
    #}NexT.{{ theme.scheme }}{#
  #}</a>{% if theme.footer.theme.version %} v{{ theme.version }}{% endif %}{#
#}</div>
{% endif %}
-->
```

**hexo博客 添加文章时自动打开编辑器**
新建博文非常简单，只需在所在hexo根目录下git bash 命令行键入：  hexo new "new blog article"  ，回车即可在hexo 根目录 scource 文件夹下的 _posts 目录下自动生成相应的md文件。然后只需找到打开编辑就好。然而当博文比较多，再寻找成了一件比较费时的事情。
那么，hexo 添加文章时自动打开编辑器就比较实用了。
首先在hexo 根目录下 新建 scripts 目录，在此目录下新建JavaScript脚本文件。任意取名.js 。在脚本内写入以下内容：
```
var spawn = require('child_process').exec;
// Hexo 2.x 用户复制这段
hexo.on('new', function(path){
  spawn('start  "markdown编辑器绝对路径.exe" ' + path);
});
// Hexo 3 用户复制这段
hexo.on('new', function(data){
  spawn('start  "markdown编辑器绝对路径.exe" ' + data.path);
});
```

### 重装系统，或者不同电脑如何更方便的更新博客
Hexo一般部署到github上的文件类型，是转换后的html静态网页，如果常用的电脑重装系统，或者想在别的电脑上修改你的博文的时候，要么是在其他地方也有博文markdown文件备份，不然就比较麻烦了。看了很多这方面的相关解决方案，觉得分支是最方便的。
2个分支，一个用来保管html静态网页，一个用来保管源代码.

* **部署**：
1. 创建仓库，名称为：username.github.io .添加分支 “hexo” (随意取名用来存源代码) ;hexo 设为默认分支，（仓库中的setting 有相关操作） ;

2. 拷贝仓库，git clone git@github.com:xxxx/xxxx.github.io.git  ;

3. 将博客源文件粘贴到仓库内；主要拷贝： _config.yml 站点配置文件，themes 主题文件夹，source博客文章的源文件，scaffolds 文章的模版，.gitignore限定在push时哪些文件需要拷贝，哪些可以忽略；删除主题文件夹下的.git/,否则主题文件夹无法push;

4. 仓库内git bash (分支显示为hexo) 依次执行: npm install ,npm install hexo-deployer-git ;

5. 依次执行 git add . , git commit -m "message" , git push origin hexo 将博客源码提交部署到hexo 分支上;

6. 查看修改_config.yml 中的deploy参数，分支应为master ; 执行hexo g -d 生成静态网页并部署到GitHub上 ，master分支下 ;

username.github.io仓库下的 master和hexo分支，分别保存静态网页和源文件。 

* **更新**：
在本地进行 博客修改、主题样式修改、新文章撰写等:

1. 依次执行 git add . , git commit -m "message" , git push origin hexo , 部署本地文件到分支hexo;

2. 执行 hexo generate -d 生成网站并部署到 Github 上的 master 分支；

如果在github博客仓库页面出现英文显示uglify-js版本过低的的提示，在本地仓库中删除 package-lock,json文件。重新提交即可。

* **重装**：
重装系统，其他电脑更新博客:
1. 安装git，安装Nodejs和npm;
2. 拷贝博客仓库 git clone git@github.com:xxxx/xxxx.github.io.git  ；
3. 在仓库下，git bash 输入指令：npm install hexo-cli -g , npm install , npm install hexo-deployer-git. ;
4. 更新操作;


### 问题与解决方法

**部署失败**
更新博客时，需要生成和部署到git仓库，hexo generate正常， 但 hexo deploy 出现提示：

You have to configure the deployment settings in _config.yml first!
Example:
​    deploy:
​     type: git
​     repo: <repository url>
​     branch: [branch]
​     message: [message]
​     extend_dirs: [extend directory]
For more help, you can check the docs:  http://hexo.io/docs/deployment.html
INFO  Deploy done: git

显然是你的_config.yml出现问题，在deploy这里相应的内容检查是不是有问题，尤其要注意在 : 后面要有空格，网上很多人都出现过这样的粗心问题.
还有当同时部署到多个仓库，比如github 和coding，如下为例:

 deploy:
​     type: git
​     repo:
       github: git@github.com:username/username.github.io.git,master
       coding: git@git.coding.net:username/username.git,master
	   
这里要注意一下：上面这段代码，设置的时候多数填写格式有问题。在repo下面两行用tab键缩进就行。如果还出现问题在repo下面输入法调至英文状态，用空格缩进，方可解决问题。

