# 基础信息

|      |      |
|------|------|
| 名称 | binarywang |
| 编码语言 | .java |
| 代码路径 | weixin-java-miniapp-demo/src/main/java/com/github/binarywang |
| 包名 | docs.src.main.java.com.github.binarywang |
| 概述说明 | 微信小程序后端核心模块，含媒体管理、用户会话和消息路由功能，支持多账号配置，采用Spring Boot框架，包含错误处理和JSON工具类。 |

# 说明

## 概述  
该模块是微信小程序后端服务集合，核心职责包括媒体文件管理、用户会话服务和微信消息路由，同时集成错误页面处理和配置管理功能。采用基于appid的多租户架构，接口规范遵循Spring MVC标准，关键数据结构涵盖media_id列表、用户会话JSON、微信消息对象及WxMaProperties配置类。外部依赖微信SDK加密服务、HTTP请求处理和Spring框架。例如上传接口返回media_id，登录接口返回sessionKey，错误处理自动路由404页面。

## 主要业务场景  
模块支持三类核心流程：1)媒体文件管理类似CDN操作；2)用户认证遵循OAuth2.0模式；3)消息路由采用事件总线机制。典型交互为请求→验证→执行→清理→响应闭环，完整覆盖小程序后台开发需求。多租户配置管理支持并行处理多个小程序实例，错误处理通过状态码映射实现自动跳转。例如通过code换取会话，或根据消息类型路由到对应处理器链。


### 包内部结构视图

```mermaid
graph TD
    binarywang --> demo
    demo --> wx
    wx --> miniapp
    miniapp --> controller
    miniapp --> utils
    miniapp --> error
    miniapp --> config
    miniapp --> WxMaDemoApplication.java
    controller --> WxMaMediaController.java
    controller --> WxMaUserController.java
    controller --> WxPortalController.java
    utils --> JsonUtils.java
    error --> ErrorController.java
    error --> ErrorPageConfiguration.java
    config --> WxMaProperties.java
    config --> WxMaConfiguration.java
```

该流程图展示了微信小程序demo项目的核心结构，从根目录binarywang开始，逐级展开到miniapp模块，包含controller、utils、error、config等子模块及其对应的Java类文件。每个节点仅显示最后一级名称，清晰呈现了项目的层级关系和组织架构。

# 文件列表

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| [demo](demo/_module.md) | package | 微信小程序后端核心模块，含媒体管理、用户会话和消息路由功能，支持多账号配置，采用Spring Boot框架，包含错误处理和JSON工具类。 |


