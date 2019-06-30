---
layout: post
title: java四种引用简介
copyright: java
category: java
tags: [java]
keywords: java,引用
---
## 引言
java内存管理分为内存分配和内存回收，都不需要程序员负责，垃圾回收的机制主要是看对象是否有引用指向该对象。

java对象的引用包括
  强引用，软引用，弱引用，虚引用

Java中提供这四种引用类型主要有两个目的：

第一是可以让程序员通过代码的方式决定某些对象的生命周期；

第二是有利于JVM进行垃圾回收。
## 概念
### 强引用
* 指创建一个对象并把这个对象赋值给一个引用变量

     比如：

     Object strongRef =new Object();

     String strongRef ="strongReference";

* 强引用有引用对象指向时，永远不会被GC(垃圾回收器)回收，JVM宁可抛出OutOfMemory（内存溢出错误）也不回收。

* 举个栗子：

    	 static Object strongRef = new Object();
    	 public static void main(String[] args) {
        	Object obj = strongRef; // 方法中的强引用对象在方法执行完后被GC回收
        	strongRef = null; // 将引用主动设置为null可中断与对象的关联
        	System.gc();
        	System.out.println("gc之后：" + obj);
    	 }
### 软引用
* 如果一个对象具有软引用，那么在内存充足的情况下，该对象不会被垃圾回收器回收。
* 只有当内存不足时，会被回收。
* 软引用可用来实现内存敏感的高速缓存,比如网页缓存、图片缓存等。使用软引用能防止内存泄露，增强程序的健壮性。   
* SoftReference的特点是它的一个实例保存对一个Java对象的软引用， 该软引用的存在不妨碍垃圾收集线程对该Java对象的回收。也就是说，一旦SoftReference保存了对一个Java对象的软引用后，在垃圾线程对 这个Java对象回收前，SoftReference类所提供的get()方法返回Java对象的强引用。

* 另外，一旦垃圾线程回收该Java对象之 后，get()方法将返回null。

* 举个栗子：
* 
		Object softObj = new Object();
        // 使用SoftReference 创建一个软引用
        SoftReference softReference = new SoftReference(softObj);
        // get() 方法得到该实例
        softReference.get();

### 弱引用
* 弱引用也是用来描述非必需对象的，当JVM进行垃圾回收时，无论内存是否充足，都会回收被弱引用关联的对象。
* 在java中，用java.lang.ref.WeakReference类来表示。
* 另外，一旦垃圾线程回收该Java对象之 后，get()方法将返回null。
* 举个栗子：

        // 弱引用
        Object Obj = new Object();
        // 使用SoftReference 创建一个弱引用
        WeakReference weakReference = new WeakReference(weakObj);
        // get() 方法得到该实例
        weakReference.get();

### 虚引用
* 虚引用和前面的软引用、弱引用不同，它并不影响对象的生命周期。
* 在java中用java.lang.ref.PhantomReference类表示。
* 如果一个对象与虚引用关联，则跟没有引用与之关联一样，在任何时候都可能被垃圾回收器回收。
* 要注意的是，虚引用必须和引用队列关联使用，当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会把这个虚引用加入到与之 关联的引用队列中。程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。如果程序发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。
* 主要用于在GC回收之前做一些处理。
* 举个栗子：
   
        // 虚引用
        ReferenceQueue<String> queue = new ReferenceQueue<String>();
        // 必须与引用队列联合使用
        PhantomReference<String> pr = new PhantomReference<String>(new String("phantom"), queue);
        //  无论是否被GC回收，get()得到的实例为null
        System.out.println(pr.get());