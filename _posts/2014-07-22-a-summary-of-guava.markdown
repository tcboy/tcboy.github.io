---
layout: post
title:  "Guava用法总结(实时更新)"
date:   2014-07-22 16:30:00
categories: Code
---

Splitter (字符串分隔)

{% highlight java %}

Splitter splitter = Splitter.on(CharMatcher.anyOf(":="));
//使用':'或者'='分隔字符串构造List

Splitter.MapSplitter mapSplitter = Splitter.on(",").omitEmptyStrings().trimResults().withKeyValueSeparator(":");	
//使用','分隔字符串, 并使用':'分隔key&value构造Map

{% endhighlight %}


Files

{% highlight java %}

List<String> datas = Files.readAllLines(Paths.get("file.txt"), Charsets.UTF_8); 
//直接读取一个文件,并按行存为List

{% endhighlight %}

