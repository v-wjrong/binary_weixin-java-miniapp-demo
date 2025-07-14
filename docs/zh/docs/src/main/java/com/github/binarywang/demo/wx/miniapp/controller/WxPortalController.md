# 基础信息

|      |      |
|------|------|
| 名称 | WxPortalController |
| 编码语言 | .java |
| 代码路径 | weixin-java-miniapp-demo\src\main\java\com\github\binarywang\demo\wx\miniapp\controller\WxPortalController.java |
| 包名 | com.github.binarywang.demo.wx.miniapp.controller |
| 依赖项 | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.bean.WxMaMessage', 'cn.binarywang.wx.miniapp.constant.WxMaConstants', 'cn.binarywang.wx.miniapp.message.WxMaMessageRouter', 'cn.binarywang.wx.miniapp.util.WxMaConfigHolder', 'lombok.AllArgsConstructor', 'lombok.extern.slf4j.Slf4j', 'org.apache.commons.lang3.StringUtils', 'org.springframework.web.bind.annotation', 'java.util.Objects'] |
| 概述说明 | 微信小程序控制器，处理认证和消息请求，验证签名并路由消息，支持明文和AES加密，清理ThreadLocal。 |

# 说明

这是一个微信小程序门户控制器类，包含两个核心方法。GET方法处理微信服务器认证请求，验证签名参数后返回echostr字符串。POST方法处理微信消息，支持明文和AES加密两种格式，根据消息格式进行解析后路由处理，最后返回success响应。两个方法都会检查appid有效性，并在处理完成后清理ThreadLocal存储的配置信息。控制器通过日志记录所有请求参数，对非法请求会抛出异常。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| WxPortalController | class | 微信小程序控制器，处理认证和消息请求，验证签名并路由消息，支持明文和AES加密，返回成功或错误信息。 |



## 类 WxPortalController

|      |      |
|------|------|
| 访问范围 | @RestController;@AllArgsConstructor;@RequestMapping("/wx/portal/{appid}");@Slf4j;public |
| 类型 | class |
| 名称 | WxPortalController |
| 说明 | 微信小程序控制器，处理认证和消息请求，验证签名并路由消息，支持明文和AES加密，返回成功或错误信息。 |


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
        +getMsgDataFormat() String
    }

    class WxMaConstants {
        <<Enumeration>>
        MsgDataFormat
    }

    class WxMaConfigHolder {
        +remove() void
    }

    WxPortalController --> WxMaService : 依赖
    WxPortalController --> WxMaMessageRouter : 依赖
    WxMaMessageRouter --> WxMaMessage : 路由消息
    WxMaService --> WxMaConfig : 获取配置
    WxMaService --> WxMaConstants : 使用枚举
    WxMaMessage --> WxMaConfig : 依赖
    WxPortalController --> WxMaConfigHolder : 清理ThreadLocal
```

类图描述：该图展示了一个微信小程序门户控制器(WxPortalController)的核心结构，它依赖于微信小程序服务接口(WxMaService)和消息路由器(WxMaMessageRouter)。控制器处理两种HTTP请求：GET用于认证，POST用于消息处理。系统通过WxMaConfigHolder管理线程本地配置，支持JSON和XML两种消息格式，并实现了明文和AES加密两种消息处理方式。各组件通过清晰的接口定义进行交互，体现了良好的分层设计。


### 内部方法调用关系图

```mermaid
graph TD
    A["类WxPortalController"]
    B["属性: WxMaService wxMaService"]
    C["属性: WxMaMessageRouter wxMaMessageRouter"]
    D["GET方法: authGet"]
    E["POST方法: post"]
    F["私有方法: route"]
    G["日志记录: log.info"]
    H["参数校验: StringUtils.isAnyBlank"]
    I["切换配置: wxMaService.switchover"]
    J["签名验证: wxMaService.checkSignature"]
    K["消息路由: wxMaMessageRouter.route"]
    L["清理ThreadLocal: WxMaConfigHolder.remove"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F
    D --> G
    D --> H
    D --> I
    D --> J
    D --> L
    E --> G
    E --> I
    E --> F
    F --> K
    E --> L
```

```mermaid
sequenceDiagram
    participant Client as 微信客户端
    participant Controller as WxPortalController
    participant Service as WxMaService
    participant Router as WxMaMessageRouter

    Client->>Controller: GET /wx/portal/{appid} (携带signature/timestamp/nonce/echostr)
    Controller->>Controller: 记录日志(log.info)
    Controller->>Controller: 参数校验(StringUtils.isAnyBlank)
    Controller->>Service: 切换配置(switchover)
    Service-->>Controller: 返回结果
    Controller->>Service: 验证签名(checkSignature)
    Service-->>Controller: 返回结果
    Controller->>Controller: 清理ThreadLocal(WxMaConfigHolder.remove)
    Controller-->>Client: 返回echostr或"非法请求"

    Client->>Controller: POST /wx/portal/{appid} (携带消息体)
    Controller->>Controller: 记录日志(log.info)
    Controller->>Service: 切换配置(switchover)
    Service-->>Controller: 返回结果
    alt 明文消息
        Controller->>Controller: 解析消息(fromJson/fromXml)
        Controller->>Router: 路由消息(route)
    else AES加密消息
        Controller->>Controller: 解密消息(fromEncryptedJson/fromEncryptedXml)
        Controller->>Router: 路由消息(route)
    end
    Controller->>Controller: 清理ThreadLocal(WxMaConfigHolder.remove)
    Controller-->>Client: 返回"success"或错误
```

这段代码实现了一个微信小程序消息处理控制器，主要包含GET和POST两个核心方法。GET方法用于微信服务器认证验证，通过校验签名参数确保请求合法性；POST方法处理微信推送的各种消息，支持明文和AES加密两种格式，通过路由机制将消息分发给相应处理器。整个流程严格遵循微信开发规范，包含完善的日志记录、参数校验、配置切换和资源清理机制，体现了高可靠性的消息处理架构。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| wxMaMessageRouter | WxMaMessageRouter | 微信小程序消息路由器的私有不可变实例。 |
| wxMaService | WxMaService | 微信小程序服务实例，私有不可变。 |

### 方法列表

| 名称  | 类型  | 说明 |
|-------|-------|------|
| post | String | 处理微信请求的POST接口，支持明文和AES加密消息，校验appid后根据格式（JSON/XML）解析并路由消息，最后清理ThreadLocal返回成功或错误。 |
| route | void | 方法route接收WxMaMessage消息，调用wxMaMessageRouter.route处理，异常时记录错误日志。 |
| authGet | String | 处理微信认证请求，验证签名参数，返回echostr或错误信息。检查appid和参数合法性，清理ThreadLocal。 |




