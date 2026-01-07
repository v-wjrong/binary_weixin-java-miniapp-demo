# 基础信息

|      |      |
|------|------|
| 名称 | weixin-java-miniapp-demo |
| 编码语言 | .java |
| 代码路径 | weixin-java-miniapp-demo |
| 概述说明 | 该模块为微信小程序提供后端核心服务，支持多实例配置、用户认证、消息处理与媒体资源管理。通过AppId路由和线程本地变量实现请求隔离，结合WxJava SDK完成微信协议对接。接口遵循RESTful风格，支持Multipart文件传输、JSON/XML解析及AES加密通信。主要依赖包括wx-java-miniapp-spring-boot-starter、commons-fileupload及Spring Web相关组件。关键数据结构有WxMaConfig、WxMaUserInfo、WxMaJscode2SessionResult和WxMpXmlMessage等。模块使用JsonUtils工具类进行JSON序列化操作，基于Jackson ObjectMapper配置空值忽略与格式化输出功能。整体采用Spring Boot标准结构，由WxMaDemoApplication启动类引导初始化流程。模块整合了微信小程序三大交互流程：用户登录、消息推送与素材管理，交互模式类似事件总线架构，由Portal Controller统一分发请求。支持从配置加载到服务运行的完整生命周期，通过WxMaProperties绑定多实例参数，利用消息路由器分发不同类型事件至日志、文本回复或图片响应处理器。典型应用如扫码返回二维码、订阅通知触发消息推送等。API类型涵盖Controller层HTTP接口、Service层业务逻辑及自定义消息处理器注册机制，适用于Spring Boot微服务部署环境。同时集成统一错误页面机制，提升前端体验一致性，开发者可快速复用构建自定义错误提示界面。 |

# 模块列表

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| [_module.md](src/main/java/com/_module.md) | folder | 该模块为微信小程序提供后端服务，支持多实例配置、用户认证、媒体管理及消息推送。采用RESTful接口设计，集成WxJava SDK与Spring Boot框架，实现文件上传、JSON解析、AES加密通信等功能，并通过统一错误处理机制提升系统稳定性。 |


