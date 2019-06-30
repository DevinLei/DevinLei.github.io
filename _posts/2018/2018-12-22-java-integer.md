---
layout: post
title: 探究java Integer的实现
copyright: java
category: java
tags: [java]
keywords: java,Integer
---

### 我们先来看一个例子
	public static void main(String[] args) {
        Integer a = 1,b=2;
        System.out.println("交换前a="+a+ "，b="+b);
        // 交换a,b值
        exchange(a,b);
        System.out.println("交换后a="+a+ "，b="+b);
    }
    public static void exchange(Integer o1,Integer o2){
      Integer temp = o1;
      o1=o2;
      o2=temp;
    }
运行结果：
 			
	        交换前a=1，b=2
			交换后a=1，b=2

### 按照预期结果a和b值应该被交换，但实际结果却是a和b的值根本就没有变化，这是怎么回事呢？

我们知道java中有两种传递方式

引用传递：对象类型值存储在堆中，而所对应的引用即堆中的地址存放在栈中。即传递的是这个对象地址。

值传递：基本数据类型用到都是值传递，会直接copy一个原对象的副本。

我们还知道对象类型和引用类型存放的位置是不同的 对象类型存储在堆中，引用类型存储在栈中
对象类型使用的是引用传递。

而Integer是特殊的引用类型即封装类型，java中规定传递他们的副本。

我们通过下面两幅图来了解下这个过程


![交换前](https://img-blog.csdnimg.cn/20190630162529375.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNzM2OTQ3,size_16,color_FFFFFF,t_70)


![交换后](https://img-blog.csdnimg.cn/20190630162550720.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNzM2OTQ3,size_16,color_FFFFFF,t_70)


所以实际进行交换的仅仅是a,b的副本，并非a,b本身。

### 不过你仍然可能有疑问，为什么Integer要规定传递副本呢？
我们可以通过阅读Integer的源码找到答案：
	
![Integer部分源码](https://img-blog.csdnimg.cn/20190630162612548.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNzM2OTQ3,size_16,color_FFFFFF,t_70)

可以看到Integer的value被final修饰，意味着值不可变。

### 那么你可能会问，该怎么实现正确的交换呢？

我们知道value作为私有的成员变量是不可访问的，但是我们可以通过反射来跳过权限检查，从而达到操作的目的。

我们对exchange()方法做一些修改如下：

	public static void exchange(Integer o1,Integer o2)  {
        try {
            // 通过反射获取Integer的value属性
            Field field =Integer.class.getDeclaredField("value");
            // 设置该值为true,以绕开权限检查
            field.setAccessible(true);
            int temp = o1.intValue();
            field.set(o1,o2.intValue());
            field.set(o2,temp); 
        }catch (Exception e){
            e.printStackTrace();
        }
    }

再次运行，运行结果如下：

	交换前a=1，b=2
	交换后a=2，b=2

###发现了吗？只有a变了，b却没变，这是为啥啊？

其实这个问题就涉及到了java中的一个语法糖-自动装箱与拆箱

* 自动装箱：自动将基本数据类型转换为包装器类型。如下：

     
        Integer a = 1,b=2;
  相当于：

        Integer a = Integer.valueOf(1),b=Integer.valueOf(2);
  
  先看看valueOf()的源码：

		public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    	}
        
  IntegerCache 的代码：

        private static class IntegerCache {
        static final int low = -128;
        static final int high;
        static final Integer cache[];
        
 可以看到在转换的时候,如果目标值在low和high之间 ，也就是在-128到127之间，会直接从Cache缓存中取值。 如果目标值超出这个范围则会new 一个Integer对象。

## 这里需要格外注意下：

        Integer a =1,b=1;
   		return a==b; 
        该比较结果为true,因为a,b的值在-128~127，从缓存取值相同，为同一地址。

        ----------------------------------------------------------------- 
		Integer a =129,b=129;
   		return a==b; 
        该比较结果为false,因为a,b的值超出-128~127，会分别new一个Integer,分配的是不同的内存地址。


* 自动拆箱： 自动将包装器类型转换为基本数据类型。如下：


         Integer total = 99;
    	 //自定拆箱
    	 int totalprim = total;
         

了解了自动装箱的原理，我们再来分析一下这个问题。我们现在知道了 field.set(o2,temp);  中的temp使用了自动装箱的操作 ，相当于 filed.set(02,Integer.valueOf(temp).intValue) 在执行valueOf()方法时，会根据数组下标从缓存取值 而temp的值等于i1，i1的下标地址取值为2，所以temp也为2 。于是最后 b =2。

## 解决办法

1、将temp定义为Integer避免自动装箱缓存问题

   int temp = o1.intValue(); --> Integer temp = o1.intValue();
 
2、set时避免自动装箱

   field.set(o1,o2.intValue()); --> field.setInt(o1,o2.intValue());

   field.set(o2,temp);          --> field.setInt(o2,temp); 

修改后运行结果：

    交换前a=1，b=2
	交换后a=2，b=1

