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
	public abstract T getNext(T o1);
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

	private void update(int segmentValue) {
		value = segmentValue;
		if ( treeLeft != null ) {
			treeLeft.update(segmentValue);
		}
		if ( treeRight != null ) {
			treeRight.update(segmentValue);
		}
	}

	public void insert(T beginT, T endT, int segmentValue) {
		if ( (oper.compare(begin, beginT) == 0) && (oper.compare(end, endT) == 0 ) ) {
			update(segmentValue);
			return;
		}

		if ( oper.compare(beginT, endT) == 0 ) {
			return;
		}
		T mid = oper.getMid(begin, end);
		T midNext = oper.getNext(mid);
		if ( isLeaf ) {
			treeLeft = new SegmentTree<T>(begin, mid, value, oper);
			treeRight = new SegmentTree<T>(midNext, end, value, oper);
			isLeaf = false;
		}
		if ( oper.compare(mid, endT) >= 0 ) {
			treeLeft.insert(beginT, endT, segmentValue);
		}
		else if ( oper.compare(midNext, beginT) <= 0 ) {
			treeRight.insert(beginT, endT, segmentValue);
		}
		else {
			treeLeft.insert(beginT, mid, segmentValue);
			treeRight.insert(midNext, endT, segmentValue);
		}
	}
}
{% endhighlight %}

代码中没有对Query Delete等方法进行实现，需要的话可以根据实际情况对其进行编写。

