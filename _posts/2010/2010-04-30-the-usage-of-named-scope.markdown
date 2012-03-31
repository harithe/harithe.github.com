---
layout: post
title: named_scope��ʹ��
---
�������֪��named_scope(��֪���ģ����Բο�named_scope���÷�)�������ܻ���֪�� named_scope���lambda��ǿ�������������railscasts�е�Custom Routes�� ��by_date���������ǾͿ��Խ���named_scope�����lambda��ʵ�֣�
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
ʹ���൱�ļ�ࣺ
{% highlight ruby %}
@articles = Article.by_date(params[:year], params[:month], params[:day])
{% endhighlight %}