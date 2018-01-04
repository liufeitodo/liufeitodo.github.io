---
title: hexo + GitHub 搭建个人网站记录总结
copyright: true
top: 0
date: 2017-11-30 10:20:19
tags: [hexo,GitHub]
categories: GitHub
---

## 搭建个人网站记录总结

　　刚接触hexo时，如果没有教程安装部署是无从下手的，现在往上有很多相关的教程，详简程度也参差不齐。个人建议这篇文章，《GitHub+Hexo 搭建个人网站详细教程》 https://zhuanlan.zhihu.com/p/26625249  内容详细，时间上也不至于和现在脱节。
　　记录下经常使用的个性化设置备忘，和提高效率的方法，和遇到的不常见的问题。

<!--more-->

### 个性化设置备忘
#### 1. 隐藏网页底部powered by Hexo / 强力驱动
打开 themes/next/layout/_partials/footer.swig ,  使用 "<!-- --> "隐藏之间的代码即可，或者直接删除。如下：
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

### 提高效率
#### 1. hexo博客 添加文章时自动打开编辑器
新建博文非常简单，只需在所在hexo根目录下git bash 命令行键入： 
```
hexo new "new blog article"
```
回车即可在hexo 根目录 scource 文件夹下的 _posts 目录下自动生成相应的md文件。然后只需找到打开编辑就好。然而当博文比较多，再寻找成了一件比较费时的事情。
hexo 添加文章时自动打开编辑器就比较实用了。
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



### 问题与解决方法

#### 1. 部署失败 
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

​        显然是你的_config.yml出现问题，在deploy这里相应的内容检查是不是有问题，尤其要注意在 : 后面要有空格，网上很多人都出现过这样的粗心问题.
还有当同时部署到多个仓库，比如github 和coding，如下为例:

 deploy:
​     type: git
​     repo:
       github: git@github.com:username/username.github.io.git,master
       coding: git@git.coding.net:username/username.git,master
​        这里要注意一下：上面这段代码，设置的时候多数填写格式有问题。在repo下面两行用tab键缩进就行。如果还出现问题在repo下面输入法调至英文状态，用空格缩进，方可解决问题。
