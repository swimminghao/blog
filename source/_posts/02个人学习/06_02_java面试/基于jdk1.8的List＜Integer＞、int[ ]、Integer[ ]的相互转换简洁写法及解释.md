---
title: java8类型互转
tags:
  - java8
categories: 语法
abbrlink: 74cfd586
date: 2022-02-28 19:57:47
---

# java8类型互转

## List< Integer >、int[ ]、Integer[ ]相互转换

[toc]

下文中出现的list、ints、integers分别代表一个列表、一个int数组、一个Integer数组。
它们之间所谓的转化，其实是 复制数据，互不干扰，可以理解为深拷贝。

### int[ ] 转 List< Integer >

```java
List<Integer> list = Arrays.stream(ints).boxed().collect(Collectors.toList());
```

Arrays.stream(ints) 之后返回的类型是 IntStream，IntStream 是一个接口继承自 BaseStream，BaseStream 又继承自AutoCloseable。所以我把IntStream看成一个方便对每个整数做操作的数据流。

之后调用了 boxed()，它的作用是对每个整数进行装箱，基本类型流转换为对象流，返回的是 Stream<Integer>，Stream 也是继承自 BaseStream。所以这一步的作用是把 IntStream 转换成了 Stream<Integer>。

最后通过collect方法将数据流 (Stream<Integer>) 收集成了集合 ( List<Integer>)，这里 collect 方法里传入的是一个收集器 (Collector)，它通过 Collectors.toList() 产生。

小结：

1. Arrays.stream(ints) 将基本类型数组转换为基本类型流。 int[ ] => IntStream
2. .boxed() 将基本类型流转换为对象流。 => Stream< Integer >
3. .collect(Collectors.toList()) 将对象流收集为集合。 => List< Integer >

### int[ ] [ ] 转 List<  List < Integer >  >

```java
String temp = Arrays.deepToString(fooArr).replaceAll("\\[","").replaceAll("\\]","");
List<String> fooList = new ArrayList<>(Arrays.asList(",")); //不对
```

```java
List<List<Integer>> collect2 = Arrays.Stream(ints1).map(ar -> Arrays.stream(ar).boxed().collect(Collectors.toList())).collect(Collectors.toList());
```

### int[ ] 转 Integer[ ]

```java	
Integer[] integers = Arrays.stream(ints).boxed().toArray(Integer[]::new);
```

一样的内容就不重复了，toArray(T[ ] :: new) 方法返回基本类型数组。
小结：

1. Arrays.stream(ints) 将基本类型数组转换为基本类型流。 int[ ] => IntStream
2. .boxed() 将基本类型流转换为对象流。=> Stream< Integer >
3. .toArray(Integer[ ]::new) 将对象流转换为对象数组。=> Integer[ ]

### int[ ] [ ]转 Integer[ ] [ ]

```java
Integer[][] integers3 = Arrays.stream(ints1).map(ints -> Arrays.stream(ints).boxed().toArray(Integer[]::new)).toArray(Integer[][]::new);
```

### Integer[ ] 转 List< Integer >

```java
List<Integer> list = Arrays.asList(integers);
```

这个就很简单了，通过Arrays类里的asList方法将数组装换为List。值得注意：

asList 返回的是 Arrays 里的静态私有类 ArrayList，而不是 java.util 里的 ArrayList，它无法自动扩容。

可以用下面2种方法生成可扩容的ArrayList：

```java
List<Integer> list = new ArrayList<>(Arrays.asList(integers)); 
```

或者

```java
List<Integer> list = new ArrayList<>(); 
Collections.addAll(list, integers);
```

### Integer[ ] [ ]转 List< List < Integer > >

```java
Integer[][] a = {{1,2,3},{1,2,3}};
List<List<Integer>> lists = Arrays.stream(a).map(Arrays::asList).collect(Collectors.toList());
```

### Integer[ ] 转 int[ ]

```java
int[] ints = Arrays.stream(integers).mapToInt(Integer::valueOf).toArray();
```

map的意思是把每一个元素进行同样的操作。mapToInt的意思是把每一个元素转换为int。mapToInt(Integer::valueOf)方法返回的是IntStream。
小结：

1. Arrays.stream(integers) 将对象数组转换为对象流。 Integer[ ] => Stream< Integer >
2. .mapToInt(Integer::valueOf) 将对象流转换成基本类型流。=> IntStream
3. .toArray() 将基本类型流转换为基本类型数组。 => int[ ]

### Integer[ ] [ ]转 int[ ] [ ]

```java
Integer[][] a = {{1,2,3},{1,2,3}};      
int[][] ints1 = Arrays.stream(a).map(a1 -> Arrays.stream(a1).mapToInt(Integer::valueOf).toArray()).toArray(int[][]::new);
```

### List< Integer > 转 int[ ]

```java
int[] ints = list.stream().mapToInt(Integer::valueOf).toArray();
```

经过上面的说明，相信这里已经很好理解了，直接小结。
小结：

1. list.stream() 将列表转换为对象流。List< Integer > => Stream< Integer >
2. .mapToInt(Integer::valueOf) 将对象流转换为基本数据类型流。=> IntStream
3. .toArray() 将基本数据类型流转换为基本类型数组。=>int[ ]

### List< List < Integer > > 转 int[ ] [ ]

```java	
List<List<Integer>> lists = ...;
int[][] arrays = lists.stream()                                // Stream<List<Integer>>
        .map(list -> list.stream().mapToInt(i -> i).toArray()) // Stream<int[]>
        .toArray(int[][]::new);
```

### List< Integer > 转 Integer[ ]

```java
Integer[] integers = list.toArray(new Integer[list.size()]);
```

```java
Integer[] integers = list.stream().toArray(Integer[]::new); //不推荐
```

这个也很简单，方法里的参数是一个数组，所以要规定长度。也有无参的方法，但是要进行转型，所以不推荐使用。

### List< List < Integer > > 转 Integer[ ] [ ]

```java
List<List<Integer>> lists = ...;
Integer[][] arrays = lists.stream()                                // Stream<List<Integer>>
        .map(list -> list.toArray(new Integer[list.size()])) // Stream<Integer[]>
        .toArray(Integer[][]::new);
```

```java
Integer[][] arrays = lists.stream()                                // Stream<List<Integer>>
        .map(l -> l.stream().toArray(String[]::new)) // Stream<Integer[]>
        .toArray(Integer[][]::new);
```

```java
Integer[][] arrays = lists.stream().map(List::toArray).toArray(Integer[][]::new);
```



## List< Character >、char[ ]、Character[ ]相互转换

### char[ ] 转 List< Character >

```java
char[] chars = {'a', 'b', 'c'};
List<Character> collect = new String(chars).chars().mapToObj(i -> (char) i).collect(Collectors.toList());
```

### char[ ] [ ] 转 List<  List < Character >  >

```java
List<List<Character>> collect2 = Arrays.stream(ints1).map(chars1 -> new String(chars1).chars().mapToObj(i -> (char) i).collect(Collectors.toList())).collect(Collectors.toList());
```

### char[ ] 转 Character[ ]

```java
char[] chars = {'a', 'b', 'c'};
Character[] characters1 = new String(chars).chars().mapToObj(i -> (char) i).toArray(Character[]::new);     
```

### char[ ] [ ]转 Character[ ] [ ]

```java
Character[][] integers3 = Arrays.stream(ints1).map(chars -> new String(chars).chars().mapToObj(i->(char)i).toArray(Character[]::new)).toArray(Character[][]::new);
```

### Character[ ] 转 List< Character >

```java
List<Character> collect2 = Arrays.stream(characters).collect(Collectors.toList());
```

### Character[ ] [ ]转 List< List < Character > >

```java
List<List<Character>> lists = Arrays.stream(a).map(Arrays::asList).collect(Collectors.toList());
```

### Character[ ] 转 char[ ]

```java
char[] chars4 = Arrays.stream(characters1).map(String::valueOf).collect(Collectors.joining()).toCharArray();

```

### Character[ ] [ ]转 char[ ] [ ]

```java
char[][] ints1 = Arrays.stream(a).map(a1 -> Arrays.stream(a1).map(String::valueOf).collect(Collectors.joining()).toCharArray()).toArray(char[][]::new);
```

### List< Character > 转 char[ ]

```java
char[] value = characters.stream().map(String::valueOf).collect(Collectors.joining()).toCharArray();
```

```java
Character[] charArr = characters.toArray(new Character[characters.size()]);
char[] value = ArrayUtils.toPrimitive(charArr);
```

### List< List < Character > > 转 char[ ] [ ]

```java
char[][] arrays = lists.stream()                                // Stream<List<Integer>>
                .map(list -> list.stream().map(String::valueOf).collect(Collectors.joining()).toCharArray()) // Stream<int[]>
                .toArray(char[][]::new);
```

### List< Character > 转 Character[ ]

```java
Character[] characters2 = Arrays.stream(characters1).toArray(Character[]::new);
```

### List< List < Character > > 转 Character[ ] [ ]

```java
 Character[][] integers1 = lists.stream()                                // Stream<List<Integer>>
                .map(list -> list.toArray(new Character[list.size()])).toArray(Character[][]::new);
```

```java
Character[][] integers2 = lists.stream()                                // Stream<List<Integer>>
                .map(l -> l.stream().toArray(Character[]::new))
                .toArray(Character[][]::new);
```

```java
Character[][] integers = lists.stream().map(List::toArray).toArray(Character[][]::new);
```

