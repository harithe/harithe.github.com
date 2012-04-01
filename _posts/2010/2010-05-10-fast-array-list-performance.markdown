---
layout: post
title: FastArrayList�����ܲ���
---

java.util.ArrayList?(��ͬ��)��JAVA�����г��õļ����࣬apache commons collections���ṩ��һ��ֱ�Ӽ̳�ArrayList��ʵ��----FastArrayList��д�˱Ƚϼ򵥵Ĵ��������һ�£����ܽ�ArrayList���кܴ��������

FastArrayListʹ������ģʽ��
1. slow-------��ģʽ�ṩ�˶��߳���ͬ������
2. fast--------��ģʽû���ṩͬ������

���Դ��룺
{% highlight ruby %}
import java.util.ArrayList;
import org.apache.commons.collections.FastArrayList;

/**
 *
 */
public class FastArrayListTrial {

    public static void main(String[] args) {
        //create a new FastArrayList and ArrrayList
        FastArrayList fArList = new FastArrayList();
        ArrayList arList = new ArrayList();

        //Add a million strings to the list
        //Use new String to stop JVM from reusing String instances
        for (int i = 0; i &lt; 4000000; i++) {
            fArList.add(new String("AString"));
            arList.add(new String("AString"));
        }

        //Get time in milliseconds
        long utilBegin = System.currentTimeMillis();

        for (int util = 0; util &lt; 4000000; util++) {
            fArList.get(util);
        }
        System.out.println("UTIL ArrayList Milliseconds Taken = "
                + (System.currentTimeMillis() - utilBegin));

        long slowBegin = System.currentTimeMillis();

        for (int slow = 0; slow &lt; 4000000; slow++) {
            fArList.get(slow);
        }

        System.out.println("SLOW FastArrayList Milliseconds Taken = "
                + (System.currentTimeMillis() - slowBegin));

        fArList.setFast(true);

        long fastBegin = System.currentTimeMillis();

        for (int fast = 0; fast &lt; 400000; fast++) {
            fArList.get(fast);
        }

        System.out.println("FAST FastArrayList Milliseconds Taken = "
                + (System.currentTimeMillis() - fastBegin));
    }
}
{% endhighlight %}
�����
> UTIL ArrayList Milliseconds Taken = 204
> SLOW FastArrayList Milliseconds Taken = 203
> FAST FastArrayList Milliseconds Taken = 15
> BUILD SUCCESSFUL (total time: 2 seconds)</blockquote>

��fastģʽ�£����ܵ������ǳ������ԡ�

����һ��FastArrayList��Դ���룬���ֻ��ǱȽϼ򵥵ģ�����Ҫ��add������
{% highlight ruby %}
/**
 * Appends the specified element to the end of this list.
 *
 * @param element The element to be appended
 */
public boolean add(Object element) {

	if (fast) {
		synchronized (this) {
			ArrayList temp = (ArrayList) list.clone();
			boolean result = temp.add(element);
			list = temp;
			return (result);
		}
	} else {
		synchronized (list) {
			return (list.add(element));
		}
	}
}
{% endhighlight %}	
	
ע������clone������ʹ�á��Ը÷�����������Ӧ�Ĳ��ԣ�FastArrayList��add��������ܣ���ArrayListҪ���Ͻ���һ����add����������slow����fastģʽ�����ṩͬ�����ܡ�
{% highlight ruby %}
public Object get(int index) {
	if (fast) {
		return (list.get(index));
	} else {
		synchronized (list) {
			return (list.get(index));
		}
	}
}
{% endhighlight %}
get���������Կ����������fastģʽ�����ṩͬ���Ĺ��ܣ�
�ڶ��߳�����£����ֻ�Ƕ�ȡ���ݣ�������ȫ����ʹ��fastģʽ����ȡ�ø��õ����ܡ�

