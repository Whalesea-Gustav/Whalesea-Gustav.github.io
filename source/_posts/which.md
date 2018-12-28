---
title: which 
date: 2018-10-24 17:56:00
categories: 工具
tags: [Linux]
---

JNI error, JVM和JDK版本不匹配
然后我找了半天的原因，发现是环境变量的位置搞错了
当然首先是去找版本的问题，JDK，JRE什么的了解了一下，但是还是找不到具体的问题根源
发现可以使用`which`来追溯环境变量的位置

结果是`javac`编译指令的源头是在jdk的bin里面的
但是`java`JVM运行指令是在另外一个文件夹里了，再去看了一下环境变量表，天知道我什么时候把java加到Path里了

不过最后证明`which`在解决问题的时候真的很管用