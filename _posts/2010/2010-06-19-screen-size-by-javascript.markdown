---
layout: post
title: 通过javascript获取浏览器屏幕大小
---

一般通过javascrip来获得screen的大小，都要使用几个if/else来分别判断不同的浏览器，下面提供一个比较简单的方法，该方法通过在页面上使用awt的Toolkit来获得除了IE之外的几个浏览器：
{% highlight javascript %}
function getScreenSize() {
    var screenW = 640, screenH = 480;
    if (parseInt(navigator.appVersion) &amp;gt; 3) {
        screenW = screen.width;
        screenH = screen.height;
    }
    else if (navigator.appName == "Netscape"
            &amp;amp;&amp;amp; parseInt(navigator.appVersion) == 3
            &amp;amp;&amp;amp; navigator.javaEnabled()
            )
    {
        var jToolkit = java.awt.Toolkit.getDefaultToolkit();
        var jScreenSize = jToolkit.getScreenSize();
        screenW = jScreenSize.width;
        screenH = jScreenSize.height;
    }
    
	return {screenWidth: screenW, screenHeight: screenH};

}
{% endhighlight %}
可以看到，直接使用了Toolkit的getScreenSize()方法，就可以分别得到屏幕的width和heignt。