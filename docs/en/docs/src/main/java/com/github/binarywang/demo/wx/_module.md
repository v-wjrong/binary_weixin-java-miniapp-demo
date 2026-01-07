# Basic Information

|      |      |
|------|------|
| Name | wx |
| Language | .java |
| Code Path | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx |
| Package Name | docs.src.main.java.com.github.binarywang.demo.wx |
| Brief Description | This module provides backend services for WeChat Mini Programs, supporting multi-instance configuration, user authentication, media management, and message push notifications. It adopts a RESTful interface design, integrates the WxJava SDK with the Spring Boot framework, implements functionalities such as file upload, JSON parsing, and AES encrypted communication, and enhances system stability through a unified error handling mechanism. |

# Description

## Overview

This module provides backend core services for WeChat Mini Programs, supporting multi-instance configuration, user authentication, message processing, and media resource management. Request isolation is achieved through AppId routing and thread-local variables, while integration with the WxJava SDK enables communication according to WeChat protocols. For example, uploading images returns a MediaId, or obtaining a user session based on a code.

The interfaces follow RESTful style, supporting Multipart file transfer, JSON/XML parsing, and AES encrypted communication. Key dependencies include wx-java-miniapp-spring-boot-starter, commons-fileupload, and Spring Web-related components. Important data structures encompass WxMaConfig, WxMaUserInfo, WxMaJscode2SessionResult, and WxMpXmlMessage among others.

Additionally, it includes a unified error handling mechanism that renders 404/500 status views via ErrorController and ErrorPageConfiguration. Similar to an event bus architecture, error requests are centrally dispatched to the /error path where Thymeleaf template pages are displayed.

The module uses the JsonUtils utility class for JSON serialization operations, configured with Jackson's ObjectMapper to ignore null values and format output. The overall structure follows standard Spring Boot conventions, initialized by the WxMaDemoApplication startup class.

## Main Business Scenarios

The module integrates three major interaction flows of WeChat Mini Programs: user login, message push, and material management. The interaction pattern resembles an event bus architecture, with the Portal Controller distributing requests uniformly. For instance, GET validates URL validity, POST receives user behavior data, which is then processed by Service components executing specific logic.

It supports a complete lifecycle from configuration loading to service operation. Multi-instance parameters are bound using WxMaProperties, and different types of events are routed via a message router to handlers such as logs, text replies, or image responses. Typical applications include returning QR codes upon scanning or triggering message pushes via subscription notifications.

API types cover HTTP interfaces at the Controller layer, business logic in the Service layer, and custom message handler registration mechanisms, suitable for deployment within Spring Boot microservices environments. It also integrates a unified error page mechanism to enhance front-end experience consistency, allowing developers to quickly reuse and build custom error prompt interfaces.


### Package Internal Structure View

```mermaid
graph TD
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

This flowchart illustrates the module structure of a WeChat Mini Program Java backend project, where `wx` serves as the root module, branching into multiple sub-modules such as controllers, utilities, error handling, and configuration, clearly demonstrating the project's organizational architecture and file dependencies.

# File List

| Name   | Type  | Description |
|-------|------|-------------|
| [miniapp](miniapp/_module.md) | package | This module provides backend services for WeChat Mini Programs, supporting multi-instance configuration, user authentication, media management, and message push. It adopts a RESTful interface design, integrates the WxJava SDK with the Spring Boot framework, implements functions such as file upload, JSON parsing, and AES encrypted communication, and enhances system stability through a unified error handling mechanism. |


