---
layout: post
title:  "Protobuf 接口测试-登录"
date:   2018-06-12 14:51:30
categories: jekyll update
---


#### 1. 测试环境

​	该项目是基于HTTPS协议以及protobuf传输格式，整个项目由3部分构成，硬件，后台，APP。


#### 2.说明

​	本次例子针对APP和后台之间的接口进行测试

#### 3.开始

​	首先已经了解了该如何编译proto文件，生成文件后开始写test。

- 接口测试采用了httpURLconnection发送https请求，采用testNG作为测试框架（看到网上也有用httpclient发送请求的例子，之后会再了解一下选择最简便的框架）
- 整个过程还是采坑很多
  - 首先要拿到接口文档，知道每个接口代表什么意思
  - 其次，要知道httpURLconnection是如何发送接收数据的，之前因为没有用InputStream用了其他的函数，结果测试结果返回400，后来才明白原来是因为数据没有写进去
  - 再次，因为用了https的加密协议，发送请求的URL必须是https的，直接用http服务器并不会回应，而这中间又存在加密证书的问题，了解到客户端发送请求可以绕过，在网上找到大牛的代码直接拿来用，效果很赞，自己还是会好好学习一下
  - 最后还有一个try-catch-finaly的问题，体现在在finaly中写return，调用该方法并不会返回数据，因为执行了finaly之后，程序会直接退出，返回值不是try或catch中保存的返回值。

主要代码如下：

```java
public class test {
	
	  public static InputStream http(String url, byte[] PostData) {
	        URL u = null;
	        HttpURLConnection con = null;
	        InputStream inputStream = null;
	        //post
	        try {
	            u = new URL(url);
	            con = (HttpURLConnection) u.openConnection();
	            con.setRequestMethod("POST");
	            con.setDoOutput(true);
	            con.setDoInput(true);
	            con.setUseCaches(false);
	            con.setRequestProperty("Content-Type", "application/octet-stream");
	            OutputStream outStream = con.getOutputStream();
	            outStream.write(PostData);
	            outStream.flush();
	            outStream.close();
	            //return 
	            inputStream = con.getInputStream();
	            System.out.println(con.getResponseCode());
	        } catch (Exception e) {
	            e.printStackTrace();
	        }
			return inputStream; 
	    }

	// round https certificate validate
	HostnameVerifier hv = new HostnameVerifier() {  
        public boolean verify(String urlHostName, SSLSession session) {  
            System.out.println("Warning: URL Host: " + urlHostName + " vs. "  
                               + session.getPeerHost());  
            return true;  
        }  
    };  
   
	 private static void trustAllHttpsCertificates() throws Exception {  
		 javax.net.ssl.TrustManager[] trustAllCerts = new javax.net.ssl.TrustManager[1];  
		 javax.net.ssl.TrustManager tm = new miTM();  
		 trustAllCerts[0] = tm;  
		 javax.net.ssl.SSLContext sc = javax.net.ssl.SSLContext  
		 .getInstance("SSL");  
		 sc.init(null, trustAllCerts, null);  
		 javax.net.ssl.HttpsURLConnection.setDefaultSSLSocketFactory(sc  
		 .getSocketFactory());  
	 }  
	  
	 static class miTM implements javax.net.ssl.TrustManager,  
	 javax.net.ssl.X509TrustManager {  
	 public java.security.cert.X509Certificate[] getAcceptedIssuers() {  
		 return null;  
	 }  
  
	 public boolean isServerTrusted(  
	 java.security.cert.X509Certificate[] certs) {  
		 return true;  
	 }  
  
	 public boolean isClientTrusted(  
	 java.security.cert.X509Certificate[] certs) {  
		 return true;  
	 }  
  
	 public void checkServerTrusted(  
	 java.security.cert.X509Certificate[] certs, String authType)  
	 throws java.security.cert.CertificateException {  
		 return;  
	 }  
  
	 public void checkClientTrusted(  
			 java.security.cert.X509Certificate[] certs, String authType)  
					 throws java.security.cert.CertificateException {  
		 return;  
	 }  
 }  
}

```

PS:因为是HTTPS请求，需要认证，这里了解到发送方可以忽略，该部分功能参考网上已有解决方案。