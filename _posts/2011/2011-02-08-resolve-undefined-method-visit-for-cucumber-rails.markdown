---
layout: post
title: 解决NoMethodError:undefined method 'visit' for Cucumber::Rails
---

今天在rubymine下尝试了一下rubymine对cucumber的支持，可以是，rubymine是目前来说，
对ruby and rails支持最好的IDE，没有之一。但在练习的过程中，碰到了一个奇怪的问题，run feature的时候，
以致出现下面的错误：

> NoMethodError: undefined method 'visit' for #<Cucumber::Rails::World:0x381cff0>
> ./features/step_definitions/webrat_steps.rb:16:in '/^(?:|I )am on (.+)$/'

看了一下webrat_steps.rb的源码，发现visit这个方法确实是存在的，问题，实际上是出在webrat的config中，解决其实也相当的简单：

修改features/support/evn.rb文件中的配置项：
{% highlight ruby %}
Webrat.configure do |config|
  config.mode = :rails
  config.open_error_files = false # Set to true if you want error pages to pop up in the browser
end
{% endhighlight %}
将上面的 `config.mode = :rails` 改为 `config.mode = :rack`，就OK了。