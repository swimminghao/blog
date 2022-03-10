---
title: 2、【对线面试官】今天来聊聊Java泛型
tags:
  - 面试
  - 记录
categories: 技术
abbrlink: 5a56f011
date: 2022-02-28 19:57:47
---
#2、【对线面试官】今天来聊聊Java泛型

## 泛型了解

1. 在Java中的泛型简单来说就是：在创建对象或调用方法的时候才明确下具体的类型
2. 使用泛型的好处就是代码更加简洁（不再需要强制转换），程序更加健壮（在编译期间没有警告，在运行期就不会出现ClassCastException异常）

## 工作中用得多吗

1. 在操作集合的时候，还是很多的，毕竟方便啊。List lists = new ArrayList<>();lists.add （"面试造火箭"）；
2. 如果是其他场景的话，那就是在写「基础组件」的时候了。

## 你是怎么写的

1. 再明确一下泛型就是「在创建对象或调用方法的时候才明确下具体的类型」

2. 而组件为了做到足够的通用性，是不知道「用户」传入什么类型参数进来的所以在这种情况下用泛型就是很好的实践。

3. 这块可以参考SpringData JPA的JpaRepository写法。

   ```java
   public interface JpaRepository<T, ID> extends PagingAndSortingRepository<T, ID>, QueryByExampleExecutor<T> {
   
    List<T> findAll();
   
    List<T> findAll(Sort sort);
   
    List<T> findAllById(Iterable<ID> ids);
   
    <S extends T> List<S> saveAll(Iterable<S> entities);
   
    void flush();
   
    <S extends T> S saveAndFlush(S entity);
   
    void deleteInBatch(Iterable<T> entities);
   
    void deleteAllInBatch();
   
    T getOne(ID id);
   
    @Override
    <S extends T> List<S> findAll(Example<S> example);
   
    @Override
    <S extends T> List<S> findAll(Example<S> example, Sort sort);
   }
   ```

4. 要写组件，还是离不开Java反射机制（能够从运行时获取信息），所以一般组件是泛型+反射来实现的。

5. 回到我所讲的组件吧，背景是这样的：我这边有个需求，需要根据某些字段进行聚合。

6. 换到SQL其实就是select sum（column 1),sum(column2) from table group by fie ld1,field2

7. 需要sum和group by的列肯定是由业务方自己传入，而SQL的表其实就是我们的POJO（传入的字段也肯定是POJO的属性）

8. 单个业务实际可以在参数上写死POJO，但为了做得更加通用，我把入参设置为泛型

9. 拿到参数后，通过反射获取其字段具体的值，做累加就好了。

   ```java
   // 传入 需要group by 和 sum 的字段名
   public cacheMap(List<String> groupByKeys, List<String> sumValues) {
     this.groupByKeys = groupByKeys;
     this.sumValues = sumValues;
   }
   
   private void excute(T e) {
     
     // 从pojo 取出需要group by 的字段 list
     List<Object> key = buildPrimaryKey(e);
     
     // primaryMap 是存储结果的Map
     T value = primaryMap.get(key);
     
     // 如果从存储结果找到有相应记录
     if (value != null) {
       for (String elem : sumValues) {
         // 反射获取对应的字段，做累加处理
         Field field = getDeclaredField(elem, e);
         if (field.get(e) instanceof Integer) {
           field.set(value, (Integer) field.get(e) + (Integer) field.get(value));
         } else if (field.get(e) instanceof Long) {
           field.set(value, (Long) field.get(e) + (Long) field.get(value));
         } else {
           throw new RuntimeException("类型异常,请处理异常");
         }
       }
       
       // 处理时间记录
       Field field = getDeclaredField("updated", value);
       if (null != field) {
         field.set(value, DateTimeUtils.getCurrentTime());
       }
     } else {
       // group by 字段 第一次进来
       try {
         primaryMap.put(key, Tclone(e));
         createdMap.put(key, DateTimeUtils.getCurrentTime());
       }catch (Exception ex) {
         log.info("first put value error {}" , e);
       }
     }
   }
   ```

10. 理解了泛型的作用之后，再去审视自己代码时，就可以判断是否需要用到泛型了。

## 价值体现

1. 主要是在平时工作中，写代码的时候会多想想，遇到能用到的地方会优化下代码