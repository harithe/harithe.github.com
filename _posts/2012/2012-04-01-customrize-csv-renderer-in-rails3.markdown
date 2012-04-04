---
layout: post
title: 自定义Rails的CSV Render
---

和大部分的WEB框架一样，[Rails]使用经典的[MVC](Model, View, Controller)模式来实现Web系统的职能分工。Model层实现系统中的业务逻辑。 View层用于与用户的交互。 Controller层是Model与View之间沟通的桥梁。

在[Rails]的Controller中，使用render方法来渲染View部分。经常使用的几个选项比如`render :text`, `render :file`, `render :template`来渲染一些文本，也可以通过`render :js`, `render :json`, `render :xml`等来渲染一些格式化文本。具体关于Rails的模板渲染部分，可以阅读[Layouts and Rendering in Rails](http://guides.rubyonrails.org/layouts_and_rendering.html)部分。

在Rails3中，由于合并了Merb，重写了其基础架构，可以很方便的对Rails的render进行扩展。比如说，在项目中需要将产品信息导出成[CSV]文件，我们就可以通过给render方法增加:csv选项来完成。

#### 1. 创建rails插件结构
在[Rails Guide](http://guides.rubyonrails.org/index.html)的
[The Basics of Creating Rails Plugins](http://guides.rubyonrails.org/plugins.html)部分，详细介绍了编写一个rails plugin的步骤及实例。

首先通过`rails plugin`命令创建csv_renderer插件：

{% highlight ruby %}
rails plugin new csv_renderer
{% endhighlight %}

>  create

>  create  README.rdoc

>  create  Rakefile

>  create  csv_renderer.gemspec

>  create  MIT-LICENSE

>  create  .gitignore

>  create  Gemfile

>  create  lib/csv_renderer.rb

>  create  lib/tasks/csv_renderer_tasks.rake

>  create  lib/csv_renderer/version.rb

>  create  test/test_helper.rb

>  create  test/csv_renderer_test.rb

>  append  Rakefile

>vendor_app  test/dummy

>  run  bundle install

(会在test/dummy目录下创建一个rails 的 app，可以用来在rails环境中运行和测试插件)

在进行下一步之前，让我们先来看一下rails中，render方法的源码，通过对源码的学习，可以让我们按照rails的方式来完成我们的插件。

在`actionpack-3.2.2\lib\action_controller\metal\renderers.rb`中，我们可以看到`:xml`的实现，
{% highlight ruby %}
add :xml do |xml, options|
  self.content_type ||= Mime::XML
  xml.respond_to?(:to_xml) ? xml.to_xml(options) : xml
end
{% endhighlight %}

在上面的方法里，直接使用了to_xml方法返回对象的xml形式。

`self.content_type ||= Mime::XML`指定了HTTP响应的Mime types，在`mime_types.rb`文件中，定义了所有在Rails中可以使用的Mime types:
{% highlight ruby %}
# Build list of Mime types for HTTP responses
# http://www.iana.org/assignments/media-types/

Mime::Type.register "text/html", :html, %w( application/xhtml+xml ), %w( xhtml )
Mime::Type.register "text/plain", :text, [], %w(txt)
Mime::Type.register "text/javascript", :js, %w( application/javascript application/x-javascript )
Mime::Type.register "text/css", :css
Mime::Type.register "text/calendar", :ics
Mime::Type.register "text/csv", :csv

Mime::Type.register "image/png", :png, [], %w(png)
Mime::Type.register "image/jpeg", :jpeg, [], %w(jpg jpeg jpe)
Mime::Type.register "image/gif", :gif, [], %w(gif)
Mime::Type.register "image/bmp", :bmp, [], %w(bmp)
Mime::Type.register "image/tiff", :tiff, [], %w(tif tiff)

Mime::Type.register "video/mpeg", :mpeg, [], %w(mpg mpeg mpe)

Mime::Type.register "application/xml", :xml, %w( text/xml application/x-xml )
Mime::Type.register "application/rss+xml", :rss
Mime::Type.register "application/atom+xml", :atom
Mime::Type.register "application/x-yaml", :yaml, %w( text/yaml )

Mime::Type.register "multipart/form-data", :multipart_form
Mime::Type.register "application/x-www-form-urlencoded", :url_encoded_form

# http://www.ietf.org/rfc/rfc4627.txt
# http://www.json.org/JSONRequest.html
Mime::Type.register "application/json", :json, %w( text/x-json application/jsonrequest )

Mime::Type.register "application/pdf", :pdf, [], %w(pdf)
Mime::Type.register "application/zip", :zip, [], %w(zip)

# Create Mime::ALL but do not add it to the SET.
Mime::ALL = Mime::Type.new("*/*", :all, [])
{% endhighlight %}
所以，我们可以在controller中使用下面的方式来render一个xml:
{% highlight ruby %}
render :xml => @obj
{% endhighlight %}
该方法会调用`renderers.rb`中的`:xml`代码块。

下面，我们就可以按照rails的这种方式，来完成我们自己的插件开发。

首先，我们会使用下面的调用方式：
{% highlight ruby %}
render :csv => @obj
{% endhighlight %}

下面我们再lib/csv_renderer.rb中来实现:

{% highlight ruby %}
module CsvRenderer
  ActionController::Renderers.add :csv do |object, options|
    require 'csv'
    csv_string = CSV.generate do |csv|
      object.each do |obj|
        csv << obj.attributes.values.to_a # implementation
      end
    end

    csv_string
  end
end
{% endhighlight %}

下面我们使用rails的scaffold来生成一个简单的测试例子:
> test\dummy\script>rails g scaffold post title:string content:text
> test\dummy>rake db:create:all
> test\dummy>rake db:migrate

修改`PostsController`的index方法：
{% highlight ruby %}
def index
@posts = Post.all

respond_to do |format|
  format.html # index.html.erb
  format.json { render json: @posts }
  format.csv { render csv: @posts}
end
end
{% endhighlight %}
启动Raikls Server:

`test\dummy\bundle exec rails s`

访问http://localhost:3000/posts.csv就可以看得生成的csv文件了。

[Rails]: http://rubyonrails.org/
[MVC]: http://baike.baidu.com/view/739359.htm
[CSV]: http://baike.baidu.com/view/468993.htm