---
layout: post
title:  "一个用JAVA实现的线段树类"
date:   2014-07-10 13:00:00
categories: Code
---

抽象类SegmentOperation.java:
{% highlight java %}
public abstract class SegmentOperation<T> {
	public abstract T getMid(T o1, T o2);
	public abstract int compare(T o1, T o2);
}
{% endhighlight %}

线段树实现SegmentTree.java:
{% highlight java %}
public class SegmentTree<T> {

	private SegmentTree<T> treeRight;
	private SegmentTree<T> treeLeft;
	private boolean isLeaf;

	private T begin;
	private T end;
	private int value;

	SegmentOperation<T> oper;

	public SegmentTree(T beginT, T endT, int segmentValue, SegmentOperation<T> operT) {
		begin = beginT;
		end = endT;
		value = segmentValue;
		oper = operT;
		treeRight = null;
		treeLeft = null;
		isLeaf = true;
	}

	public void update(int segmentValue) {
		value = segmentValue;
		if ( treeLeft != null ) {
			treeLeft.update(segmentValue);
		}
		if ( treeRight != null ) {
			treeRight.update(segmentValue);
		}
	}

	public void insert(T beginT, T endT, int segmentValue) {
		if ( (oper.compare(begin, beginT) >= 0) && (oper.compare(end, endT) <= 0 ) ) {
			update(segmentValue);
			return;
		}
		T mid = oper.getMid(begin, end);
		if ( isLeaf ) {
			treeLeft = new SegmentTree<T>(begin, mid, value, oper);
			treeRight = new SegmentTree<T>(mid, end, value, oper);
			isLeaf = false;
		}
		if ( oper.compare(beginT, mid) < 0 ) {
			treeLeft.insert(beginT, endT, segmentValue);
		}
		if ( oper.compare(endT, mid) > 0 ) {
			treeRight.insert(beginT, endT, segmentValue);
		}
	}
}
{% endhighlight %}

使用时需要将抽象类中的两个函数@Override.

