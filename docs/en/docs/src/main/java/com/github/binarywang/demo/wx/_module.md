# Basic Information

|      |      |
|------|------|
| Name | wx |
| Language | .java |
| Code Path | weixin-java-miniapp-demo\src\main\java\com\github\binarywang\demo\wx |
| Package Name | docs.src.main.java.com.github.binarywang.demo.wx |
| Brief Description | WeChat Mini Program backend system, featuring multi-account management, message routing, error handling, media file management, and user session functionality, implemented based on Spring Boot and WeChat SDK. |

# Description

## Overview  
This module serves as the backend comprehensive system for WeChat Mini Programs, with core responsibilities including multi-account service configuration, message routing and distribution, and HTTP error handling, functioning similarly to a gateway routing and error handling hub. It manages multiple accounts through WxMaProperties, with key data structures including the Config class (Appid/Secret), a message routing Map, and an ErrorPage registry. It relies on the WeChat SDK, Spring Web, and Lombok. For example, @ConfigurationProperties injects configurations, MsgRouter predefines five types of handlers, and ErrorController uniformly renders error pages.  

## Key Business Scenarios  
The module covers the entire lifecycle of a Mini Program: initial configuration validation → building multi-account services → handling messages/errors. The interaction model is event-driven, such as text messages triggering customer service replies or 404 errors redirecting to preset pages. It fully supports WeChat protocols (e.g., subscription message distribution) and HTTP status handling. Typical workflows include media file uploads (returning media_id), user login (AES-decrypted information), and portal interactions (GET/POST dual modes). API integration examples include QR code generation and session maintenance, all implemented under a multi-tenant architecture based on AppID isolation.


### Package Internal Structure View

```mermaid
graph TD
    wx --> miniapp
    miniapp --> config
    miniapp --> WxMaDemoApplication.java
    miniapp --> utils
    miniapp --> error
    miniapp --> controller
    config --> WxMaConfiguration.java
    config --> WxMaProperties.java
    utils --> JsonUtils.java
    error --> ErrorController.java
    error --> ErrorPageConfiguration.java
    controller --> WxMaMediaController.java
    controller --> WxMaUserController.java
    controller --> WxPortalController.java
```

This flowchart illustrates the directory structure of a WeChat Mini Program Demo project, starting from the root directory `wx` and hierarchically expanding into the `miniapp` subdirectory along with its subordinate directories: `config` for configurations, `utils` for utilities, `error` for error handling, and `controller` for controllers. Each subdirectory contains corresponding implementation files, such as configuration classes, utility classes, error handling classes, and multiple controller classes, clearly presenting the hierarchical relationships between project modules.

# File List

| Name   | Type  | Description |
|-------|------|-------------|
| [miniapp](miniapp/_module.md) | package | WeChat Mini Program backend system, featuring multi-account management, message routing, error handling, media file management, and user session functionality, implemented based on Spring Boot and WeChat SDK. |


