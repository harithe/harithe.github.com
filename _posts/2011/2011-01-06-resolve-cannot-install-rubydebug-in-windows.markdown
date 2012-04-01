---
layout: post
title: 解决无法安装ruby-debug
---
把系统换成了windows 7 64bit，需要重新安装ruby & rails环境。

ruby使用的是rubyinstaller上面的1.8.7的安装文件。但是，遇到一个问题，在安装ruby-debug的时候，出错了：
{% highlight ruby %}
gem install ruby-debug
Fetching: columnize-0.3.2.gem (100%)
Fetching: linecache-0.43.gem (100%)
ERROR:  Error installing ruby-debug:
        The 'linecache' native gem requires installed build tools.

Please update your PATH to include build tools or download the DevKit
from 'http://rubyinstaller.org/downloads' and follow the instructions
at 'http://github.com/oneclick/rubyinstaller/wiki/Development-Kit'
{% endhighlight %}

出错的原因是安装ruby-debug的时候，需要build tools，但系统中没有。出错信息中同时也给出了解决的法案：

1. 到 http://rubyinstaller.org/downloads/ 去下载dev kit - [DevKit-tdm-32-4.5.1-20101214-1400-sfx.exe](http://github.com/downloads/oneclick/rubyinstaller/DevKit-tdm-32-4.5.1-20101214-1400-sfx.exe)

2. 按照 [Development-Kit](http://github.com/oneclick/rubyinstaller/wiki/Development-Kit/) 安装dev kit

主要安装步骤如下：

1. 如果原来系统中已经安装了旧版的dev kit, 则删除它
2. 下载上面提到的dev kit
3. 解压下载下来的文件到指定的目录，如c:/devkit。(注意：目录不能有空格)
4. 运行ruby dk.rb,然后按照提示分别运行ruby dk.rb init 和 ruby dk.rb install来增强ruby
5. 可以运行 gem install rdiscount --platform=ruby 来测试是否成功

完成上面的步骤后，就可以成功安装ruby-debug和ruby-debug-ide了。