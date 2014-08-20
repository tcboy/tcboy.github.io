---
layout: post
title:  "JAVA多线程的一些东西"
date:   2014-08-20 12:00:00
categories: Code
---

1. 多线程类有两种创建方法, 一种是implements实现Runnable类, 另一种是extends继承Thread类..
最好使用前者.

2. 如何让主线程等待所有子线程都结束了再执行下一步
{% highlight java %}

public class WorkThread implements Runnable {

	private int index;

	public WorkThread(int index) {
		this.index = index;
	}

	@Override
	public void run() {
		Random random = new Random();
		try {
			Thread.sleep(random.nextInt(1000));
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println(index);
	}

	public static void main(String[] args) {
		List<Thread> threads = new ArrayList<Thread>();
		for (int i = 0; i < 10; i++) {
			WorkThread workThread = new WorkThread(i);
			Thread thread = new Thread(workThread);
			thread.start();
			threads.add(thread);
		}

		for (Thread thread : threads) {
			try {
				thread.join();
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
		System.out.println("DONE");
	}
}

{% endhighlight %}


