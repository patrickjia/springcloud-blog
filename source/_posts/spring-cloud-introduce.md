
title: Spring Cloud微服务框架主要子项目和RPC框架的对比
categories:
- Spring Cloud
tags:
- Spring Cloud
---
　**摘要**:Spring Cloud是一个相对比较新的微服务框架，今年(2016)推出1.0的release版本，目前Github上更新速度很快. 虽然Spring Cloud时间最短, 但是相比Dubbo等RPC框架, Spring Cloud提供的全套的分布式系统解决方案。spring cloud 为开发者提供了在分布式系统（配置管理，服务发现，熔断，路由，微代理，控制总线，一次性token，全局琐，leader选举，分布式session，集群状态）中快速构建的工具，使用Spring Cloud的开发者可以快速的启动服务或构建应用．它们将在任何分布式环境中工作，包括开发人员自己的笔记本电脑，裸物理机的数据中心，和像Cloud Foundry云管理平台。在未来引领这微服务架构的发展，提供业界标准的一套微服务架构解决方案。
<!--more-->

## 1.什么是Spring Cloud？
 　Spring Cloud是一个相对比较新的微服务框架，今年(2016)才推出1.0的release版本. 虽然Spring Cloud时间最短, 但是相比Dubbo等RPC框架, Spring Cloud提供的全套的分布式系统解决方案。spring cloud 为开发者提供了在分布式系统（配置管理，服务发现，熔断，路由，微代理，控制总线，一次性token，全居琐，leader选举，分布式session，集群状态）中快速构建的工具，使用Spring Cloud的开发者可以快速的启动服务或构建应用．它们将在任何分布式环境中工作，包括开发人员自己的笔记本电脑，裸物理机的数据中心，和像Cloud Foundry云管理平台。下面是官方对Spring Cloud定义和解释。
　{% blockquote %}
  　Spring Cloud provides tools for developers to quickly build some of the common patterns in distributed systems (e.g. configuration management, service discovery, circuit breakers, intelligent routing, micro-proxy, control bus, one-time tokens, global locks, leadership election, distributed sessions, cluster state). Coordination of distributed systems leads to boiler plate patterns, and using Spring Cloud developers can quickly stand up services and applications that implement those patterns. They will work well in any distributed environment, including the developer’s own laptop, bare metal data centres, and managed platforms such as Cloud Foundry.
　{% endblockquote %}

## 2.Spring Cloud主要项目
  Spring Cloud 侧重于提供良好的开箱即用的功能，以便支持典型的开发场景和扩展支持。下面主要Spring Cloud项目在微服务框架中的主要子项目，具体的子项目源码分析，以及实现细节，将会在后面的文章中介绍。
- Spring Cloud Config---配置中心
   Spring Cloud Config就是我们通常意义上的配置中心 - 把应用原本放在本地文件的配置抽取出来放在中心服务器，从而能够提供更好的管理、发布。
   >在RPC服务治理框架中，一般都会开发一个配置中心和ZK配合使用，用于管理分布式应用中的配置信息。比如熔断的阀值，负载均衡的策略等。
- Spring Cloud Netflix--注册中心，服务发现，LB
   Spring Cloud Netflix通过Eureka Server实现服务注册中心(包括服务注册，服务发现)，通过Ribbon实现软负载均衡(load balance,简称LB)
   >在RPC框架中，例如：dubboX，HSF，OSP(唯品会的RPC框架)等RPC框架，都会通过ZK等实现服务注册，服务发现。当服务启动时，会将服务的IP地址，端口，服务命名，版本号等信息注册到ZK中，同时ZK Node会监听变化，接收最新的服务注册信息到client端或Proxy端。
   >至于LB，都会有自己的实现算法，熔断等都有自己的实现方式。
- Hystrix
　　熔断，包含在服务治理中。
- Spring Cloud Sleuth
   Spring Cloud Sleuth为Spring Cloud提供了分布式追踪方案。全链路监控系统。
>　　APM（Application Performance Monitor）这个领域最近异常火热。国外该领域知名公司包括New Relic，Appdynamics，Splunk。其中New Relic已经成功IPO，估值超过20亿美元。
>　１．国内外的个大互联网公司也都有类似大名鼎鼎的APM产品，例如淘宝鹰眼Eagle Eyes，点评的CAT，微博的Watchman，twitter的Zipkin。他们的产品虽未像专业APM公司的产品这样功能强大，但结合各自公司的业务特点，这些产品在支撑业务系统的高性能和稳定性方面，发挥了显著的作用。
> 　２．众所周知，中大型互联网公司的后台业务系统由众多分布式组件构成，这些组件由web类型组件，RPC服务化类型组件，缓存组件，消息组件和数据库组件。一个通过浏览器或移动客户端的前端请求到达后台系统后，会经过很多个业务组件和系统组件，并且留下足迹和相关日志信息。但这些分散在每个业务组件和主机下的日志信息不利于问题排查和定位问题的Root Cause。这种监控场景正是应用性能监控系统的用武之地，应用性能监控系统收集，汇总并分析日志信息达到有效监控系统性能和问题的效果．
>　３．在唯品会体系中，Mercury提供的主要功能包括：
　　定位慢调用：包括慢Web服务（包括Restful Web服务），慢OSP服务，慢SQL
　　定位错误：包括4XX，5XX，OSP Error
　　定位异常：包括Error Exception，Fatal Exception
　　展现依赖和拓扑：域拓扑，服务拓扑，trace拓扑
　　Trace调用链：将端到端的调用，以及附加在这次调用的上下文信息，异常日志信息，每一个调用点的耗时都呈现给用户
　　应用告警：根据运维设定的告警规则，扫描指标数据，如违反告警规则，则将告警信息上报到唯品会中央告警平台

## 3.dubbo与Spring Cloud的比较
   　1.dubbo出自于阿里，Spring cloud出自于Spring社区,基于Spring boot提供一套完整的微服务解决方案。dubbo或者dubbox是RPC框
　架，功能是Spring Cloud功能的一个子集。
   　2.dubbo是RPC服务治理框架，和Spring Cloud一样具备服务注册、发现、路由、负载均衡等能力。但是没有配置中心，完整的好用全链路监
　控，需要采用开源的解决方案定制或者自研。Spring cloud的配置中心，全链路监控等组件。从目前来看，Spring Cloud国内中小型企业用的比较多，大型企业可能需要对其需要的组件进行定制化处理。
   　3.Spring cloud基于注解的服务发现，服务治理等功能具有代码侵入性，dubbo没有代码侵入性，业务开发人员不需要通过注解的方式去关注
框架级别的处理。从中间件或者做基础架构的角度来看，其实服务治理等功能对普通的业务程序员应该是透明的，业务程序员不需要关注服务治理框架的使用，专注于业务代码即可。
   因此大型企业可能需要对Spring cloud进行定制化处理。更多比较信息，可以参考下面的连接。
   
   [http://blog.didispace.com/microservice-framework/](http://blog.didispace.com/microservice-framework/ "Spring Cloud与Dubbo的对比")

   文章来源于:[http://blog.xujin.org/2016/10/18/spring-cloud-introduce/](http://blog.xujin.org/2016/10/18/spring-cloud-introduce/ "Spring Cloud与Dubbo的对比")