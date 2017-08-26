---
layout: post
title: 使用github-pages搭建博客
tags:
- github-pages
- Jekyll
- 学习心得
- 博客创建
categories:
- practical skills
---
## 学习技巧
 以后有机会会专门写一个有关学习某个知识点或某门技术的整个学习心得，这里鉴于大家主要还是学习github-pages的搭建方法，这里就简单介绍下自己学习技巧。
 
<!-- more -->

个人学习一门技术或知识的过程一般是先借助wiki，了解这门技术是什么，干什么用，有什么好的特性，然后结合官方文档学习。个人觉得看官方文档是最权威的，看别人写的blog只能是作为辅助作用，帮助理解以及某种情况下提高效率。帮助理解很大程度上是针对官方文档是英文的（哈哈，其实基本上都是英文的，很少有中文），自身英文水平不高的情况，这种情况的同学可能会出现对某些英文句子的理解错误，导致看文档很吃力，所以借助别人的blog帮助理解是很有必要的。提高效率是因为官方文档都是覆盖整门技术的知识点的讲解（譬如待会讲到的jelly，其中就包括配置，模板等等），但其实开始学习我们并不需要把所有的知识点都学一遍。我们只需要先学好一个框架就好。

这里我用待会会讲到的jekyll作为例子。jekyll 的configuration有很多options,但其实正常使用的话里面很多options很少会用到，或者根本不会用到。譬如里面的Time Zone（我没有用过）。所以我们并没有必要把官方文档详细的看一遍，第一浪费时间，第二很多类似的配置我们可以在需要用到的时候再去看也不迟。（有学过spring等框架的人应该都明白大部分的配置信息其实都是用到的时候再去找，然后copy改。当然前提是你一定要知道如何去找）这也是我写代码和学习领域知识的一个观点，叫**"应用大于配置"**。所以看别人的blog可以大概清楚那些方面是重点，摸清大概的一个框架，而且可以绕过别人走过的弯路（有些人的blog中会写到自己在学习过程中走的弯路或者该注意的点），节省更多的时间学习起来会更加得心应手。下面的讲解我会渗透以上观点帮助理解。

## 使用github-pages搭建博客
### git配置
有人说git是版本管理的未来，我很支持这种说法。确实git带给我们很多便利，也使得开源有更多的可能性。  
首先安装git客户端，下载地址点击[官方网址](https://git-scm.com/downloads),选择对应系统的版本。关于git的设置以及ssh生成等，可以点击[官方帮助文档](https://help.github.com/articles/set-up-git/)，这里不在赘述。也可参考文章最下面的推荐参考链接，里面也有提到，不过建议看官方文档，比较详细。
### 创建博客仓库
git客户端安装后，在你放仓库的目录下，新建以“github用户名.github.io"为名的目录，(这个目录名也是你后面要建立的仓库的名字，这种类型的pages是属于user /organization pages,还有一种是project pages，两种的区别可看官方[官方帮助文档](https://help.github.com/articles/user-organization-and-project-pages/)，写的很详细)接下来输入下面命令（Git Bash下输入）。

    $ mkdir github用户名.github.io                      //新建目录
    $ git init      //在该目录下初始一个本地仓库，会自动新建一个git文件夹
    $ git checkout -orphan master   //创建一个没有父节点的master分支,注意这种
                                      user pages只能用master分支.
									  
*接下来就可以新建自己的博客目录了。*基本目录如下：  
_includes  //默认的在模板中可以应用的文件  
_posts    //博客文章存放的位置  
_layouts  //默认公共页面存放位置  
.gitignore  //git将忽略这个文件中列出的匹配的文件或文件夹，不将其纳入源码管理  
_config.yml //jekyll模板引擎的配置文件  
index.html  //默认的主页  
**目录结构具体内容的构建会可看[这篇博文](http://www.ezlippi.com/blog/2015/03/github-pages-blog.html)，下面用jekyll构建的pages会有相应的介绍，所以这里先不赘述。**
**接下来在远程github网站创建以**“github用户名.github.io”为名的repository。
然后再次打开Git Bash，将刚建好博客仓库提交上去。下面是具体命令：

    $ git add .    //将当前改动暂存在本地仓库
    $ git commit -m "提交描述"   //将暂存的改动提交到本地仓库，“”为本次提交的描述信                                 息
    $ git remote add origin https://github.com/username/项目名（github用户名.github.io）.git       //将远程仓库在本地添加一个引用：origin
    $ git push origin master  //向origin推送master分支，该命令会把本地master分支推到github远程仓库
	
**到此为github-pages的博客搭建的大概流程，输入网址：https://github用户名.github.io即可进入自己的博客页面。了解大概的流程后下面介绍具体的博客目录创建和博客文件的配置**
### 利用jekyll解析pages

**本地jekyll安装:**  
github page官方推荐jekyll生成器，你每次上传博客文章前，需要先在本地做测试，测试成功后再上传到远程github中，保证博客不会出错。所以你本地需要搭建jekyll测试环境。  

因为jekyll本身是基于Ruby开发的，所以需要先安装Ruby的开发和运行环境。首先先安装Ruby。[官网下载地址](http://www.ruby-lang.org/zh_cn/downloads/)，根据系统平台选定版本。windows可以[使用Rubyinstaller安装](http://rubyinstaller.org/downloads)。  

其次是安装RubyDevKit,最新版本的Ruby是要安装MSYS2,可以看[官方链接](http://rubyinstaller.org/downloads)里面的WHICH DEVELOPMENT KIT?里面有MSYS2的安装教程和下载地址。**最新ruby2.4版本自带gems,不需要安装，如果是低版本需要安装rubygems可以看[这个链接](https://rubygems.org/pages/download)**。
*Ruby运行环境搭建完后即可开始jekyll的安装。*windows中可在在cmd中运行以下命令：

    # Install Jekyll and Bundler gems through RubyGems
    ~ $ gem install jekyll bundler

    # Create a new Jekyll site at ./myblog
    ~ $ jekyll new myblog

    # Change into your new directory
    ~ $ cd myblog

    # Build the site on the preview server
    ~/myblog $ bundle exec jekyll serve

    # Now browse to http://localhost:4000  
	
**至此jekyll本地环境搭建完成，接下来就是个人博客的设计了**  

### 博客设计
利用jekyll解析博客的对应文件配置，可以到[官方文档](https://help.github.com/categories/customizing-github-pages/)查看。我自己是套用别人的模板，github上提供一个[页面](https://github.com/jekyll/jekyll/wiki/sites),里面提供了许多模板可供参考。我自己是使用了一个叫jacman主题的模板，本人觉得外观不错就套用了。如果你们觉得我的blog还可以，也可以到我的github中clone。jacman主题的模板可在github上搜索Jacman就有了。下面是Jacman的主页截图：  
![首页图]({{site.baseurl}}{{ site.assets }}/images/jacman.png)  

**PS:**Jacman里面有一个评论的控件，我使用的是DISQUS,有时候会被墙，有科学上网就没有问题了。这里需要注册DISQUS账号，账号注册完以后在右上角有一个齿轮的标记 setting，在下拉列表中选择 add disqus to site，需要填写 Site URL，Site Name，Site Shortname，完事后就注册了一个账号。
这时候网址会自动跳转进<你注册的名字>.disqus.com，这时候，下面的标签页有comments，discussions，analytics，setting 选择 setting ，选择 advanced 在下面的Trusted Domains中，添加 gitbook.com，gitbooks.io（一行一条网址，不用加www）
## 绑定自己独立域名
我是在[Godaddy](https://sg.godaddy.com/?ci=)上申请的域名，网上评论都还不错，也有挺多的优惠券，是个不错的选择，优惠券自己可到网上找，挺多的。  

域名购买完成后配置域名解析，网上流传Godday的域名解析偶尔被墙，导致域名无法解析，所以我选择了[DNSPod](https://www.dnspod.com/),他们的产品做的不错。  

使用方法就是，第一注册登录，可看[帮助文档](https://www.dnspod.com/support/index/fid/1)。
第二在Godaddy中修改自己的DNS服务,可看[dnspod文档](https://www.dnspod.com/support/index/fid/119)。
第三就是在DNSPod添加自己的域名记录：可看[github文档](https://help.github.com/articles/quick-start-setting-up-a-custom-domain/)。
最后在github中配置自己的域名：[网址](https://help.github.com/articles/adding-or-removing-a-custom-domain-for-your-github-pages-site/)。  

以上关于绑定域名都可在github help中看到:[网址](https://help.github.com/articles/quick-start-setting-up-a-custom-domain/)。
**至此blog的搭建完成！！！**
  
**PS:这篇博文主要还是告诉大家学习github-pages需要到哪里找对应资料，对应的官方链接较多，也是想告诉大家，关于这类问题的学习最好最权威的学习方式就是看官方帮助文档，看别人写的博客原因文章开头有谈到，当然还有一点就是别人写的博客毕竟是属于二手资料，有可能出现错误的地方，就好比你抄写课本总会有抄错字的时候。所以官方文档是最好的学习手段，重要的事情多少几遍！**

*还有一点想要说的是自己在找博客读的时候尽量找一些连载的博客，说明这个作者对这方面研究较多，这可以提高找到有价值资源的概率。*

**===============   
 最后如果觉得我的blog写的不够好欢迎指出，以后会改进，感谢。实在看不下去(哈哈)，可以看下面的参考，我就是利用下面的链接学习的，我可以做得，你也可以！！  
 ====================**   

    
**参考：**  
       
[个人参考过的博客，关于githubpages的搭建。推荐](https://www.ezlippi.com/blog/2015/03/github-pages-blog.html)   
[jekyll 官方帮助文档](http://jekyllrb.com/docs/home/)  
[利用jekyll生成器的_config.yml文件的配置](http://jekyllrb.com/docs/configuration/)  
[利用jekyll生成器的博客页面配置](http://jekyllrb.com/docs/templates/)  
[github pages 基本配置官方帮助文档](https://help.github.com/categories/github-pages-basics)  
[自定义github pages，包括使用jekyll作为pages的生成器](https://help.github.com/categories/customizing-github-pages)  
[github 官方帮助文档](https://help.github.com/)  
[github pages 基本配置官方文档](https://help.github.com/categories/github-pages-basics/)  

    
    

 



