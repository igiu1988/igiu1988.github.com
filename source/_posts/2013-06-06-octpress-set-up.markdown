---
layout: post
title: "Octopress配置"
date: 2013-06-06 12:30
keywords: octopress配置, 个性化, set up
comments: true
categories: 
---

##修改默认的markdown文件
每次使用rake post时产生的新文件中都有如下格式：

<pre>
layout: post
title: "Octopress配置"
date: 2013-06-06 12:30
comments: true
keywords: octopress		//默认是没有这行的
categories: 
</pre>
如果想自定义这里的内容，比如添加一个key，<!--more-->比如上面的keywords，那么就修改Rakefile，搜索`layout:`你就知道接下来怎么做了。
##首页只显示正文
如果你只想让博客首页在显示该博文时只显示到某一处，那就在那儿添加一个` <!--more-->`。首页会多出一个继续阅读的链接，试一试就知道了。

##推荐几个关于配置这方面的链接

* [我的Octopress](http://www.yanjiuyanjiu.com/blog/20130402/)
* [octopress官方文档](http://octopress.org/docs/)
* [octopress的个性化配置](http://linyi.herokuapp.com/blog/config-octopress.html)
* [涉及导航，分类，sina，评论](http://whbzju.github.io/blog/2013/03/01/octopress-custom-config/)
* [Octopress易筋经，中文分类标签](http://khaos.github.io/blog/2012/12/06/using-chinese-category-tags-in-octopress/)

##迁移你的博客系统

* [Clone Your Octopress to Blog From Two Places](http://blog.zerosharp.com/clone-your-octopress-to-blog-from-two-places/)
