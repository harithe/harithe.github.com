---
layout: post
title: java对象序列化
---
> 您觉得自己懂 Java 编程？事实上，大多数程序员对于 Java 平台都是浅尝则止，
> 只学习了足以完成手头上任务的知识而已。
> 在本 系列 中，Ted Neward 深入挖掘 Java 平台的核心功能，
> 揭示一些鲜为人知的事实，帮助您解决最棘手的编程挑战。

这是developerWorks新一系列的导语，对于相当部分的java开发人员来说，都仅仅是会使用java，
但却都没有去深入了解java的核心功能。CSDN上面的一篇博客 [十五年,你积累了什么?](http://blog.csdn.net/axman/archive/2010/04/24/5523746.aspx)更是说出了这样的事实----浮于表面。

developerWorks上关于java核心功能的文章不少，但相对来说，都比较的杂乱，而这一期，则将以一个系列的方式，
来介绍java的核心功能。

第一篇，是介绍java对象序列化的。java对象序列化，对于每一个java程序员来说，
都不会陌生，Implements java.io.Serializable，给对象private static final serialVersionUID 字段，
然后呢？关于 Java [对象序列化您不知道的 5 件事](http://www.ibm.com/developerworks/cn/java/j-5things1/index.html?ca=drs-cn-0504)将告诉你关于JAVA对象序列化的更多知识。