---
layout: post
title: 设计模式(二)-工厂方法模式
copyright: designpattern 
category: designpattern
tags: [designpattern]
keywords: factory method,工厂方法模式
--- 

 **什么是工厂方法模式呢？**
 
   定义一个用于创建对象的接口，让子类决定实例化哪一个类。工厂方法使一个类的实例化延迟到子类。
 我们已经知道简单工厂模式，在工厂类里做了分支判断，如果要新增功能，就必须修改工厂类增加分支。这种做法违背了开放-封闭原则，因为对修改也变成开放的了。而工厂方法在保留简单工厂优点的基础上弥补了简单工厂的不足。

简单来说工厂方法模式包含四个部分：
抽象产品：产品对象共同的接口或基类
具体产品：某种具体的产品
抽象工厂：所有子类工厂统一的基类或接口
具体工厂：具体生产某种产品的子类工厂

工厂方法模式结构图

![工厂方法模式结构图](https://img-blog.csdnimg.cn/20190527221426889.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNzM2OTQ3,size_16,color_FFFFFF,t_70)

 **show   code**

创建一个电脑接口 ：Computer 
```java
package design_patterns.factorymethod;

/**
 * @Author DevinLei
 */
public interface Computer {

    void coding();
}


```
创建一个三个笔记本电脑类 ,实现Computer 接口
```java
package design_patterns.factorymethod;

/**
 * @Author DevinLei
 */
public class Dell implements Computer {

    @Override
    public void coding() {
        System.out.println("中规中矩!");
    }
}

```
```java
package design_patterns.factorymethod;

/**
 * @Author DevinLei
 */
public class Lenovo implements Computer {

    @Override
    public void coding() {
        System.out.println("键盘用着舒服!");
    }
}

```

```java
package design_patterns.factorymethod;

/**
 * @Author DevinLei
 */
public class Mac implements Computer {

    @Override
    public void coding() {
        System.out.println("装逼神器!");
    }
}
```


创建一个工厂接口 ComputerFactory 
```java
package design_patterns.factorymethod;

/**
 * @Author DevinLei
 */
public interface ComputerFactory {

    Computer creatComputer();
}

```
创建三个工厂类，实现 ComputerFactory 

```java
package design_patterns.factorymethod;

/**
 * @Author DevinLei
 */
public class DellFactory implements ComputerFactory {

    @Override
    public Computer creatComputer() {
        return new Dell();
    }
}

```

```java
package design_patterns.factorymethod;

/**
 * @Author DevinLei
 */
public class LenovoFactory implements ComputerFactory {
    @Override
    public Computer creatComputer() {
        return new Lenovo();
    }
}

```

```java
package design_patterns.factorymethod;

/**
 * @Author DevinLei
 */
public class MacFactory implements ComputerFactory {

    @Override
    public Computer creatComputer() {
        return new Mac();
    }
}

```
最后，新建一个Customer 类测试一下

```java
package design_patterns.factorymethod;

/**
 * @Author DevinLei
 */
public class Customer {

    public static void main(String[] args) {

        ComputerFactory factory = null;

        factory= new LenovoFactory();
        Computer  lenovo  =  factory.creatComputer();
        lenovo.coding();

        factory= new DellFactory();
        Computer  dell  =  factory.creatComputer();
        dell.coding();

        factory= new MacFactory();
        Computer  mac  =  factory.creatComputer();
        mac.coding();

    }
}

```
输出结果：

```
键盘用着舒服!
中规中矩!
装逼神器!
```

UML类图

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190527221706565.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNzM2OTQ3,size_16,color_FFFFFF,t_70)

 **工厂方法模式的实际应用**
 
 spring中IOC创建bean,FactoryBean
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190527222837465.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNzM2OTQ3,size_16,color_FFFFFF,t_70)


	
   


