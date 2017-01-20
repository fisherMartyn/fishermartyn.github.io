---
layout: post
title: 基于Spring Cloud的配置中心
categories: blog
excerpt: 利用Spring Cloud搭建简单的配置中心demo
tags: [微服务,分布式,Spring]
---

## 是什么

Spring Cloud Config是Spring Cloud的配置中心，可以解决Spring boot环境的配置问题；另外由于其本身以接口的方式提供了配置相关的数据，因此也可以支持其它环境中集中式的配置管理。

看了下Spring的文档和demo，觉得并没有起到用一个简单demo来介绍清楚`是什么`的目的，因此也就有了这篇博客。本文试图用最简单的文档和demo来讲清楚Spring cloud config的核心功能。

## 核心环节

Spring cloud配置中心的核心模块主要有三个：

1. 代码仓库
2. Server模块
3. Client模块

甚至如果从`配置中心`，而不是使用者的角度讲，只包括上面的1和2。

因为配置中心应该负责：提供配置文件的存储、以接口的形式将配置文件的内容提供出去。而客户端如何通过接口获取数据、并依据此数据初始化自己的应用，原则上是客户端自己去考虑的事情。


## 代码仓库

很多应用都将配置文件跟随源码打包发布、版本管理，这种方式相对于集中式的配置文件仓库也有很多弊端，例如生产/测试环境等管理混乱、易出错，与docker等环境兼容性差等。

理想的情况是运行环境和代码一致、采用集中式的配置中心、通过简单配置来区分环境。

配置文件的底层存储使用文件或者git等。配置文件的形式只需要遵循简单的规范，例如本demo使用的配置<a href="https://github.com/fisherMartyn/config-repo">代码仓库</a>。


## Server模块

通过Spring Boot应用和Spring Cloud提供的注解，可以用非常少的代价来部署一个配置中心的Server，主要工作包括：

1. 配置用于提供Server服务的代码仓库（即上述1的uri）
2. 指定提供服务相关的端口号，如果没有反向代理的话，要跟client一起定，防止端口冲突等问题。
3. 通过`@EnableConfigServer`注解来提供服务。

相关的demo可以参考：<a href="https://github.com/fisherMartyn/configserver">ConfigServer</a>。

启动成功后，可以通过访问本地服务来测试服务是否正常，例如：


	$ curl http://localhost:10086/app2/dev
	{"name":"app2","profiles":["dev"],"label":"master","version":"7ce00583cd328c51af64a3f85412d7203608e616","state":null,"propertySources":[{"name":"https://github.com/fisherMartyn/config-repo/app2-dev.properties","source":{"redis.passwd":"dev","redis.name":"dev"}}]}

## Client模块

Client模块主要是配置中心的主要消费者，通过配置中心的数据来初始化自身的Enviroment环境和相关变量，主要工作包括：

1. 指定App Name和active Profile，用于获取特定的配置文件。
2. 指定配置中心的uri信息
3. 通过Environment变量或者Value注解等方式来获取配置信息。

主要注意的是，注意区别spring boot程序中bootstrap配置和application配置的关系，client的信息必须写在bootstrap里。

相关的demo可以参考：<a href="https://github.com/fisherMartyn/configclient">configclient</a>。

Demo中写了相关的Rest接口来测试获取的配置信息是否正确，启动成功后，可以进行测试：

	$ curl http://localhost:8080/r
	dev
	dev

## 高层次内容

除了上述最简单的内容外，个人觉得配置中心还涉及以下两个方面的内容：

1. 高可用。 作为集中式管理的核心环节，一旦挂掉后果不堪设想。这个话题可能涉及到可用性自检、故障报警、客户端缓存等手段。
2. 通知下发。服务端配置文件的变更应该准实时的推送给客户端。这个有现成实现。

## 相关参考：

1. http://cloud.spring.io/spring-cloud-config/spring-cloud-config.html
2. https://segmentfault.com/a/1190000006138698
3. http://www.jianshu.com/p/69dea19abf04
