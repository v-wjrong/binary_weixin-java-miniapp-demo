
# weixin-java-miniapp-demo

## 概述

本工程是一个面向微信小程序的后端服务框架，定位为轻量级、可扩展的小程序接入中心。它解决微信生态中用户认证、消息处理与资源管理等核心交互问题，支持多实例部署，具备良好的隔离性与稳定性。其架构基于Spring Boot微服务风格设计，整合WxJava SDK实现对微信开放协议的高效对接，类似分布式网关中转请求并路由至不同业务单元。

模块通过AppId识别不同小程序实例，并借助ThreadLocal实现上下文隔离，确保并发安全。核心功能包括登录凭证校验（code换取session）、消息订阅与回调、以及图片、音频等媒体文件上传下载。此外，系统内置统一异常处理机制，将错误请求集中导向/error路径，配合Thymeleaf模板引擎呈现友好提示页面，提高开发调试效率和用户体验一致性。

## 什么是weixin-java-miniapp-demo?

该项目是基于WxJava开源SDK构建的微信小程序后端演示工程，集成了用户登录、消息接收与事件分发、素材上传下载等多个功能模块。各组件之间通过Spring容器协调工作，形成清晰的服务调用链路，类似消息中间件按类型转发事件到指定处理器。例如当收到文本消息时，会自动匹配至TextMsgHandler进行响应构建。

技术上采用RESTful API风格暴露接口，支持JSON/XML数据交换及AES加密通信，保障传输安全性。通过wx-java-miniapp-spring-boot-starter简化配置流程，开发者只需声明WxMaConfig即可启用对应实例。典型应用场景包括电商小程序中的订单通知推送、教育平台的消息订阅确认等，具有高度复用性和二次开发适配能力。

## 快速导航

### 💻 开发者

- [开发指南](summary/dev_guide.md) - 快速上手项目开发


- [模块说明](docs/_module.md) - 项目模块详细说明


### 🏗️ 架构师

- [系统架构](summary/system_architecture.md) - 系统架构


### ↔️ API 文档

- <a href='http://52.237.88.89/wiki/binary/weixin-java-miniapp-demo/acb95743d3a86fe1e043ca6537768e9719883ee0/api-viewer.html' target='_blank'>API 文档</a> - API 文档

