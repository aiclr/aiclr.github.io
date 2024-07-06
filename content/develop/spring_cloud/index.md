+++
title = 'Spring Cloud'
date = '2024-07-03'
author = 'aiclr'
categories = ['develop']
tags = ['spring','spring boot','spring cloud','java']
Summary='Spring Cloud provides tools for developers to quickly build some of the common patterns in distributed systems (e.g. configuration management, service discovery, circuit breakers, intelligent routing, micro-proxy, control bus, short lived microservices and contract testing). Coordination of distributed systems leads to boiler plate patterns, and using Spring Cloud developers can quickly stand up services and applications that implement those patterns. They will work well in any distributed environment, including the developer’s own laptop, bare metal data centres, and managed platforms such as Cloud Foundry.'
+++

[https://spring.io/projects/spring-cloud](https://spring.io/projects/spring-cloud)

- 服务治理是SpringCloud的核心
    - Consul
    - Eureka(Netflix)
        - spring cloud eureka 是 Spring Cloud 对 Netflix 开源的一款服务治理产品进行二次封装而出现的一个子项目


- 微服务治理
    - 服务注册
    - 服务发现
    - 服务注销
    - 服务状态监控
    - 负载均衡
- 微服务网关
    - 微服务映射
    - 服务路由管理
    - 请求过滤
    - AB测试
    - 金丝雀测试
- 微服务容错
    - 服务降级
    - 熔断机制
    - 超时管理
    - 回退机制
    - 服务限流
- 统一配置
    - 加载与刷新
    - 配置存储
    - 版本管理
    - 加密与解密
- 微服务监控
    - 日志聚合
    - 日志监控
    - 调用链监控
    - 可视化分析
    - 健康检查
    - Metrics监控
- 微服务通信
    - 基于RESTful协议
    - 消息中间件整合
    - 发布-订阅模式
    - 远程事件
- 微服务安全
    - Session管理
    - 单点登录
    - OAuth认证
    - JWT授权
- 微服务部署与编排
    - Docker
    - K8s
    - 服务编排
    - 自动发布

# Spring Cloud 技术概览

Spring Cloud 是在 Netflix OSS 等多家开源的基础上，使用 SpringBoot 风格将这些比较成熟的微服务框架组合起来，屏蔽掉复杂的配置和实现原理。为快速构建微服务机构的应用提供了一套基础设施工具和开发支持。

- 基于Netflix实现服务治理、客户端负载均衡、声明式调用
- 服务网关
- 微服务容错管理
- 整合消息中间件提供消息驱动式开发
- 基于SpringSecurity提供微服务安全、单点登录功能
- 分布式、版本化的统一配置管理
- 微服务调用链及追踪管理

## Spring Cloud子项目

- 对现有成熟的第三方开源项目 SpringBoot 化，开箱即用
- 新增一些微服务机构开发所需的基础设施
    - Spring Cloud Config 统一配置中心
    - Spring Cloud Stream 快速集成 Kafka，RabbitMQ 等消息中间件