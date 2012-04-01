---
layout: post
title: FastArrayList的性能测试
---

java.util.ArrayList?(非同步)是JAVA开发中常用的集合类，apache commons collections中提供了一个直接继承ArrayList的实现----FastArrayList，写了比较简单的代码测试了一下，性能较ArrayList具有很大的提升。

FastArrayList使用两种模式：
1. slow-------该模式提供了多线程下同步功能
2. fast--------该模式没有提供同步功能

测试代码：
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
结果：
> UTIL ArrayList Milliseconds Taken = 204
> SLOW FastArrayList Milliseconds Taken = 203
> FAST FastArrayList Milliseconds Taken = 15
> BUILD SUCCESSFUL (total time: 2 seconds)</blockquote>

在fast模式下，性能的提升非常的明显。

看了一下FastArrayList的源代码，发现还是比较简单的，最主要的add方法：
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
	
注意里面clone方法的使用。对该方法进行了相应的测试，FastArrayList在add上面的性能，比ArrayList要好上将近一倍。add方法不管是slow还是fast模式，均提供同步功能。
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
get方法，可以看到，如果是fast模式，则不提供同步的功能：
在多线程情况下，如果只是读取数据，我们完全可以使用fast模式，以取得更好的性能。

