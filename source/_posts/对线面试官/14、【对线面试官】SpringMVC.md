---
title: 14、【对线面试官】SpringMVC
date: 2022-02-28 19:57:47
tags: [面试,记录]
categories: 技术
---
# 14、【对线面试官】SpringMVC

## 今天要不来聊聊SpringMVC吧？

1. 我先简单说下我对SpringMVC的理解哈
2. SpringMVC我觉得它是对Servlet的封装，屏蔽掉Servlet很多的细节卫
3. 接下来我举几个例子
4. 可能我们刚学Servlet的时候，要获取参数需要不断的getParameter
5. 现在只要在SpringMVC方法定义对应的J avaBean，只要属性名与参数名一致，SpringMVC就可以帮我们实现「将参数封装到JavaBean」上了
6. 又比如，以前使用Servlet 「上传文件」，需要处理各种细节，写一大堆处理的逻辑（还得导入对应的jar）
7. 现在一个在SpringMVC的方法上定义出MultipartFile接口，又可以屏蔽掉上传文件的细节了。
8. 例子还有很多，我就不赘述了。

## 既然你说SpringMVC是对Servlet的封装，你了解SpringMVC请求处理的流程吗？

1. 总体流程大概是这样的
   - 首先有个统一处理请求的入口
   - 随后根据请求路径找到对应的映射器
   - 找到处理请求的适配器
   - 4）：拦截器前置处理
   - 5）：真实处理请求（也就是调用真正的代码）
   - 6）：视图解析器处理
   - 7）：拦截器后置处理

## 嗯，了解，可以再稍微深入点吗？

1. 统一的处理入口，对应SpringMVC下的源码是在DispatcherServlet下实现的
2. 该对象在初始化就会把映射器、适配器、视图解析器、异常处理器、文件处理器等等给初始化掉
3. 至于会初始化哪些具体实例，看下DispatcherServlet.properties就知道了，都配置在那了
4. 所有的请求其实都会被doService方法处理，里边最主要就是调用doDispatch方法
5. 通过doDispatch方法我们就可以看到整个SpringMVC处理的流程
6. 查找映射器的时候实际就是找到「最佳匹配」的路径，具体方法实现我记得好像是在lookupHandlerMethod方法上
7. 从源码可以看到「查找映射器」实际返回的是HandlerExecutionChain，里边有映射器Handler+拦截器List
8. 前面提到的拦截器前置处理和后置处理就是用的HandlerExecutionChain中的拦截器List
9. 获取得到HandlerExecutionChain后，就会去获取适配器，一般我们获取得到的就是RequestMappingHandlerAdapter
10. 在代码里边可以看到的是，经常用到的@ResponseBody和@Requestbody的解析器
11. 就会在初始化的时候加到参数解析器List中
12. 得到适配器之后，就会执行拦截器前置处理
13. 拦截器前置处理执行完后，就会调用适配器对象实例的hanlde方法执行真正的代码逻辑处理
14. 核心的处理逻辑在invokeAndHandle方法中，会获取得到请求的参数并调用，处理返回值
15. 参数的封装以及处理会被适配器的参数解析器进行处理，具体的处理逻辑取决于HttpMessageConverter的实例对象

## 嗯，了解了。要不你再压缩下关键的信息

1. DispatcherServlet （入口）

2. DispatcherServlet.properties（会初始化的对象）

3. HandlerMapping （映射器，

4. HandlerExecutionChain（映射器最终实例+拦截器List）

5. HttpRequestHandlerAdapter（适配器

6. HttpMessageConverter（数据转换

   ![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/ERUrGo_20211029183808.png)