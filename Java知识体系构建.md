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

### feign中可配置超时时间的地方有哪些，什么关系

* `feign.httpclient` 中可配置链接超时时间及最大连接数，这两个参数不仅对httpclient有效，okhttp也是用的这两个参数

* `feign.client` 中可配置所有feign接口的链接超时及读取超时，也可配置指定某一个接口的参数

* `ribbon` 中也可配置读取超时及连接超时

* 三者参数生效的优先级是：`feign.client`>`ribbon`>`feign.httpclient`  

* 若要修改`feign.client`的配置参数，那么和其对应的okHttp最好也配置为相同的值，因为若是值不同，那么在进行请求调用的时候会每个请求都会产生一个新的okHttpClient的对象，但是，在application.yaml中是无法配置okHttpClient的读取超时时间的，因此需要用Java类来配置okHttpClient,配置文件如下所示：

  ```java
  @Configuration
  public class OkhttpConfiguration {
  
      @Bean
      @ConditionalOnMissingBean(ConnectionPool.class)
      public ConnectionPool httpClientConnectionPool(
              FeignHttpClientProperties httpClientProperties,
              OkHttpClientConnectionPoolFactory connectionPoolFactory) {
          Integer maxTotalConnections = httpClientProperties.getMaxConnections();
          Long timeToLive = httpClientProperties.getTimeToLive();
          TimeUnit ttlUnit = httpClientProperties.getTimeToLiveUnit();
          return connectionPoolFactory.create(maxTotalConnections, timeToLive, ttlUnit);
      }
  
      @Bean
      public okhttp3.OkHttpClient client(OkHttpClientFactory httpClientFactory,
                                         ConnectionPool connectionPool,
                                         FeignHttpClientProperties httpClientProperties) {
          Boolean followRedirects = httpClientProperties.isFollowRedirects();
          Integer connectTimeout = httpClientProperties.getConnectionTimeout();
          Boolean disableSslValidation = httpClientProperties.isDisableSslValidation();
          return httpClientFactory.createBuilder(disableSslValidation)
                  .connectTimeout(connectTimeout, TimeUnit.MILLISECONDS)
                  .readTimeout(5000, TimeUnit.MILLISECONDS)
                  .followRedirects(followRedirects).connectionPool(connectionPool)
                  .build();
      }
  }
  ```

  

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

