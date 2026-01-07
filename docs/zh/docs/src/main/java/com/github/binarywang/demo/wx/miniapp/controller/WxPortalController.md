# 基础信息

|      |      |
|------|------|
| 名称 | WxPortalController |
| 编码语言 | .java |
| 代码路径 | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/controller/WxPortalController.java |
| 包名 | com.github.binarywang.demo.wx.miniapp.controller |
| 依赖项 | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.bean.WxMaMessage', 'cn.binarywang.wx.miniapp.constant.WxMaConstants', 'cn.binarywang.wx.miniapp.message.WxMaMessageRouter', 'cn.binarywang.wx.miniapp.util.WxMaConfigHolder', 'lombok.AllArgsConstructor', 'lombok.extern.slf4j.Slf4j', 'org.apache.commons.lang3.StringUtils', 'org.springframework.web.bind.annotation', 'java.util.Objects'] |
| 概述说明 | 该控制器用于处理微信小程序的GET和POST请求，支持消息签名验证、解密及路由处理。GET方法用于服务器认证，POST方法用于接收并解析用户消息，支持明文和AES加密两种传输方式，并根据配置自动切换处理JSON或XML格式数据。 |

# 说明

该控制器用于处理微信小程序接入认证及消息推送。通过GET请求完成服务器有效性验证，返回echostr确认请求合法；POST请求接收并解析用户发送的消息，支持明文与AES加密两种方式，根据配置自动切换JSON或XML格式解析数据，并将消息路由至指定处理器。所有操作均校验appid合法性，确保安全性。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| WxPortalController | class | 该控制器用于处理微信小程序的GET和POST请求，实现服务器验证与消息接收功能。GET方法用于校验签名并返回echostr，POST方法解析明文或AES加密的消息内容，并通过路由分发处理。支持JSON和XML格式数据，确保线程安全并清理上下文。 |



## 类 WxPortalController

|      |      |
|------|------|
| 访问范围 | @RestController;@AllArgsConstructor;@RequestMapping("/wx/portal/{appid}");@Slf4j;public |
| 类型 | class |
| 名称 | WxPortalController |
| 说明 | 该控制器用于处理微信小程序的GET和POST请求，实现服务器验证与消息接收功能。GET方法用于校验签名并返回echostr，POST方法解析明文或AES加密的消息内容，并通过路由分发处理。支持JSON和XML格式数据，确保线程安全并清理上下文。 |


### UML类图

```mermaid
classDiagram
    class WxPortalController {
        -WxMaService wxMaService
        -WxMaMessageRouter wxMaMessageRouter
        +authGet(String appid, String signature, String timestamp, String nonce, String echostr) String
        +post(String appid, String requestBody, String msgSignature, String encryptType, String signature, String timestamp, String nonce) String
        -route(WxMaMessage message) void
    }

    class WxMaService {
        <<Interface>>
        +switchover(String appid) boolean
        +checkSignature(String timestamp, String nonce, String signature) boolean
        +getWxMaConfig() WxMaConfig
    }

    class WxMaMessageRouter {
        <<Interface>>
        +route(WxMaMessage message) void
    }

    class WxMaMessage {
        <<Interface>>
        +fromJson(String json) WxMaMessage
        +fromXml(String xml) WxMaMessage
        +fromEncryptedJson(String json, WxMaConfig config) WxMaMessage
        +fromEncryptedXml(String xml, WxMaConfig config, String timestamp, String nonce, String msgSignature) WxMaMessage
    }

    class WxMaConfig {
        <<Interface>>
        +getMsgDataFormat() String
    }

    class WxMaConstants {
        <<Interface>>
        +MsgDataFormat JSON
    }

    class WxMaConfigHolder {
        <<Interface>>
        +remove() void
    }

    WxPortalController --> WxMaService : 依赖
    WxPortalController --> WxMaMessageRouter : 依赖
    WxPortalController --> WxMaMessage : 依赖
    WxPortalController --> WxMaConfigHolder : 依赖
    WxMaService --> WxMaConfig : 依赖
    WxMaMessage --> WxMaConfig : 依赖
```

该类图展示了微信小程序门户控制器 `WxPortalController` 的结构及其相关依赖。它通过接口调用处理 GET 和 POST 请求，完成签名校验、消息解析与路由转发等功能。同时依赖于多个微信小程序服务接口及配置类来实现完整的业务逻辑处理流程。


### 内部方法调用关系图

```mermaid
graph TD
    A["类WxPortalController"]
    B["属性: WxMaService wxMaService"]
    C["属性: WxMaMessageRouter wxMaMessageRouter"]
    D["GET方法: authGet(...)"]
    E["POST方法: post(...)"]
    F["私有方法: route(WxMaMessage message)"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F

    D --> D1["日志记录请求参数"]
    D1 --> D2["校验参数完整性"]
    D2 --"参数缺失"--> D3["抛出异常: 请求参数非法"]
    D2 --"参数完整"--> D4["切换appid配置"]
    D4 --"切换失败"--> D5["抛出异常: 未找到对应appid配置"]
    D4 --"切换成功"--> D6["验证签名"]
    D6 --"验证通过"--> D7["清理ThreadLocal"]
    D7 --> D8["返回echostr"]
    D6 --"验证失败"--> D9["清理ThreadLocal"]
    D9 --> D10["返回'非法请求'"]

    E --> E1["日志记录请求信息"]
    E1 --> E2["切换appid配置"]
    E2 --"切换失败"--> E3["抛出异常: 未找到对应appid配置"]
    E2 --"切换成功"--> E4["判断消息格式是否为JSON"]
    E4 --> E5{"encryptType为空?"}
    E5 --"是"--> E6["解析明文消息"]
    E6 --> E7["调用route方法处理消息"]
    E7 --> E8["清理ThreadLocal"]
    E8 --> E9["返回'success'"]
    E5 --"否"--> E10{"encryptType为'aes'?"}
    E10 --"是"--> E11["解析AES加密消息"]
    E11 --> E7
    E10 --"否"--> E12["清理ThreadLocal"]
    E12 --> E13["抛出异常: 不可识别的加密类型"]

    F --> F1["调用wxMaMessageRouter.route(message)"]
    F1 --> F2{"是否发生异常?"}
    F2 --"是"--> F3["记录错误日志"]
    F2 --"否"--> F4["结束"]
```

该流程图展示了微信公众号接入控制器 `WxPortalController` 的主要逻辑。包括 GET 请求用于服务器认证和 POST 请求用于接收并处理用户消息，涵盖了参数校验、配置切换、消息解密与路由等关键步骤，并在各关键节点进行 ThreadLocal 清理以防止内存泄漏。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| wxMaMessageRouter | WxMaMessageRouter | 这是一个微信小程序消息路由器的私有常量实例，用于处理和路由微信小程序的消息请求。 |
| wxMaService | WxMaService | 这是一个微信小程序服务接口的私有常量字段声明，用于在类中提供微信小程序相关功能调用。 |

### 方法列表

| 名称  | 类型  | 说明 |
|-------|-------|------|
| authGet | String | 该接口用于处理微信服务器的GET认证请求，验证签名合法性并返回echostr或错误信息。 |
| post | String | 该接口处理微信小程序消息推送，支持明文和AES加密两种传输方式，根据消息格式（JSON或XML）解析并路由处理，确保线程安全并返回成功响应。 |
| route | void | 该方法用于路由微信小程序消息，通过wxMaMessageRouter处理消息，若处理过程中发生异常则记录错误日志。 |




