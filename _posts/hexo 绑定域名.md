---
title: hexo 绑定域名
date: 
categories: 教程
tags: 域名
---
去阿里云看了下 pangbo.space 只要5块，果断拍下...下面就是用的教程：
<!-- more -->
**域名解析：**

有了自己的域名之后，luckykun.github.io替换成luckykun.com，只要设置下解析即可，进入万网的云解析页面，添加如下解析：
![这里写图片描述](http://7xtawy.com1.z0.glb.clouddn.com/domain22.png?_=0.8093272649489154)
说明：192.30.252.154和192.30.252.153是github服务器对应的ip地址，这步一定要设置，否则访问不了。


**添加CNAME：**
然后回到博客项目根目录，在source/下新建一个名为CNAME的文件，里面的内容写入luckykun.com即可。

最后，hexo g, hexo d.