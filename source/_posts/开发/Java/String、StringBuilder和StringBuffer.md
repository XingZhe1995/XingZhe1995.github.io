---
title: String、StringBuilder和StringBuffer
tags:
  - Java
categories:
  - 开发
  - 编程语言
  - Java
abbrlink: 2871eb40
date: 2021-04-05 15:13:37
---

String是Java中使用频率极高的一个类，但由于其是不可变，因此有StringBuffer和StringBuilder两个类来弥补在大量修改场景下的不足。



<!-- more -->



# String

当我们需要存储单一的字符时，使用的是基本数据类型*char*来进行存储，而要存储一连串的字符时，毫无疑问的是想到使用字符数组*char[]*来进行存储，如果代码中每次都要先声明数组才能存储字符串就显得非常冗余和烦琐了，因此贴心的Java为我们提供了String类。

String类的数据结构如下

``` java
private final char[] value;
```

可以看到String类确实就是用一个字符数组来存储字符串的，但这是Java版本1.9版本之前的内容。



## 数据类型的改变

在Java版本1.9之后，String类的数组的数据类型就由*char[]*改为了*byte[]*

``` java
private final byte[] value;
```

博主由于对这个变化了解的不是很透彻，因此摘抄了别人的解释，具体可看[这里](https://github.com/Snailclimb/JavaGuide/issues/675)或者自行搜索相关内容。

> 为什么使用byte字节而舍弃了char字符:

> 节省内存占用，byte占一个字节(8位)，char占用2个字节（16），相较char节省一半的内存空间。节省gc压力。
> 针对初始化的字符，对字符长度进行判断选择不同的编码方式。如果是 LATIN-1 编码，则右移0位，数组长度即为字符串长度。而如果是 UTF16 编码，则右移1位，数组长度的二分之一为字符串长度。



## 不可变性

String类的不可变性是由几个方面实现

1. String类本身由final修饰符进行修饰

   ``` java
   public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
   ```

2. 存储字符的属性值value由private修饰符修饰，且没有提供*setter*方法供外界进行修改

   ``` java
   private final byte[] value;
   ```

3. 虽然存储字符的属性值value使用了final修饰符进行修饰，但是仅能做到变量value对于数组的引用不变而已，而无法阻止数组元素的修改，**通过查看String的源码其实可以发现，Java的开发者很小心的处理String类中的每一行代码，从而做到在细节上实现了不可变（即没有修改数组元素）**

通过以上的几个方面，实现了String类无法被继承且内部元素无法被修改，做到了String的不可变，由于其不可变性，可以理解为一个常量，因此**String类是线程安全的**。

注：如果真的要破坏String类的不可变性，只能通过使用反射的方式进行破坏，强行进行修改了。



# StringBuilder和StringBuffer

由于String类的不可变性，每一次修改为不同的字符串就会生成一个新的String对象，这明显不利于在需要大量修改字符串内容的场景，因此Java提供了StringBuilder类和StringBuffer类来实现字符串的动态修改，只有在最后修改完毕后才才调用*toString()*方法真正的生成String对象。



## 抽象类AbstractStringBuilder

StringBuilder类和StringBuffer类都是继承自*抽象类AbstractStringBuilder*，部分代码如下

``` java
abstract class AbstractStringBuilder implements Appendable, CharSequence {
    /**
     * The value is used for character storage.
     */
    byte[] value;
```

可以看到注释，属性value就是存储字符串的地方，且没有使用final修饰符进行修饰，因此是可变的。

*AbstractStringBuilder*类本身提供且实现了大量的修改字符串内容的方法，如 `expandCapacity`、`append`、`insert`、`indexOf` 等公共方法，StringBuffer类和StringBuilder类的许多方法实现上都是直接调用的父类（AbstractStringBuilder）的方法。



## 线程安全和性能

虽说StringBuilder类和StringBuffer类都是继承自*抽象类AbstractStringBuilder*，整体上都是差不多，但是在性能和多线程操作处理上存在区别

* 线程安全

  StringBuilder类并没有对方法做任何多线程方面的处理，因此是非线程安全的。

  StringBuffer类中大量的使用**synchronized**关键字对方法进行了加锁操作，因此是线程安全的。

* 性能

  StringBuffer类由于其方法上的加锁操作，因此性能要比StringBuilder类低。

**总结：需要考虑线程安全的就用StringBuffer，无线程安全问题且重视性能的就用StringBuilder，无脑操作就选StringBuffer。**





# 参考

* [String类的不可变性](https://www.jianshu.com/p/cd72099051f9)
* [Java 9 之后，String 类的实现改用 byte 数组存储字符串](https://github.com/Snailclimb/JavaGuide/issues/675)