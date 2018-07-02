---
layout: post
title:  "Appium等待时间"
date:   2018-06-10 15:51:30
categories: jekyll update
img: how-to-start.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [appium]
---


by spark

2018/5/23

​	由于每次执行用例都可能会遇到空间加载时间的问题，往往会导致用例执行失败，非常恼火，直接用sleep也一样，无法控制好准确的等待时间，因此需要有方法来解决这一问题。

1. sleep()

2. 隐式等待 implicitlyWait()

{% highlight java %}
   driver.manage().timeouts().implicitlyWait(30,TimeUnit.SECONDS);
{% endhighlight %}

   全局等待30秒，不管元素是否加载完成

   - 如果没有找到元素，继续等待，超出设定时间后抛出找不到元素的异常
   - 当查找元素或元素并没有立即出现的时候，将等待一段时间再朝赵，默认时间是0
   - 它会存在于整个webdriver对象实例的生命周期，会让一个正常响应的应用变慢

3. *显式等待 WebDriverWait()* 

{% highlight java %}
   WebDriverWait wait = new WebDriverWait(driver, 60);
       WebElement e= wait.until(new  ExpectedCondition<WebElement>() {
               @Override
               public WebElement apply(WebDriver d) {
                   return d.findElement(By.id("q"));
               }
           })
{% endhighlight %}

   默认情况下，WebDriverWait每500毫秒调用一次ExceptionCondition，知道有成功的返回，如果超过时间没有成功返回，将抛出异常。

   该方式适用于selenium，如果要在appium中使用，需要改造。

   参考链接：https://www.cnblogs.com/tobecrazy/p/4596214.html

   

自己封装了一个方法，参考上链接

{% highlight java %}
public static void wait(AndroidDriver driver, String id) {
	WebElement we = new AndroidDriverWait(driver,30)
    		  .until(new ExpectedCondition<WebElement>() {
				@Override
				public WebElement apply(AndroidDriver d) {
					return d.findElementById(id);
				}
			});
}
{% endhighlight %}

实现过程中遇到的问题：

- 开始的时候找不到AndroidDriver，后来发现是java-client的版本太低，后来下载了新的版本，然后重新再Maven中配置，环境更新后，不再报错。
- 接着是遇到了包冲突的问题，网上查了之后，发现是因为用AndroidDriver不能引入appium-server包，否则就会报错。

