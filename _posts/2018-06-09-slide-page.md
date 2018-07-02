---
layout: post
title:  "Appium滑动页面"
date:   2018-06-09 15:51:30
categories: jekyll update
img: how-to-start.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [appium]
---


by spark

2018/6/5

​	今天继续开始写自动化的测试，主要解决了滑动页面找想要的元素问题。

​	想要的效果是，要点击列表中某元素，如果在当前页面没有就向上滑动，知道找到为止。

代码如下：

{% highlight java %}
	/**
	 * 每次向上滑动100像素,并判断是否滑到底部
	 * @param driver
	 */
	public static void slidePage(AppiumDriver driver) {
		int x = driver.manage().window().getSize().height;
		int y = driver.manage().window().getSize().width;
		int xCenter = x/2;
		int yCenter = y/2;
		int yEnd = yCenter-100;
		
		String beforeSource;
		String afterSource;
		do {
			beforeSource = driver.getPageSource();
			driver.swipe(xCenter, yCenter, xCenter, yEnd, 300);
			afterSource = driver.getPageSource();
		} while (!afterSource.equals(beforeSource));
		
	}
	
	/**
	 * 向上滑动页面知道找到指定元素
	 * @param driver
	 * @param name
	 */
	public static void findElement(AppiumDriver driver, String name) {
		boolean flag = false;
		do {
			try {
				driver.findElement(By.xpath("//android.widget.TextView[contains(@text,'"+name+"')]"));
				flag = true;
			} catch (Exception e) {
				slidePage(driver);
				flag = false;
			}
		} while (flag == false);
	}
{% endhighlight %}

遇到问题：

- 上滑的问题比较好解决，都有完整的方法，可以直接拿来用
- 寻找元素写循环的时候就比较坎坷了，一直没找对方法，导致一直报异常，后来想起来xpath的语法可以利用，try-catch可以处理异常，这个过程卡了挺长时间，仔细一想，除了自己对appium的使用还不熟练之外就是对java的掌握还差很多，如果掌握比较好应该可以很快想清楚应该怎么写，还需要努力。

