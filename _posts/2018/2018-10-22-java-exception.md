---
layout: post
title: java受检异常与非受检异常
copyright: java
category: java
tags: [java]
keywords: java,Exception
---

![](https://img-blog.csdnimg.cn/20190630162049629.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNzM2OTQ3,size_16,color_FFFFFF,t_70)

## Error:表示错误,程序无法处理，多与硬件相关。
## Exception:表示程序逻辑错误，可以被程序处理。
## 受检异常和非受检异常的区别
### 受检异常（IOException及其子类）:编译时检查，必须处理，不处理编译无法通过。    
### 非受检异常（RuntimeException及其子类） :编译时不检查，可处理也可不处理，不处理时在运行期间JVM自动处理。
### 异常处理方式:
* try catch 捕获 这种方式需要注意以下两点
一是可能会破坏封装性，即修改实现类会影响接口 ;二是增调用方代码复杂度 
* throws 抛出
