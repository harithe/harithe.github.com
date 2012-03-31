---
layout: post
title: named_scope的使用
---
大多数人知道named_scope(不知道的，可以参看named_scope的用法)，但可能还不知道 named_scope结合lambda的强大的威力。比如railscasts中的Custom Routes中 的by_date方法，我们就可以借助named_scope，结合lambda来实现：
{% highlight ruby %}
named_scope :by_date, lambda { |year, month, day|
    conditions = []
    conditions << ["YEAR(created_at) = ?", year] if year
    conditions << ["MONTH(created_at) = ?", month] if month
    conditions << ["DAY(created_at) = ?", day] if day
    {
      :conditions => [conditions.transpose.first.join(' AND '), *conditions.transpose.last]
    }}
{% endhighlight %}    
使用相当的简洁：
{% highlight ruby %}
@articles = Article.by_date(params[:year], params[:month], params[:day])
{% endhighlight %}