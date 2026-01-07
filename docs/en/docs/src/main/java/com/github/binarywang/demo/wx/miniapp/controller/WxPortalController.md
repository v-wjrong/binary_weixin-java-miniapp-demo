# Basic Information

|      |      |
|------|------|
| Name | WxPortalController |
| Language | .java |
| Code Path | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/controller/WxPortalController.java |
| Package Name | com.github.binarywang.demo.wx.miniapp.controller |
| Dependencies | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.bean.WxMaMessage', 'cn.binarywang.wx.miniapp.constant.WxMaConstants', 'cn.binarywang.wx.miniapp.message.WxMaMessageRouter', 'cn.binarywang.wx.miniapp.util.WxMaConfigHolder', 'lombok.AllArgsConstructor', 'lombok.extern.slf4j.Slf4j', 'org.apache.commons.lang3.StringUtils', 'org.springframework.web.bind.annotation', 'java.util.Objects'] |
| Brief Description | This controller is used to handle GET and POST requests from WeChat Mini Programs, supporting message signature verification, decryption, and routing processing. The GET method is used for server authentication, while the POST method is used to receive and parse user messages. It supports both plaintext and AES encryption transmission methods, and automatically switches between JSON or XML format data processing based on configuration. |

# Description

This controller is used to handle WeChat Mini Program access authentication and message push. Server validity verification is completed through GET requests, returning echostr to confirm the legitimacy of the request; POST requests receive and parse messages sent by users, supporting both plaintext and AES encryption methods. It automatically switches between JSON or XML format parsing based on configuration, and routes messages to designated processors. All operations verify appid legitimacy to ensure security.

# Class Summary

| Name   | Type  | Description |
|-------|------|-------------|
| WxPortalController | class | This controller is used to handle GET and POST requests from WeChat Mini Programs, implementing server verification and message receiving functions. The GET method is used to verify signatures and return echostr, while the POST method parses plaintext or AES-encrypted message content and distributes processing through routing. It supports JSON and XML format data, ensures thread safety, and cleans up context. |



## Class WxPortalController

|      |      |
|------|------|
| Access Modifier | @RestController;@AllArgsConstructor;@RequestMapping("/wx/portal/{appid}");@Slf4j;public |
| Type | class |
| Name | WxPortalController |
| Description | This controller is used to handle GET and POST requests from WeChat Mini Programs, implementing server verification and message receiving functions. The GET method is used to verify signatures and return echostr, while the POST method parses plaintext or AES-encrypted message content and distributes processing through routing. It supports JSON and XML format data, ensures thread safety, and cleans up context. |


### UML Class Diagram

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

    WxPortalController --> WxMaService : Dependency
    WxPortalController --> WxMaMessageRouter : Dependency
    WxPortalController --> WxMaMessage : Dependency
    WxPortalController --> WxMaConfigHolder : Dependency
    WxMaService --> WxMaConfig : Dependency
    WxMaMessage --> WxMaConfig : Dependency
```

This class diagram shows the structure of the WeChat Mini Program portal controller `WxPortalController` and its related dependencies. It handles GET and POST requests through interface calls, completing functions such as signature verification, message parsing, and routing forwarding. It also depends on multiple WeChat Mini Program service interfaces and configuration classes to implement a complete business logic processing flow.


### Internal Method Call Graph

```mermaid
graph TD
    A["Class WxPortalController"]
    B["Property: WxMaService wxMaService"]
    C["Property: WxMaMessageRouter wxMaMessageRouter"]
    D["GET Method: authGet(...)"]
    E["POST Method: post(...)"]
    F["Private Method: route(WxMaMessage message)"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F

    D --> D1["Log request parameters"]
    D1 --> D2["Validate parameter completeness"]
    D2 --"Missing parameters"--> D3["Throw exception: Invalid request parameters"]
    D2 --"Parameters complete"--> D4["Switch appid configuration"]
    D4 --"Switch failed"--> D5["Throw exception: Corresponding appid configuration not found"]
    D4 --"Switch successful"--> D6["Verify signature"]
    D6 --"Verification passed"--> D7["Clean up ThreadLocal"]
    D7 --> D8["Return echostr"]
    D6 --"Verification failed"--> D9["Clean up ThreadLocal"]
    D9 --> D10["Return 'illegal request'"]

    E --> E1["Log request information"]
    E1 --> E2["Switch appid configuration"]
    E2 --"Switch failed"--> E3["Throw exception: Corresponding appid configuration not found"]
    E2 --"Switch successful"--> E4["Check if message format is JSON"]
    E4 --> E5{"Is encryptType null?"}
    E5 --"Yes"--> E6["Parse plaintext message"]
    E6 --> E7["Call route method to process message"]
    E7 --> E8["Clean up ThreadLocal"]
    E8 --> E9["Return 'success'"]
    E5 --"No"--> E10{"Is encryptType 'aes'?"}
    E10 --"Yes"--> E11["Parse AES encrypted message"]
    E11 --> E7
    E10 --"No"--> E12["Clean up ThreadLocal"]
    E12 --> E13["Throw exception: Unrecognized encryption type"]

    F --> F1["Call wxMaMessageRouter.route(message)"]
    F1 --> F2{"Exception occurred?"}
    F2 --"Yes"--> F3["Record error log"]
    F2 --"No"--> F4["End"]
```

This flowchart illustrates the main logic of the WeChat public account access controller `WxPortalController`. It includes the GET request for server authentication and the POST request for receiving and processing user messages, covering key steps such as parameter validation, configuration switching, message decryption, and routing. ThreadLocal cleanup is performed at critical nodes to prevent memory leaks.

### Field List

| Name  | Type  | Description |
|-------|-------|------|
| wxMaMessageRouter | WxMaMessageRouter | This is a private constant instance of a WeChat Mini Program message router, used to handle and route message requests for WeChat Mini Programs. |
| wxMaService | WxMaService | This is a private constant field declaration for a WeChat Mini Program service interface, used to provide WeChat Mini Program related function calls within the class. |

### Method List

| Name  | Type  | Description |
|-------|-------|------|
| authGet | String | This interface is used to handle GET authentication requests from the WeChat server, verify the legitimacy of the signature, and return the echostr or error information. |
| post | String | This interface handles WeChat Mini Program message push notifications, supporting both plaintext and AES encryption transmission methods. It parses and routes messages based on their format (JSON or XML), ensures thread safety, and returns a successful response. |
| route | void | This method is used to route WeChat Mini Program messages, processing messages through wxMaMessageRouter, and logging error logs if exceptions occur during processing. |




