---
layout: post
title: "Octopress Blog"
date: 2013-09-10 08:30
comments: true
categories: Octopress Markdown
---
  折腾了一天，blog终于搞定了，写点文字，纪念一下这该死的过程！
搭建过程有点不顺利，主要是由于ruby版本不一致以及蜗牛一样的网速（还经常断掉）导致的。
 
搭建环境
---
>工欲善其事，必先利其器

1、安装 GitHub   
下载地址：[https://help.github.com/articles/set-up-git](https://help.github.com/articles/set-up-git)，下载后直接安装就OK了。   
2、安装 Ruby    
我的Mac自带的是ruby 1.8.7(终端查看ruby版本：`ruby --version`)，按照官网要求安装ruby 1.9.3。    
安装方法：[http://octopress.org/docs/setup/rvm/](http://octopress.org/docs/setup/rvm/)
先安装RVM(Ruby Version Manager): 

    curl -L https://get.rvm.io | bash -s stable --ruby
    
在安装Ruby 1.9.3

    rvm install 1.9.3	#安装1.9.3版本的ruby
    rvm use 1.9.3 #如果有多个版本的ruby，使用1.9.3版本的
    rvm rubygems latest

3、Mou   
Markdown编辑软件，用于编辑文章，我现在就在用该软件编写了，很棒的，所见即所得。  下载地址：[http://mouapp.com](http://mouapp.com)   

安装Octopress
---
>一切按照官方的文档安装就OK了，官方文档地址：[http://octopress.org/docs/setup/](http://octopress.org/docs/setup/)

在终端输入git命令，安装Octopress，安装完成后跳转到 Octopress 所在目录中。

    git clone git://github.com/imathis/octopress.git  octopress
    cd octopress #跳转到 octopress 目录

安装依赖项

    gem install bundler #
    bundle install #会安装很多，这个地方最有可能失败了，没关系，多按几次就OK了。
    
如果之前你的ruby环境安装成功，那么此处会有绿色的字体提示。接下来安装默认主题：   

    rake install


在Github上创建一个新的Repositories
---
登录Github，创建一个新的存储库，命名为：`username.github.com`，只有这种格式的命名才能在运行命令 `rake setup_github_pages` 之后自动创建 **master**和**source**分支，否则为作为普通仓库生成 **gh-pages**分支。   
打开客户端软件Github，点击底部的 + -> Creat New Repositories…，填写   
Name: username.github.com   
Description: blog   
Local Path: 指定到磁盘的某一位置   
然后创建即可。

 <!-- more -->
 
发布Octopress到Github
---
在终端输入如下命令：

    rake setup_github_pages

1、按照提示输入刚才新建的仓库地址，类似：https://github.com/username/username.github.com.git    
2、接着输入以下命令：

    rake generate #生成静态页面
    rake preview #用来本地浏览，按Control+C终止。 

本地浏览默认网站为：[http://localhost:4000](http://localhost:4000)
3、输入以下命令，发布blog到Github上：

    rake deploy
    
这样blog生成的内容会自动发布到master分支，并且可以使用 [http://username.github.com](http://username.github.com)访问啦。
4、把所有源文件发布到source分支下：

    git add .
    git commit -m "message"
    git push origin source
    
到目前为止，blog以发布完成。可能有部分同学不会成功，没关系，回头再检查一遍上边的步骤，看看是不是有些地方设置有误。      
接下来对blog进行设置。

Ocotpress博客配置
---
###1、blog的基本设置    
blog的基本设置是在Octopress根目录的主配置文件`_config.yml`中，所以我们需要修改配置文件`_config.yml`。   
###需要特别说明的是: yml格式默认是：参数 + : + 空格，如果不加空格，编译会报错。

    url: http://wzrong.me 	# 博客地址
    title: Wzrong's Blog		# 博客标题
    subtitle: 感受生活，感悟工作，感触心灵. 	# 博客副标题
    author: 卫志荣	# 作者
    simple_search: http://google.com/search		# 搜索引擎
    description: 记录并沉淀自己的学习点滴。		# 关于博客的描述
    subscribe_rss: /atom.xml		# Rss订阅地址，默认为 /atom.xml
    email: wzrong@me.com	# Rss订阅的Email地址
    root: / 	# 博客路径，默认为 "/"，如果你的blog在子目录中，需要修改这个路径
    permalink: /blog/:year/:month/:day/:title/   # 文章的固定连接形式
    
###2、更换主题    
暂时不换了，换的时候再考虑吧。

###3、添加多说评论   
在 `_config.yml` 中添加   
    
    # duoshuo comments
    duoshuo_comments: true
    duoshuo_short_name: yourname
    
替换 `source/_layouts/post.html` 中的 `disqus` 代码

    { % if site.disqus_short_name and page.comments == true % }
        <section>
            <h1>Comments</h1>
            <div id="disqus_thread" aria-live="polite">{ % include post/disqus_thread.html % }</div>
        </section>
    { % endif % }
    
为下方的 `多说评论` 模块

    { % if site.duoshuo_short_name and site.duoshuo_comments == true and page.comments == true % }
        <section>
            <div id="comments" aria-live="polite">{ % include post/duoshuo.html % }</div>
        </section>
    { % endif % }
    
在路径 `source/_includes/post/` 下创建一个 `duoshuo.html` 页面，并加入以下代码：

    <!-- Duoshuo Comment BEGIN -->
    { % if site.duoshuo_short_name % }        
    <div class="ds-thread" data-title="{ % if site . title case % }{{ post . title | title case }}{ % else % }{{ post . title }}{ % end if % }"></div>
    <script type="text/javascript">
        var duoshuoQuery = {short_name:"{{ site.duoshuo_short_name }}"};
        (function() {
            var ds = document.createElement('script');
            ds.type = 'text/javascript';ds.async = true;
            ds.src = 'http://static.duoshuo.com/embed.js';
            ds.charset = 'UTF-8';
            (document.getElementsByTagName('head')[0] 
            || document.getElementsByTagName('body')[0]).appendChild(ds);
        })();
    </script>    
    { % endif % }
    <!-- Duoshuo Comment END -->    
   
然后再修改 `source/_layouts/post.html` 文件，在标签 `</article>` 下边添加以下`多说评论`代码：

    <!-- Duoshuo Comment BEGIN -->
    { % if site.duoshuo_short_name and page.comments == true % }
        <section id="comment">
            { % include post/duoshuo.html % }
        </section>
    { % endif % }	
    <!-- Duoshuo Comment END -->

到此 `多说评论` 在 Octopress 中已经设置完成，接下来去[多说网](http://duoshuo.com)注册一个账号，添加站点，获取 `duoshuo_short_name`。
    
###4、添加版权声明    
这里所说的版权声明是指添加在每篇文章后面的版权信息。    
首先在 `source/_includes/post`目录中添加 `license.html` 文件，内容如下：

    <!-- Copyright Info BEGIN -->
    { % if site.post_license % }
    < p class="post-footer">
        原文地址：< a href="{{site.url}}{{ page.url }}"> {{site.url}}{{ page.url }} </a >
        <br />
        <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/3.0/deed.zh" ></a>版权声明：自由转载-非商用-非衍生-保持署名 | <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/3.0/deed.zh" >Creative Commons BY-NC-ND 3.0 </a> | <img alt="知识共享许可协议" style="border-width:0" src="http://i.creativecommons.org/l/by-nc-nd/3.0/80x15.png" />
    </p>
    { % endif % }
    <!-- Copyright Info END -->
    
在 `sass/custom/` 中的 `_style.css` 文件中添加以下样式来设置版权信息的样式

    .post-footer{
        background: #ccc;
    }
    
修改文件 `source/_layouts/post.html`，在 `{ % include article.html %}` 下边添加 `{ % include post/license.html % }`    
在 `_config.yml` 添加配置项来控制是否显示页面的版权信息   

    # Post License
    post_license: true
至此版权信息添加完成，效果与本blog一致。

>特别说明：文中有遇到 `{ %` 与 `% }` 的情况时，请自行把`{`和`%`之间的空格去掉。由于编写时候有编译错误，故加了空格，抱歉。
