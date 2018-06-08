---
layout:     post
title:      "Java ：Pass by value or pass by reference"
subtitle:   " \"Hello World, Hello Blog\""
date:       2018-06-8 23:00:00
author:     "ray"
header-img: "img/post-bg-2015.jpg"
tags:
    - java
    - reference
---

    Java 中到底是否只存在值传递，因为在查阅资料时，经常看到有人说 Java 只有值传递，但有人说既有值传递，也有引用传递，对于两个观点个人觉得应该是站的角度不同而得出两个不同的说法，其实两个说法其中的原理是一样的，只要咱们懂得其中的原理，那么至于叫什么也就无所谓了，下面是我在网上看到的一个帖子，解释的感觉挺全面，就转过来，以供以后学习参考：

##**1：按值传递是什么**

指的是在方法调用时，传递的参数是按值的拷贝传递。示例如下：
```
1.  public class TempTest {  
2.  private void test1(int a){  
3.  // 做点事情  
4.  }  
5.  public static void main(String[] args) {  
6.  TempTest t = new TempTest();  
7.  int a = 3;  
8.  t.test1(a);// 这里传递的参数 a 就是按值传递  
9.  }  
10.  }  
```
按值传递重要特点：传递的是值的拷贝，也就是说传递后就互不相关了。

示例如下：
```
1.  public class TempTest {  
2.  private void test1(int a){  
3.  a = 5;  
4.  System.out.println("test1 方法中的 a="+a);  
5.  }  
6.  public static void main(String[] args) {  
7.  TempTest t = new TempTest();  
8.  int a = 3;  
9.  t.test1(a);// 传递后，test1 方法对变量值的改变不影响这里的 a  
10.  System.out.println(”main 方法中的 a=”+a);  
11.  }  
12.  }  
```
运行结果是：
```
1.  test1 方法中的 a=5  
2.  main 方法中的 a=3  
```
##**2：按引用传递是什么**

指的是在方法调用时，传递的参数是按引用进行传递，其实传递的引用的地址，也就是变量所对应的内存空间的地址。

示例如下：
```
1.  public class TempTest {  
2.  private void test1(A a){  
3.  }  
4.  public static void main(String[] args) {  
5.  TempTest t = new TempTest();  
6.  A a = new A();  
7.  t.test1(a); // 这里传递的参数 a 就是按引用传递  
8.  }  
9.  }  
10.  class A{  
11.  public int age = 0;  
12.  }  
```
##**3：按引用传递的重要特点**

传递的是值的引用，也就是说传递前和传递后都指向同一个引用（也就是同一个内存空间）。

示例如下：
```
1.  public class TempTest {  
2.  private void test1(A a){  
3.  a.age = 20;  
4.  System.out.println("test1 方法中的 age="+a.age);  
5.  }  
6.  public static void main(String[] args) {  
7.  TempTest t = new TempTest();  
8.  A a = new A();  
9.  a.age = 10;  
10.  t.test1(a);  
11.  System.out.println(”main 方法中的 age=”+a.age);  
12.  }  
13.  }  
14.  class A{  
15.  public int age = 0;  
16.  }  
```
运行结果如下：
```
1.  test1 方法中的 age=20  
2.  main 方法中的 age=20  
```
##**4：理解按引用传递的过程——内存分配示意图**

要想正确理解按引用传递的过程，就必须学会理解内存分配的过程，内存分配示意图可以辅助我们去理解这个过程。

用上面的例子来进行分析：

（1）：运行开始，运行第 8 行，创建了一个 A 的实例，内存分配示意如下：

![](http://img.my.csdn.net/uploads/201211/14/1352904793_7656.jpg)

（2）：运行第 9 行，是修改 A 实例里面的 age 的值，运行后内存分配示意如下：

![](http://img.my.csdn.net/uploads/201211/14/1352904854_9920.jpg)

（3）：运行第 10 行，是把 main 方法中的变量 a 所引用的内存空间地址，按引用传递给 test1 方法中的 a 变量。请注意：这两个 a 变量是完全不同的，不要被名称相同所蒙蔽。

内存分配示意如下：

![](http://img.my.csdn.net/uploads/201211/14/1352904930_4435.jpg)

由于是按引用传递，也就是传递的是内存空间的地址，所以传递完成后形成的新的内存示意图如下：

![](http://img.my.csdn.net/uploads/201211/14/1352905063_4451.jpg)

也就是说：是两个变量都指向同一个空间。

（4）：运行第 3 行，为 test1 方法中的变量 a 指向的 A 实例的 age 进行赋值，完成后形成的新的内存示意图如下：

![](http://img.my.csdn.net/uploads/201211/14/1352905129_9181.jpg)

此时 A 实例的 age 值的变化是由 test1 方法引起的

（5）：运行第 4 行，根据此时的内存示意图，输出 test1 方法中的 age=20

（6）：运行第 11 行，根据此时的内存示意图，输出 main 方法中的 age=20

##**5：对上述例子的改变**

理解了上面的例子，可能有人会问，那么能不能让按照引用传递的值，相互不影响呢？就是 test1 方法里面的修改不影响到 main 方法里面呢？

方法是在 test1 方法里面新 new 一个实例就可以了。改变成下面的例子，其中第 3 行为新加的：
```
1.  public class TempTest {  
2.  private void test1(A a){  
3.  a = new A();// 新加的一行  
4.  a.age = 20;  
5.  System.out.println("test1 方法中的 age="+a.age);  
6.  }  
7.  public static void main(String[] args) {  
8.  TempTest t = new TempTest();  
9.  A a = new A();  
10.  a.age = 10;  
11.  t.test1(a);  
12.  System.out.println(”main 方法中的 age=”+a.age);  
13.  }  
14.  }  
15.  class A{  
16.  public int age = 0;  
17.  }  
```
运行结果为：
```
1.  test1 方法中的 age=20  
2.  main 方法中的 age=10  
```
为什么这次的运行结果和前面的例子不一样呢，还是使用内存示意图来理解一下

##**6：再次理解按引用传递**

（1）：运行开始，运行第 9 行，创建了一个 A 的实例，内存分配示意如下：

![](http://img.my.csdn.net/uploads/201211/14/1352905341_1854.jpg)

（2）：运行第 10 行，是修改 A 实例里面的 age 的值，运行后内存分配示意如下：

![](http://img.my.csdn.net/uploads/201211/14/1352905439_6900.jpg)

（3）：运行第 11 行，是把 main 方法中的变量 a 所引用的内存空间地址，按引用传递给 test1 方法中的 a 变量。请注意：这两个 a 变量是完全不同的，不要被名称相同所蒙蔽。

内存分配示意如下：

![](http://img.my.csdn.net/uploads/201211/14/1352905470_6545.jpg)

由于是按引用传递，也就是传递的是内存空间的地址，所以传递完成后形成的新的内存示意图如下：

![](http://img.my.csdn.net/uploads/201211/14/1352905527_3430.jpg)

也就是说：是两个变量都指向同一个空间。

（4）：运行第 3 行，为 test1 方法中的变量 a 重新生成了新的 A 实例的，完成后形成的新的内存示意图如下：

![](http://img.my.csdn.net/uploads/201211/14/1352905561_7018.jpg)

（5）：运行第 4 行，为 test1 方法中的变量 a 指向的新的 A 实例的 age 进行赋值，完成后形成的新的内存示意图如下：

![](http://img.my.csdn.net/uploads/201211/14/1352905593_3787.jpg)

注意：这个时候 test1 方法中的变量 a 的 age 被改变，而 main 方法中的是没有改变的。

（6）：运行第 5 行，根据此时的内存示意图，输出 test1 方法中的 age=20

（7）：运行第 12 行，根据此时的内存示意图，输出 main 方法中的 age=10

##**7：说明**

（1）：“在 Java 里面参数传递都是按值传递” 这句话的意思是：按值传递是传递的值的拷贝，按引用传递其实传递的是引用的地址值，所以统称按值传递。

（2）：在 Java 里面只有基本类型和按照下面这种定义方式的 String 是按值传递，其它的都是按引用传递。就是直接使用双引号定义字符串方式：String str = “Java 私塾”;

