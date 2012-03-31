---
layout: post
title: 为ActiveRecord增加next previous方法
---
ActiveRecord是一个纯ruby的ORM框架，是针对Martin Fowler在他的《企业应用架构模式》中的ActiveRecord模式的ruby实现。在Ruby On Rails框架中，ActiveRecord提供了数据持久化的能力。
ActiveRecord即完成了基本的ORM模式的对象关系映射，同时还提供了强大的模型验证和查询功能。
考虑一下博客中经常使用的当前文章的上一篇文章和下一篇文章这 样一个应用，
比如：
{% highlight ruby %} post = Post.find(3)` {% endhighlight %}
我用上面的语句，得到了一个id为3的Post对象，然后，我想得到相邻的id为2和id为4的Post对象，按照ruby on rails的思想，可能马上就会想到，是否可以用post.next和post.previous直接得到呢？
可惜的是，并没有这两个方法。但是，凭借ruby的强大的动态语言的特性，可以很方便的扩展ActiveRecord，给ActiveRecord提供两 个方便的方法:next和previous，这样就可以比较方便的找到当前记录的上一个/下一个记录。
具体的做法，就是在environment.rb文件中，添加如下的方法：
{% highlight ruby %}
class ActiveRecord::Base
  def next
    self.class.find(:first, :conditions =>  ['id > ?', self.id], :order => 'id desc')
  end

  def previous
    self.class.find(:first, :conditions =>  ['id < ?', self.id], :order => 'id desc')
  end
end
{% endhighlight %}
这样，我们就可以使用post.next和post.previous来很方便的得到post的上一个和下一个Post对象了。
