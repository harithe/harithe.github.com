---
layout: post
title: ΪActiveRecord����next previous����
---

ActiveRecord��һ����ruby��ORM��ܣ������Martin Fowler�����ġ���ҵӦ�üܹ�ģʽ���е�ActiveRecordģʽ��rubyʵ�֡���Ruby On Rails����У�ActiveRecord�ṩ�����ݳ־û���������
ActiveRecord������˻�����ORMģʽ�Ķ����ϵӳ�䣬ͬʱ���ṩ��ǿ���ģ����֤�Ͳ�ѯ���ܡ�
����һ�²����о���ʹ�õĵ�ǰ���µ���һƪ���º���һƪ������ ��һ��Ӧ�ã�
���磺
`post = Post.find(3)`
�����������䣬�õ���һ��idΪ3��Post����Ȼ������õ����ڵ�idΪ2��idΪ4��Post���󣬰���ruby on rails��˼�룬�������Ͼͻ��뵽���Ƿ������post.next��post.previousֱ�ӵõ��أ�
��ϧ���ǣ���û�����������������ǣ�ƾ��ruby��ǿ��Ķ�̬���Ե����ԣ����Ժܷ������չActiveRecord����ActiveRecord�ṩ�� ������ķ���:next��previous�������Ϳ��ԱȽϷ�����ҵ���ǰ��¼����һ��/��һ����¼��
�����������������environment.rb�ļ��У�������µķ�����
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
���������ǾͿ���ʹ��post.next��post.previous���ܷ���ĵõ�post����һ������һ��Post�����ˡ�