---
layout: post
title: "git-jenkins测试构建"
date: 2018-07-12 10:00:39
categories: jekyll update
img: we-in-rest.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [git, jenkins]
---


### 实践

- 首先要安装git，jenkins也是肯定要安装就不说啦

- 接着要再jenkins的`全局工具`中配置git的安装路径；在`系统设置`中有个git plugin 这里配置自己的git账号名字和邮箱（不一定必要）

- 然后就是创建项目，这里是新建了maven项目，在源码管理中配置git（这里实在是困扰了好半天），再添加Credentials的时候要添加
![](\image\git.png)
  
  注意：这里username可以随意些，下面要添加私钥，私钥是不带pub的那个！里面的所有内容都要添加上，包括“注释”！

- 然后还是跟之前一样的配置

- 完成之后构建成功，哈哈

### 注意

- 配置真的是一个相当繁琐的事情，稍不留意就混乱了，这里在添加秘钥的时候，要在jenkins安装的服务器上生成，然后要将公钥添加到gitlab上，私钥用在项目配置中；
- 除此之外因为开发可能在另一个平台上，在该平台上也要安装git，生成一下秘钥，然后将公钥添加到gitlab的项目里
- 在构建项目时pom文件中要注释掉scope是test那一行



#### 总结

​	最近一直工作比较多，也没有去实践新的东西，每次测试尝试不同的方式，总能发现一些问题，归根到底还是因为测试场景不够丰富，前段时间看腾讯的精准测试收益很多，和我的想法很一致，每一次测试都应该有针对性，以达到事半功倍的效果。在testerhome上也看了很多，大家都着急着往测试开发的方向发展，我忽然很迷茫，而偶然在《Google软件测试之道》看到一句，测试是为了让那些不懂测试的人更好的测试，这让我一下子心静下来，无论这个行业如何发展，都是本着这一点原则，那么我相信，努力做好自己的工作，让项目能更顺利的上线，这就是意义。



解决办法参考：

https://www.jianshu.com/p/ed0edb93e234

https://blog.csdn.net/xiaoguanyusb/article/details/79905319