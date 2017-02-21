---
title: "JWS - 可同步Jekyll文章的Wordress插件"
tags: [Jekyll,Wordpress,Github API,PHP]
---

我有两个域名：
- [kyshel.com](http://kyshel.com)
- [kyshel.me](http://kyshel.me)

其中，.com是Wordpress搭建的博客，而.me是Jekyll驱动的博客，它放在Github Pages上。    
在接触Jekyll之后，我就喜欢上了直接用text editor徒手写markdown，然后把写好的文件直接放在_post目录里就好，Github自动生成页面，.me也随之更新，简单粗暴。

也因此，.com搁置了好久都没有更新。

所以，我就想能不能写个插件，实现他俩的同步呢？    
这个时候，GitHub API就派上了用场，我可以通过api，GET到Jekyll的文章数据，然后比对Wordpress的文章，进行同步。

插件地址：[github.com/kyshel/jekyll-wordpress-sync](https://github.com/kyshel/jekyll-wordpress-sync)    
演示视频：[http://player.youku.com/embed/XMjUyMzMzODI4NA==](http://player.youku.com/embed/XMjUyMzMzODI4NA==)

目前已完成单向同步，即Wordpress的文章跟随Jekyll来更新，文章比对暂时单纯地利用文件名来判断两端文章异同。在Wordpress后台中，点击Analyze进行分析，分析完确定信息无误，点击Sync开始同步，大约会花费十秒到几十秒的时间（取决于Wordpress所在服务器与GitHub API服务器的响应时间），就可以看到Sync Success，这时候就可以去post列表中查看导入后的文章了。

目前这个插件还在develop中，待加的功能有 
- 利用Github的Webhook功能，实现更新Jekyll的文章后，同时自动更新wordpress的文章
- 显示当前Github API的可用响应次数


我把插件提交到了wordpress.org，在这里可以查看提交记录：[Jekyll Wordpress Sync - 一个Wordpress插件的提交跟踪](http://kyshel.me/2017/02/16/Jekyll-Wordpress-Sync-tracking/)

