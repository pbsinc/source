---
title: hexo换电脑迁移操作
date: 
categories: 教程
tags: hexo迁移
---
实验室要换新电脑，尴尬了，一直拖着没学的hexo迁移问题不得不解决了...下面就是用的教程：
<!-- more -->
**github上创建source仓库：**
把所有post的md放到github上，备份了；
http://nodlee.com/2015/06/07/how-to-porting-hexo/#more

注：我把hexo的文件夹压缩后保存在百度云，每次下下来解压就好，网站的主题样式等配置全都备份了相当于；

**在新电脑上：**
**1.配置hexo：**
Hexo安装和搭建依赖Nodejs(https://nodejs.org/en/)和Git(https://git-scm.com/),可自行下载。
然后在桌面运行Git bash;
使用以下命令安装hexo到全局

	$ npm install -g hexo
然后输入命令hexo -v输入hexo的版本号即为安装成功。

除此之外，相信大多数人都知道，要想使用git命令来和github进行提交部署等操作，需要进行一些配置，大概就是下面一些命令，如不明白请自行搜索。

	git config --global user.email xxx@163.com
	git config --global user.name xxx
	ssh-keygen -t rsa -C xxx@163.com(邮箱地址)      // 生成ssh
	找到.ssh文件夹打开，使用cat id_rsa.pub    //打开公钥ssh串
	登陆github，settings － SSH keys  － add ssh keys（把上面的内容全部添加进去即可）	
	
**2.获取源文件source文件夹:**
先从百度云把hexo压缩包下下来解压，网站主题这些都不用变；
接下来是将Github中的源文件抓取到新设备的Hexo目录中，具体操作如下：

进入hexo初始化目录，删除source文件夹(如果新设备中已经有文章数据，建议先备份，等抓取完成后，再拷贝到source文件夹中)，避免和远程仓库名字冲突。
右键，选择 git bash。
输入命令 git clone http://github.com/<username>/source.git克隆Github上的源文件到当前目录。
现在博客的源文件已经更新到新设备上了，我们测试一下，重新生成(hexo generate)、发布(hexo deploy),然后在网页端检查是否发布成功。	
	
	
**3.部署到github:	**
上面所有的操作完成之后，你就可以将你的Blog项目部署到github上了。

然后像往常一样，进入folder运行Git bash，然后使用以下命令进行部署。

	$ hexo deploy
	
备注：如果执行上述命令报错，你可以试试下面这个命令再试。

	$ npm install hexo-deployer-git --save
第一次部署的时候会提示输入github的账号和密码。	
http://luckykun.com/work/2016-04-23/heoll-hexo.html


**每次写新文章后更新源文件source：**
如新文章为abc.md

	1.也可输入命令 hexo new post "abc"创建文章。(直接复制方便)
	2.进入 source 文件夹，右键，选择 Git bash.
	3.输入命令 git status，查看本地仓库状态。
	(在状态报告中可以看到新建的 abc.md 文件出现在“Untracked files”下面。未跟踪的文件意味着Git在之前的提交中没有这些文件。)
	4.输入命令 **git add . **添加新文章到暂存区域。
	5.输入命令 **git commit -m "备注信息" **提交新添加的文章至本地仓库。
	6.输入命令 **git push -f **强行提交至远程仓库。
	
回到Github检查新添加的文章是否添加成功。到目前为止，博客移植的主要三个操作已经介绍完毕。简单回顾下整个流程：第一步是上传旧设备中的源文件到Github中。第二步是获取Github上源文件。第三步开始写作，撰写文章，生成站点文件然后发布，更新源文件到Github。

不论是在旧设备还是新设备，每次在写作前记得先抓取远程数据更新到本地，写作完成后更新新内容到远程，慢慢的养成更新习惯。