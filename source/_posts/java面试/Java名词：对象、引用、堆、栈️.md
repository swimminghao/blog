---
title: java 名词
date: 2022-02-28 19:57:47
tags: [java]
categories: 技术
---
# java 名词

## “吃人”的那些Java名词：对象、引用、堆、栈️

经验都是慢慢积累的，天才不多｜ 第170篇

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/z40lCFUAHpmSC3DSh9MC8QiaZiakWP0mHCnSvS20HavA8udLNlNaImXlzYmA6TlVIxe00l1BkfXKvJoAIQsVWaEg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

记得中学的课本上，有一篇名为《狂人日记》课文；那时候根本理解不了鲁迅写这篇文章要表达的中心思想，只觉得满篇的“**吃人**”令人心情压抑；老师在讲台上慷慨激昂的讲，大多数的同学同我一样，在课本面前“痴痴”的发呆。

作为一个有着8年Java编程经验的IT老兵，说起来很惭愧，我被Java当中的四五个名词一直困扰着：**对象、引用、堆、栈、堆栈**（栈可同堆栈，因此是四个名词，也是五个名词）。每次我看到这几个名词，都隐隐约约觉得自己在被一只无形的大口慢慢地吞噬，只剩下满地的衣服碎屑（为什么不是骨头，因为骨头也好吃）。

十几年后，再读《狂人日记》，恍然如梦：

> 鲁迅先生以狂人的口吻，再现了动乱时期下中国人的精神状态，视角新颖，文笔细腻又不乏辛辣之味。
> 当时的中国，混乱成了主色调。以清廷和孔教为主的封建旧思想还在潜移默化地影响着人们的思想，与此同时以革命和新思潮为主的现代思想已经开始了对大众灵魂的洗涤和冲击。

最近，和沉默王二技术交流群（120926808）的群友们交流后，Java中那四五个会吃人的名词：对象、引用、堆、栈、堆栈，似乎在脑海中也清晰了起来，尽管疑惑有时候仍然会在阴云密布时跑出来——正鉴于此，这篇文章恰好做一下归纳。

### 一、对象和引用

**在Java中，尽管一切都可以看做是对象，但计算机操作的并非对象本身，而是对象的引用。** 这话乍眼一看，似懂非懂。究竟什么是对象，什么又是引用呢？

先来看对象的定义：按照通俗的说法，**每个对象都是某个类（class）的一个实例（instance）**。那么，实例化的过程怎么描述呢？来看代码（类是String）：

```
new String("我是对象张三");
new String("我是对象李四");
```

**在Java中，实例化指的就是通过关键字“new”来创建对象的过程**。以上代码在运行时就会创建两个对象——"我是对象张三"和"我是对象李四"；现在，该怎么操作他们呢？

我们都去过公园，见过几个大爷，他们很有一番本领——个个都能把风筝飞得老高老高，徒留我们眼馋的份！风筝飞那么高，没办法直接用手拽着飞啊，全要靠一根长长的看不见的结实的绳子来牵引！操作Java对象也是这个理，得有一根绳——也就是接下来要介绍的“引用”（我们肉眼也常常看不见它）。

```
String zhangsan, lisi;
zhangsan = new String("我是对象张三");
lisi = new String("我是对象李四");
```

这三行代码该怎么理解呢？

先来看第一行代码：`String zhangsan, lisi;`——声明了两个变量zhangsan和lisi，他们的类型为String。

①、歧义：zhangsan和lisi此时被称为引用。

你也许听过这样一句古文：“**神之于形，犹利之于刀；未闻刀没而利存，岂容形亡而神在？**”这是无神论者范缜（zhen）的名言，大致的意思就是：灵魂对于肉体来说，就像刀刃对于刀身；从没听说过刀身都没了刀刃还存在，那么怎么可能允许肉体死亡了而灵魂还在呢？

**“引用”之于对象，就好比刀刃之于刀身，对象还没有创建，又怎么存在对象的“引用”呢？**

如果zhangsan和lisi此时不能被称为“引用”，那么他们是什么呢？答案很简单，就是变量啊！（鄙人理解）

②、误解：zhangsan和lisi此时的默认值为`null`。

应该说zhangsan和lisi此时的值为`undefined`——借用JavaScript的关键字；也就是未定义；或者应该是一个新的关键字`uninitialized`——未初始化。但不管是`undefined`还是`uninitialized`，都与`null`不同。

既然没有初始化，zhangsan和lisi此时就不能被使用。假如强行使用的话，编译器就会报错，提醒zhangsan和lisi还没有出生（初始化）；见下图。

![图片](https://mmbiz.qpic.cn/mmbiz_png/z40lCFUAHpmSC3DSh9MC8QiaZiakWP0mHCNHs3SgxauYqH7dfThPqpXLcy1XmBFqk8KS6V6V2N1xjWTy3QtZKgjQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

如果把zhangsan和lisi初始化为`null`，编译器是认可的（见下图）；由此可见，zhangsan和lisi此时的默认值不为`null`。

![图片](https://mmbiz.qpic.cn/mmbiz_png/z40lCFUAHpmSC3DSh9MC8QiaZiakWP0mHCtBhECcmKL8DW8nGZ8pZWGOic829gaf3Ro8wqibHII0ldETvciaYHtKFKg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

再来看第二行代码：`zhangsan = new String("我是对象张三");`——创建“我是对象张三"的String类对象，并将其赋值给zhangsan这个变量。

此时，zhangsan就是"我是对象张三"的引用；“=”操作符赋予了zhangsan这样神圣的权利。

第三行代码`lisi = new String("我是对象李四");`和第二行代码`zhangsan = new String("我是对象张三");`同理。

现在，我可以下这样一个结论了——**对象是通过`new`关键字创建的；引用是依赖于对象的；`=`操作符把对象赋值给了引用**。

我们再来看这样一段代码：

```java
String zhangsan, lisi;
zhangsan = new String("我是对象张三");
lisi = new String("我是对象李四");
zhangsan = lisi;
```

当`zhangsan = lisi;`执行过后，zhangsan就不再是"我是对象张三"的引用了；zhangsan和lisi指向了同一个对象（"我是对象李四"）；因此，你知道`System.out.println(zhangsan == lisi);`打印的是`false`还是`true`了吗？

### 二、堆、栈、堆栈

谁来告诉我，为什么有很多地方（书、博客等等）把栈叫做堆栈，把堆栈叫做栈？搞得我都头晕目眩了——绕着门柱估计转了80圈，不晕才怪！

我查了一下金山词霸，结果如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/z40lCFUAHpmSC3DSh9MC8QiaZiakWP0mHCj8OO5nupmycuwht01HibVPgN2UysUiaic5CViaKzAsBBdhBAIeBHpAzx2g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我的天呐，更晕了，有没有！怎么才能不晕呢？我这里有几招武功秘籍，你们尽管拿去一睹为快：

1）以后再**看到堆、栈、堆栈三个在一起打牌的时候，直接把“堆栈”踢出去**；这仨人不适合在一起玩，因为堆和栈才是老相好；你“堆栈”来这插一脚算怎么回事；这世界上只存在“堆、栈”或者“堆栈”（标点符号很重要哦）。

2）**堆是在程序运行时在内存中申请的空间（可理解为动态的过程）；切记，不是在编译时**；因此，Java中的对象就放在这里，这样做的好处就是：

> 当需要一个对象时，只需要通过new关键字写一行代码即可，当执行这行代码时，会自动在内存的“堆”区分配空间——这样就很灵活。

另外，需要记住，**堆遵循“先进后出”的规则**。就好像，一个和尚去挑了一担水，然后把一担水装缸里面，等到他口渴的时候他再用瓢舀出来喝。请放肆地打开你的脑洞脑补一下这个流程：缸底的水是先进去的，但后出来的。所以，我建议这位和尚在缸上贴个标签——保质期90天，过期饮用，后果自负！

还是记不住，看下图：

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/z40lCFUAHpmSC3DSh9MC8QiaZiakWP0mHCfOjwjLfkibbAfhjPM8rUFuFGFsEu6YY8DUAhYHPbDBGgBuHqfCxLACA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)不好意思，这是鼎，不是缸，将就一下哈



3）栈，又名堆栈（简直了，完全不符合程序员的思维啊，我们陈许愿习惯说一就是一，说二就是二嘛），能够和处理器（CPU，也就是脑子）直接关联，因此访问速度更快；举个十分不恰当的例子哈——**眼睛相对嘴巴是离脑子近的一方，因此，你可以一目十行，但绝对做不到一开口就读十行字，哪怕十个字也做不到**。

既然访问速度快，要好好利用啊！**Java就把对象的引用放在栈里**。为什么呢？因为引用的使用频率高吗？

不是的，**因为Java在编译程序时，必须明确的知道存储在栈里的东西的生命周期**，否则就没法释放旧的内存来开辟新的内存空间存放引用——空间就那么大，前浪要把后浪拍死在沙滩上啊。

现在清楚堆、栈和堆栈了吧？

### 三、特殊的“对象”

先来看《Java编程思想》中的一段话：

> 在程序设计中经常用到一系列类型，他们需要特殊对待。之所以特殊对待，是因为new将对象存储于“堆”中，故用new创建一个对象──特别小、简单的变量，往往不是很有效。因此，不用new来创建这类变量，而是创建一个并非是引用的变量，这个变量直接存储值，并置于栈中，因此更加高效。

在Java中，这些基本类型有：boolean、char、byte、short、int、long、float、double和void；还有与之对应的包装器：Boolean、Character、Byte、Short、Integer、Long、Float、Double和Void；他们之间涉及到装箱和拆箱，我们有机会再聊。

看两行简单的代码：

```java
 int a = 3;
 int b = 3;
```

这两行代码在编译的时候是什么样子呢？

编译器当然是先处理`int a = 3;`，不然还能跳过吗？编译器在处理`int a = 3;`时在栈中创建了一个变量为a的内存空间，然后查找有没有字面值为3的地址，没找到，就开辟一个存放3这个字面值的地址，然后将a指向3的地址。

编译器忙完了`int a = 3;`，就来接着处理`int b = 3;`；在创建完b的变量后，由于栈中已经有3这个字面值，就将b直接指向3的地址；就不需要再开辟新的空间了。

依据上面的概述，我们假设在定义完a与b的值后，再令a=4，此时b是等于3呢，还是4呢？

思考一下，再看答案哈。

答案揭晓：当编译器遇到`a = 4;`时，它会重新搜索栈中是否有4的字面值，如果没有，重新开辟地址存放4的值；如果已经有了，则直接将a指向4这个地址；因此a值的改变不会影响到b的值哦。

最后，留个作业吧，下面这段代码在运行时会输出什么呢？

```java
public class Test1 {
    public static void main(String args[]) {
        int a = 1;
        int b = 1;

        a = 2;

        System.out.println(a);
        System.out.println(b);

        TT t = new TT("T");
        TT t1 = t;
        t.setName("TT");


        System.out.println(t.getName());
        System.out.println(t1.getName());
    }
}

class TT{
    private String name;

    public TT (String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }

    public void setName(String name1) {
        this.name = name1;
    }
}
```





