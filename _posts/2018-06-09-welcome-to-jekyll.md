---
layout: post
title:  "Maven打包"
date:   2018-06-09 16:15:39
categories: jekyll update
---


maven使用时，先到官网下载包，然后在环境变量中配置即可正常使用。

maven 打包代码成jar：

​	进入项目的根目录下（/src）或者pom.xml同级的目录下

​	运行命令 

```
mvn clean package -DskipTests -P dev
```

​	打包完成后可在target文件夹下看到jar包