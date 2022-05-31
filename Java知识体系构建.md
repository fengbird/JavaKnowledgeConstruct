# Java 基础

## ArrayList

* 底层数据结构
* 扩容方式

## HashMap

* 底层数据结构
* 扩容方式

## LinkedList

* 底层数据结构
* 扩容方式

## HashSet

* 底层数据结构
* 扩容方式
* 与HashMap的关系

## 反射

# 设计模式

## 观察者

* 设计思想
* 应用场景
* 现有示例

## 单例

* 设计思想
* 应用场景
* 现有示例

## 工厂

* 设计思想
* 应用场景
* 现有示例

## 模板

* 设计思想
* 应用场景
* 现有示例

## 适配器

* 设计思想
* 应用场景
* 现有示例

## 装饰者

* 设计思想
* 应用场景
* 现有示例

# JavaWeb

## servlet容器处理请求的全过程

## cookie

## session

## jdbc

# Spring

* ioc是什么
* aop是什么
* spring体系组成部分
* 项目启动时spring如何初始化的
* spring 事务的传播方式
* spring @Transactional注解失效的几种情况
* spring 常用注解

## Springboot

* springboot相比spring的优劣势
* 项目启动后springboot如何初始化的
* 如何自定义一个spring boot starter组件

## Spring MVC

* spring mvc中的常见注解
* spring mvc处理请求的步骤
* 如何定义全局异常
* 过滤器与拦截器的区别
* 定义可变成员变量的问题

## Spring Security

* spring security 如何控制接口权限
* spring security 如何定义角色权限
* spring security 如何与jwt结合
* spring security如何与jwt及oauth2.0结合
* jwt定义
* oauth2.0定义

# Spring Cloud

## Spring Cloud Gateway

* spring cloud gateway 功能有哪些
* 核心功能如何配置

## Eureka

* 注册中心的原理
* Eureka注册的流程
* eureka订阅的流程
* eureka重连机制

## Feign

### feign中okHttp与httpclient

1. feign中同时包含了okhttp与httpclient，可通过开关自由选择，默认为httpclient
2. okhttp的默认参数配置如`timeToLive`(连接保持时间，秒)，`maxConnections`(最大连接数)，`connectionTimeout`(连接超时时间，单位毫秒)都是需要在`feign.httpclient`下进行配置，而不是在`feign.okhttp`下进行配置的
3. 项目中为什么由httpclient改为了okhttp，其实两者性能上没太大差异，至于创建对象okhttp的builder模式啥的由于用了feign，这些也涉及不到，真要说的话，OKhttp在用连接池的时候对连接池内的对象管理的更完善，httpclient针对异常的链接对象不会立马处理，会等到下次再用到的时候先报超时处理，然后再生成新的对象，虽然也能正常调用到，但这种方式性能肯定比不上OKhttp的
4. 当时项目里面遇到的坑，是由于当时项目中用feign的时候没有直接用feign对应封装的starter，而是用的feign-httpclient,但是并未引入具体的httpclient包，导致实际调用的时候并未用到ApacheHttpClient对象，而是用了Java中的HttpUrlConnection对象，这个对象性能很差，而且是短连接的，每次都得创建对象，多线程的时候很容易因为频繁创建对象导致CPU跑满的问题

### feign如何进行应用之间的调用的

#### api调用service的接口

1. 定义好的feign接口在项目启动时会被生成对应的代理对象
2. 在进行实际调用时，会进入HystrixInvocationHandler中的invoke方法
3. 该方法会从setterMethodMap中根据待调用的方法对象找到与之对应的Setter对象，然后调用执行该对象的run方法
4. 在run方法中通过当前对象的成员变量dispatch获取到方法对象对应的MethodHandler->SynchronousMethodHandler
5. 调用执行SynchronousMethodHandler的invoke方法，
   1. 在ReflectiveFeignde create方法中创建拼接待请求的请求头参数放入RequestTemplate中
   2. 执行executeAndDecode,进入其中的targetRequest方法，并从中成员变量requestInterceptors中进一步获取`自定义`的FeignInterceptor中获取信息，在此处可拿到HttpServletRequest对象，并从中获取所有的请求头信息(包括host地址)，以及在拦截器里面自行设置多的参数信息，均设置进RequestTemplate中，最后调用当前类的成员变量target的invoke方法返回最终的Feign定义的Request对象回executeAndDecode方法中，然后接下来调用client.execute(request,options)方法，这里的client根据对应的配置就会实例化为OkHttpClient or ApacheHttpClient，然后调用对应client的execute方法
6. 进入FeignInterceptor

* 有哪些核心配置项

## Ribbon

* ribbon如何做负载均衡的
* 有哪些核心配置项

## Hystrix

* 如何做限流
* 如何做熔断
* 如何定义某个异常不进行熔断

# Xxl-job

* 

# Apollo

# Mybatis

# RocketMQ

# Redis

# MYSQL

# 多线程与并发

# Tomcat

# JVM

# HTTP

# LINUX

# DOCKER & K8S

