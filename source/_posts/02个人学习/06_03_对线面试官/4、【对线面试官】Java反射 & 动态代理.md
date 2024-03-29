---
title: 4、【对线面试官】Java反射 & 动态代理.md
tags:
  - 面试
  - 记录
categories: 技术
abbrlink: 986aa405
date: 2022-02-28 19:57:47
---
# 4、【对线面试官】Java反射 & 动态代理.md

## 今天要不来聊聊Java反射？你对Java反射了解多少？

1. 嗯，Java反射在JavaSE基础中还是很重要的。
2. 简单来说，反射就是Java可以给我们在运行时获取类的信息
   在初学的时候可能看不懂、又或是学不太会反射，因为初学的时候往往给的例子都是用反射创建对象，用反射去获取对象上的方法/属性什么的，感觉没多大用
3. 但毕竟我已经不是以前的我了，跟以前的看法就不一样了。
4. 理解反射重点就在于理解什么是「运行时」，为什么我们要在「运行时」获取类的信息
5. 在当时学注解的时候，我们可以发现注解的生命周期有三个枚举值（当时我还告诉过面试官你呢~）
6. 分别是SOURCE、CLASS和RUNTIME，其实一样的，RUNTIME就是对标着运行时
7. 我们都知道：我们在编译器写的代码是j ava文件，经过javac编译会变成.class文件，class文件会被JVM装载运行（这里就是真正运行着我们所写的代码（虽然是被编译过的），也就所谓的运行时。

## 嗯，你说了那么多，就讲述了什么是运行时，还是快点进入重点吧

1. 在运行时获取类的信息，其实就是为了让我们所写的代码更具有「通用性」和「灵活性」
2. 要理解反射，需要抛开我们日常写的业务代码。以更高的维度或者说是抽象的思维去看待我们所写的“工具”
3. 所谓的“工具”：在单个系统使用叫做“Utils”、被多个系统使用打成jar包叫做“组件”、组件继续发展壮大就叫做“框架”
4. 一个好用的“工具”是需要兼容各种情况的。
5. 你肯定是不知道用该“工具“的用户传入的是什么对象，但你需要帮他们得到需要的结果。
6. 例如SpringMVC你在方法上写上对象，传入的参数就会帮你封装到对象上
7. Mybatis可以让我们只写接口，不写实现类，就可以执行SQL
8. 你在类上加上@Component注解，Sprin g就帮你创建对象
9. 这些统统都有反射的身影：约定大于配置，配置大于硬编码。
10. 通过”约定“使用姿势，使用反射在运行时获取相应的信息（毕竟作为一个”工具“是真的不知道你是怎么用的），实现代码功能的「通用性」和「灵活性」

## 结合之前说的泛型，想问下：你应该知道泛型是会擦除的，那为什么反射能获取到泛型的信息呢？

```java
// 抽象类，定义泛型<T>
public abstract class BaseDao<T> {
    public BaseDao(){
        Class clazz = this.getClass();
        ParameterizedType  pt = (ParameterizedType) clazz.getGenericSuperclass(); 
        clazz = (Class) pt.getActualTypeArguments()[0];
        System.out.println(clazz);
    }
}

// 实现类
public class UserDao extends BaseDao<User> {
    public static void main(String[] args) {
        BaseDao<User> userDao = new UserDao();

    }
}
// 执行结果输出
class com.entity.User
```

1. 嗯，这个问题我在学习的时候也想过
2. 其实是这样的，可以理解为泛型擦除是有范围的，定义在类上的泛型信息是不会被擦除的。
3. Java编译器仍在class文件以Signature 属性的方式保留了泛型信息
4. Type作为顶级接口，Type下还有几种类型，比如TypeVariable、 ParameterizedT ype、 WildCardType、 GenericArrayType、以及Class。通过这些接口我们就可以在运行时获取泛型相关的信息。

## 你了解动态代理吗？

1. 嗯，了解的。动态代理其实就是代理模式的一种，代理模式是设计模式之一。
2. 代理模型有静态代理和动态代理。静态代理需要自己写代理类，实现对应的接口，比较麻烦。
3. 在Java中，动态代理常见的又有两种实现方式：JDK动态代理和CGLIB代理
4. JDK动态代理其实就是运用了反射的机制，而CGLIB代理则用的是利用ASM框架，通过修改其字节码生成子类来处理。
5. JDK动态代理会帮我们实现接口的方法，通过invokeHandler对所需要的方法进行增强。
6. 动态代理这一技术在实际或者框架原理中是非常常见的
7. 像上面所讲的Mybatis不用写实现类，只写接口就可以执行SQL，又或是SpringAOP等等好用的技术，实际上用的就是动态代理。