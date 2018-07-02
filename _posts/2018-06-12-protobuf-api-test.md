---
layout: post
title:  "Protobuf 简单例子"
date:   2018-06-12 13:51:30
categories: jekyll update
img: software.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [protobuf]
---


#### 介绍

​	Protocol Buffer是一种数据交互格式协议，其作用在于将现有格式的数据转换成二进制进行传输，速度更快，更适合设备与设备之间的数据交互。

#### 过程（此处以java为例）

1. java中的model是class中进行封装，而Protocol Buffer则是以一种message为关键字来创建类似结构体的格式将数据封装为.proto格式的文件，对比如下：

   ```
   java:
   class Login（）
   {
   	String Name;
   	String Pwd;
   }

   Protocol Buffer:
   syntax = "proto2"  #表示用了proto2的语法
   message login
   {
     String Name = 1;
     String Pwd = 2;
   }

   ```

2. 完成之后，将数据进行序列化时

{% highlight java %}
   Login.Builder builder = Login.newBuilder();
   	builder.setName("tom");
   	builder.setPwd("111111");
   	Login login = builder.build();

   login.toByteArray();  //将proto对象转换成字节流
{% endhighlight %}

3. 反序列化

{% highlight java %}
   byte[] array = login.toByteArray();  //将byte数据反序列化成proto对象

   try{
     Login login1 = Login.parseForm(array);
   } catch(InvalidProtocolBufferException e) {
     e.printStackTrace();
   }
{% endhighlight %}

   ​


参考：https://blog.csdn.net/briblue/article/details/53187780