---
layout: post
title: 设计模式(五)-装饰模式
copyright: designpattern 
category: designpattern
tags: [designpattern]
keywords: Decorator,装饰模式
---

**什么是装饰模式？**
 
  装饰模式（Decorator）,动态的给一个对象增加一些额外的职责，就增加功能来说，装饰模式比生成子类更加灵活。
 
 一般而言，当系统需要实现新功能时候，我们会向旧的类中添加新的代码以扩展其功能。我们将这种行为称为对主类的装饰，但这种做法的问题在于，在主类中增加了属性和方法，从而增加了主类的复杂度。而且增加的这部分属性只是为了满足一些特定的场景下的特殊需求。而装饰模式就很好的解决了这些问题，通过将装饰的功能放在单独的类中，并让这个类包装它要装饰的对象，因此，当客户需要新的功能时，就能在客户代码中有序的选择调用装饰对象。
 
 由此可见，装饰模式的优点就在于将类的核心职责和装饰功能区分，简化了原有的类。
 
装饰模式类结构图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190530231453607.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNzM2OTQ3,size_16,color_FFFFFF,t_70)

**代码示例：**

Person 抽象类
```java
/**
 *
 * @Author DevinLei
 */
public abstract class Person {

    /**
     * 性别
     **/
    private String sex;
    /**
     * 肤色
     **/
    private String skinColour;

    /**
     * 形象展示
     **/
    public abstract void show();

}
```
装饰抽象类 Decorator , 继承Person

```java
/**
 * @Author DevinLei
 */
public class Decorator extends Person{

    protected  Person person;

    public void setPerson(Person person){
        this.person = person;
    }

    @Override
    public void show() {
      if(person!=null){
          person.show();
      }
    }
}
```
裸奔者的具体实现类Streaking ，继承 Person

```java
/**
 * @Author DevinLei
 */
public class Streaking extends Person {

    @Override
    public void show() {
        System.out.println("一个光者腚的裸奔者");
    }
}

```
具体装饰类 Pants ,继承 Decorator

```java
/**
 * @Author DevinLei
 */
public class Pants extends Decorator{

private String shorts;

    public Pants() {
    }

    public Pants(String shorts) {
        this.shorts = shorts;
    }

    @Override
    public void show() {
        System.out.println("穿短裤:"+shorts);
        super.show();
    }
}

```
具体装饰类 Sleeve ,继承Decorator

```java
/**
 * @Author DevinLei
 */
public class Sleeve extends Decorator {

    private String color;

    public Sleeve(){
    }

    public Sleeve(String color){
        this.color = color;
    }

    @Override
    public void show() {
        super.show();
        addColor();
    }

    private void addColor(){
        System.out.println("渲染为："+color+"色");
    }
}

```
客户测试类 Customer

```java
/**
 * @Author DevinLei
 */
public class Customer {

    public static void main(String[] args) {

        Streaking streaking  = new Streaking();
        Pants pants = new Pants("七分裤");
        Sleeve sleeve = new Sleeve("黑");

        pants.setPerson(streaking);
        sleeve.setPerson(pants);
        sleeve.show();

    }
}

```
输出结果：

```java
穿短裤:七分裤
一个光者腚的裸奔者
渲染为：黑色
```
UML类图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190604223538236.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNzM2OTQ3,size_16,color_FFFFFF,t_70)**实际应用案例**
springsession框架使用HTTP请求包装类SessionRepositoryRequestWrapper和Session存储过滤器 SessionRepositoryFilter 实现实现分布式session；
具体请参考：https://blog.csdn.net/json20080301/article/details/79362860
