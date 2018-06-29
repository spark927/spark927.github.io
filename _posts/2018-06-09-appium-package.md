---
layout: post
title:  "Appium自动化测试-封装参数"
date:   2018-06-09 14:51:30
categories: jekyll update
img: # Add image post (optional)
tags: [appium]
---


by spark

2018/5/28

​	手机参数设置的封装，这里采用csv进行数据保存，java代码实现，如下：
{% highlight java %}
ArrayList<String> array = new ArrayList<String>();
		int i = 0;
		try {
			BufferedReader reader = new BufferedReader(new FileReader("dc.csv"));
			String line = null;    
			while((line=reader.readLine())!=null){    
				String item[] = line.split(",");//CSV格式文件为逗号分隔符文件，这里根据逗号切分  
				String first = item[item.length-2];
			    String last = item[item.length-1];//这就是你要的数据了   
			    
			    array.add(last);
			    i++;
			}
			System.out.println(array);
			return array;
		} 
{% endhighlight %}
​	这里顺便复习了一下java的知识

```
//定义String类型的数组
ArrayList<String> array = new ArrayList<String>;
//数组中添加数据
array.add("");
//读取数据
array.get(index);
```



​	学习过程中发现中文输入为空，查找之后发现要加代码，如下；

```
cap.setCapability("unicodeKeyboard", true);
cap.setCapability("resetKeyboard", true);
```

ps：之后每次执行会弹出来安装输入法，只需将手机的默认输入法改成Appium的即可。