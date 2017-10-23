---
title:  java提高篇（六）-----关键字static
date: 2017-10-23 13:38:04
tags: [java,转载]
categories: java开发
---

> 转载： http://blog.csdn.net/chenssy/article/details/13004291

## static代表着什么
在Java中并不存在全局变量的概念，但是我们可以通过static来实现一个“伪全局”的概念，在Java中static表示“全局”或者“静态”的意思，用来修饰成员变量和成员方法，当然也可以修饰代码块。
Java把内存分为栈内存和堆内存，其中栈内存用来存放一些基本类型的变量、数组和对象的引用，堆内存主要存放一些对象。在JVM加载一个类的时候，若该类存在static修饰的成员变量和成员方法，则会为这些成员变量和成员方法在固定的位置开辟一个固定大小的内存区域，有了这些“固定”的特性，那么JVM就可以非常方便地访问他们。同时如果静态的成员变量和成员方法不出作用域的话，它们的句柄都会保持不变。同时static所蕴含“静态”的概念表示着它是不可恢复的，即在那个地方，你修改了，他是不会变回原样的，你清理了，他就不会回来了。
同时被static修饰的成员变量和成员方法是独立于该类的，它不依赖于某个特定的实例变量，也就是说它被该类的所有实例共享。所有实例的引用都指向同一个地方，任何一个实例对其的修改都会导致其他实例的变化。
<!-- more -->
```java
    public class User {  
        private static int userNumber  = 0 ;  
        
        public User(){  
            userNumber ++;  
        }  
        
        public static void main(String[] args) {  
            User user1 = new User();  
            User user2 = new User();  
            
            System.out.println("user1 userNumber：" + User.userNumber);  
            System.out.println("user2 userNumber：" + User.userNumber);  
        }  
    }      
    ------------  
    Output:  
    user1 userNumber：2  
    user2 userNumber：2  
```

##  怎么使用static
static可以用于修饰成员变量和成员方法，我们将其称之为静态变量和静态方法，直接通过类名来进行访问。
ClassName..propertyName
ClassName.methodName(……)
Static修饰的代码块表示静态代码块，当JVM装载类的时候，就会执行这块代码，其用处非常大。（对于代码块的使用这几天介绍，敬请关注）

### static变量
static修饰的变量我们称之为静态变量，没有用static修饰的变量称之为实例变量，他们两者的区别是：
静态变量是随着类加载时被完成初始化的，它在内存中仅有一个，且JVM也只会为它分配一次内存，同时类所有的实例都共享静态变量，可以直接通过类名来访问它。
但是实例变量则不同，它是伴随着实例的，每创建一个实例就会产生一个实例变量，它与该实例同生共死。
所以我们一般在这两种情况下使用静态变量：对象之间共享数据、访问方便。
### static方法
static修饰的方法我们称之为静态方法，我们通过类名对其进行直接调用。由于他在类加载的时候就存在了，它不依赖于任何实例，所以static方法必须实现，也就是说他不能是抽象方法abstract。
Static方法是类中的一种特殊方法，我们只有在真正需要他们的时候才会将方法声明为static。如Math类的所有方法都是静态static的。
### static代码块
被static修饰的代码块，我们称之为静态代码块，静态代码块会随着类的加载一块执行，而且他可以随意放，可以存在于该了的任何地方。

## Static的局限
Static确实是存在诸多的作用，但是它也存在一些缺陷。

        1、它只能调用static变量。
        2、它只能调用static方法。
        3、不能以任何形式引用this、super。
        4、static变量在定义时必须要进行初始化，且初始化时间要早于非静态变量。
总结：<font color="#3366ff">无论是变量，方法，还是代码块，只要用static修饰，就是在类被加载时就已经"准备好了",也就是可以被使用或者已经被执行，都可以脱离对象而执行。反之，如果没有static，则必须要依赖于对象实例。</font>