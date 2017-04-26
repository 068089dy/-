---
layout: post
title: "抽象类与接口学习笔记(一)"
category: "java"
keywords: ["java", "抽象类"]
description: "java抽象类与接口学习笔记"
tags: ["java", "抽象类"]
---

### 在网上找了几篇相关博客看了看，就抄过来当作笔记给自己看看吧！


### 我觉得抽象类与接口非常相似，先看下它们的用法
## 1.抽象类：
{% highlight java %}
public abstract class Animal(){
  public void shout();
}
{%endhighlight%}
抽象类就是用来被继承的，所以抽象方法必须为public，default或者protected，默认为public。然而抽象类中不一定要有抽象方法，抽象方法没有具体的实现方法，所以以不能用抽象类创建对象。抽象类的非抽象子类必须实现父类的抽象方法。


### 继承抽象类：

{% highlight java %}
//实现类
public class Dog extends Animal(){
  public void shout(){
    System.out.println("汪");
  }
}
public class Cat extends Animals(){
  public void shout(){
    System.out.println("miao");
  }
}
//执行类
public class Opration(){
  //定一个以Animal为参数的方法
  public void ExecuteTest(Animal animal){
    //只调用“叫”的方法
    animal.shout();
  }
}
//测试类
public class Test(){
  //实现main()
  public static void main(String[] args) {
    //实例化
    Dog dog=new Dog();
    Cat cat=new Cat();
    Opration op=new Opration();

    //接下来要用多态了

    //这里是狗“叫”的放法
    op.ExecuteTest(dog);
    //这里是猫“叫”的方法
    op.ExecuteTest(cat);
  }
}
{%endhighlight%}

## 2.接口


{% highlight java %}
interface product{
	String address = "address";
	String MAKER = "maker";
	//接口中的方法只能是抽象的
	public abstract void test();
}
{%endhighlight%}

接口中的变量只能是public static final变量，方法只能是public abstract方法

### 实现接口

{% highlight java %}
public class useinterface
{
	public static void main(String[] args)
	{
		computer p=new computer();
		System.out.println(p.address+p.MAKER);
		System.out.println(p.getprice());
		//p.test();
	}
}

class computer implements product,...	//可接多个接口
{
	public int getprice()
	{
		return 2;
	}
	//方法重写（“而且必须重写product中的所有方法”）
	public void test(){
		System.out.println("111");
	}
}
{%endhighlight%}
## 3.区别及特点
从以上例子就可以看出，抽象类和接口太像了，无论是用法，还是功能，而且它们的作用都是作为模板使用，天生就是父类（虽然接口根本不是类，但还是用于为实现接口的类提供方法）然而他们还是有一些区别的，先看一下它们的使用方法以及特性的区别，以表列出：


| 参数 | 抽象类 | 接口 |
| ------ | ------ | ------ |
| 默认的方法实现|它可以有默认的方法实现|接口完全是抽象的。它根本不存在方法的实现|
|实现|抽象类可以提供成员方法的实现细节，子类使用extends关键字来继承抽象类。如果子类不是抽象类的话，它需要提供抽象类中所有声明的方法的实现。|接口中的方法必须都是抽象方法，子类使用关键字implements来实现接口。它需要提供接口中所有声明的方法的实现|
|速度|它比接口速度要快|接口是稍微有点慢的，因为它需要时间去寻找在类中实现的方法。|
|多继承|抽象方法可以继承一个类和实现多个接口|接口只可以继承一个或多个其它接口|
|访问修饰符|抽象方法可以有public、protected和default这些修饰符|接口方法默认修饰符是public。你不可以使用其它修饰符。|
|添加新方法|如果你往抽象类中添加新的方法，你可以给它提供默认的实现。因此你不需要改变你现在的代码。|如果你往接口中添加方法，那么你必须改变实现该接口的类。|
|main方法|抽象方法可以有main方法并且我们可以运行它|接口没有main方法，因此我们不能运行它。|


### 参考资料：

http://blog.sina.com.cn/s/blog_60f0ec5c0100ey99.html

http://www.cnblogs.com/dolphin0520/p/3811437.html

http://blog.xiaohansong.com/2015/12/02/abstract-class-and-interface/
