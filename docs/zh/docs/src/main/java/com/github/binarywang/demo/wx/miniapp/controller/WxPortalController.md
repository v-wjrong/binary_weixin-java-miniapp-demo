# 基础信息

|      |      |
|------|------|
| 名称 | WxPortalController |
| 编码语言 | .java |
| 代码路径 | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/controller/WxPortalController.java |
| 包名 | com.github.binarywang.demo.wx.miniapp.controller |
| 依赖项 | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.bean.WxMaMessage', 'cn.binarywang.wx.miniapp.constant.WxMaConstants', 'cn.binarywang.wx.miniapp.message.WxMaMessageRouter', 'cn.binarywang.wx.miniapp.util.WxMaConfigHolder', 'lombok.AllArgsConstructor', 'lombok.extern.slf4j.Slf4j', 'org.apache.commons.lang3.StringUtils', 'org.springframework.web.bind.annotation', 'java.util.Objects'] |
| 概述说明 | 该控制器用于处理微信小程序的GET和POST请求，支持消息签名校验、解密及路由处理。 |

# 说明

该控制器用于处理微信小程序接入请求，支持GET和POST方法。GET请求用于验证服务器有效性，接收signature、timestamp、nonce、echostr等参数，校验通过后返回echostr。POST请求用于接收微信消息，支持明文和AES加密两种方式，根据encrypt_type判断并解析消息内容，最终由消息路由器进行处理。所有操作完成后会清理线程上下文。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| WxPortalController | class | 该控制器用于处理微信小程序的GET和POST请求，实现服务器验证与消息接收功能。GET方法用于校验签名并返回echostr，POST方法解析明文或AES加密的消息体并路由处理。支持JSON和XML格式，通过appid切换配置，确保线程安全。 |



## 类 WxPortalController

|      |      |
|------|------|
| 访问范围 | @RestController;@AllArgsConstructor;@RequestMapping("/wx/portal/{appid}");@Slf4j;public |
| 类型 | class |
| 名称 | WxPortalController |
| 说明 | 该控制器用于处理微信小程序的GET和POST请求，实现服务器验证与消息接收功能。GET方法用于校验签名并返回echostr，POST方法解析明文或AES加密的消息体并路由处理。支持JSON和XML格式，通过appid切换配置，确保线程安全。 |


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

    class StringUtils {
        <<Interface>>
        +isAnyBlank(String... strings) boolean
    }

    WxPortalController --> WxMaService : 依赖
    WxPortalController --> WxMaMessageRouter : 依赖
    WxPortalController --> WxMaMessage : 依赖
    WxPortalController --> WxMaConfigHolder : 依赖
    WxPortalController --> StringUtils : 依赖
    WxPortalController --> WxMaConstants : 依赖
    WxMaService --> WxMaConfig : 依赖
    WxMaMessage --> WxMaConfig : 依赖
```

该类图展示了微信小程序门户控制器 `WxPortalController` 的结构及其与其他关键组件的交互关系。控制器通过接口依赖服务层（如 `WxMaService`）、消息路由（`WxMaMessageRouter`）以及消息解析工具类（如 `WxMaMessage`），实现微信服务器认证与消息处理逻辑。同时利用 `WxMaConfigHolder` 管理配置上下文，保证线程安全。整体设计遵循依赖倒置原则，便于扩展和测试。


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

    subgraph "authGet 流程"
        D1["接收参数: signature, timestamp, nonce, echostr"]
        D2["日志记录请求参数"]
        D3["校验参数是否为空"]
        D4["切换appid配置"]
        D5["验证签名"]
        D6["成功则返回echostr"]
        D7["失败则返回'非法请求'"]
        D8["清理ThreadLocal"]

        D --> D1
        D1 --> D2
        D2 --> D3
        D3 -- 参数缺失 --> D8
        D3 -- 参数完整 --> D4
        D4 -- 切换失败 --> D8
        D4 -- 切换成功 --> D5
        D5 -- 验证通过 --> D6
        D5 -- 验证失败 --> D7
        D6 --> D8
        D7 --> D8
    end

    subgraph "post 流程"
        E1["接收参数: requestBody, msgSignature, encryptType等"]
        E2["日志记录请求信息"]
        E3["切换appid配置"]
        E4["判断消息格式是否为JSON"]
        E5["判断encryptType是否为空"]
        E6["明文处理: 解析消息并路由"]
        E7["判断encryptType是否为'aes'"]
        E8["AES加密处理: 解析加密消息并路由"]
        E9["抛出异常: 不支持的加密类型"]
        E10["清理ThreadLocal"]

        E --> E1
        E1 --> E2
        E2 --> E3
        E3 -- 失败 --> E10
        E3 -- 成功 --> E4
        E4 --> E5
        E5 -- 明文 --> E6
        E6 --> E10
        E5 -- 加密 --> E7
        E7 -- AES --> E8
        E8 --> E10
        E7 -- 非AES --> E9
        E9 --> E10
    end

    subgraph "route 方法"
        F1["调用wxMaMessageRouter.route(message)"]
        F2["捕获异常并记录日志"]

        F --> F1
        F1 -- 异常 --> F2
    end

    D8 -.-> F
    E6 -.-> F
    E8 -.-> F
```

该流程图展示了`WxPortalController`类中两个主要接口`authGet`与`post`的执行逻辑。其中包含了微信认证验证、消息解密解析以及最终通过`route`方法分发至消息路由器的核心流程，并涵盖了线程上下文清理的关键步骤。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| wxMaService | WxMaService | 这是一个微信小程序服务接口的私有常量字段声明，用于在类中提供微信小程序相关功能调用。 |
| wxMaMessageRouter | WxMaMessageRouter | 这是一个微信小程序消息路由器的私有不可变实例变量，用于处理微信小程序的消息路由转发功能。 |

### 方法列表

| 名称  | 类型  | 说明 |
|-------|-------|------|
| route | void | 该方法用于路由微信小程序消息，通过wxMaMessageRouter处理传入的消息，若处理过程中发生异常则记录错误日志。 |
| post | String | 该接口处理微信小程序消息推送，支持明文和AES加密两种传输方式，根据消息格式（JSON或XML）解析并路由处理，确保线程安全并返回成功响应。 |
| authGet | String | 该接口用于处理微信服务器的认证请求，验证签名合法性并返回echostr或错误信息。 |




