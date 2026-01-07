# Basic Information

|      |      |
|------|------|
| Name | WxMaConfiguration |
| Language | .java |
| Code Path | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/config/WxMaConfiguration.java |
| Package Name | com.github.binarywang.demo.wx.miniapp.config |
| Dependencies | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.api.impl.WxMaServiceImpl', 'cn.binarywang.wx.miniapp.bean.WxMaKefuMessage', 'cn.binarywang.wx.miniapp.bean.WxMaSubscribeMessage', 'cn.binarywang.wx.miniapp.config.impl.WxMaDefaultConfigImpl', 'cn.binarywang.wx.miniapp.config.impl.WxMaRedisConfigImpl', 'cn.binarywang.wx.miniapp.message.WxMaMessageHandler', 'cn.binarywang.wx.miniapp.message.WxMaMessageRouter', 'com.google.common.collect.Lists', 'lombok.extern.slf4j.Slf4j', 'me.chanjar.weixin.common.bean.result.WxMediaUploadResult', 'me.chanjar.weixin.common.error.WxErrorException', 'me.chanjar.weixin.common.error.WxRuntimeException', 'org.springframework.beans.factory.annotation.Autowired', 'org.springframework.boot.context.properties.EnableConfigurationProperties', 'org.springframework.context.annotation.Bean', 'org.springframework.context.annotation.Configuration', 'redis.clients.jedis.JedisPool', 'java.io.File', 'java.util.List', 'java.util.stream.Collectors'] |
| Brief Description | This configuration class is used to initialize WeChat Mini Program services and message routers, supporting multi-mini-program configurations, and defines various message processing logic, including logging, text replies, image sending, and QR code generation functions. |

# Description

This configuration class is used to initialize the WeChat Mini Program service and related message routing processing mechanisms. It completes the registration and management of multiple mini program accounts by reading configuration properties. Its core functions include building message routers and defining processing logic for various types of messages, such as text, images, and QR codes, while also supporting the sending of customer service messages and subscription message notifications. Each processor implements specific business responses, such as logging, material uploading, and QR code generation operations, and triggers execution when corresponding content is received. The overall design adopts a streaming configuration loading and component injection approach, ensuring extensibility and flexibility.

# Class Summary

| Name   | Type  | Description |
|-------|------|-------------|
| WxMaConfiguration | class | This configuration class is used to initialize the WeChat Mini Program service. It generates multiple mini program instances by reading configurations, and sets up message routing and processing logic. It supports processing various types of messages including subscription messages, text, images, and QR codes. |



## Class WxMaConfiguration

|      |      |
|------|------|
| Access Modifier | @Slf4j;@Configuration;@EnableConfigurationProperties(WxMaProperties.class);public |
| Type | class |
| Name | WxMaConfiguration |
| Description | This configuration class is used to initialize the WeChat Mini Program service. It generates multiple mini program instances by reading configurations, and sets up message routing and processing logic. It supports processing various types of messages including subscription messages, text, images, and QR codes. |


### UML Class Diagram

```mermaid
classDiagram
    class WxMaConfiguration {
        -WxMaProperties properties
        -WxMaMessageHandler subscribeMsgHandler
        -WxMaMessageHandler logHandler
        -WxMaMessageHandler textHandler
        -WxMaMessageHandler picHandler
        -WxMaMessageHandler qrcodeHandler
        +WxMaConfiguration(WxMaProperties properties)
        +WxMaService wxMaService()
        +WxMaMessageRouter wxMaMessageRouter(WxMaService wxMaService)
    }

    class WxMaProperties {
        <<Interface>>
        +List~Config~ getConfigs()
    }

    class WxMaService {
        <<Interface>>
        +void setMultiConfigs(Map~String, WxMaDefaultConfigImpl~ configs)
        +WxMaMsgService getMsgService()
        +WxMaMediaService getMediaService()
        +WxMaQrcodeService getQrcodeService()
    }

    class WxMaServiceImpl {
        +void setMultiConfigs(Map~String, WxMaDefaultConfigImpl~ configs)
        +WxMaMsgService getMsgService()
        +WxMaMediaService getMediaService()
        +WxMaQrcodeService getQrcodeService()
    }

    class WxMaDefaultConfigImpl {
        +void setAppid(String appid)
        +void setSecret(String secret)
        +void setToken(String token)
        +void setAesKey(String aesKey)
        +void setMsgDataFormat(String msgDataFormat)
        +String getAppid()
    }

    class WxMaMessageRouter {
        +WxMaMessageRouter(WxMaService wxMaService)
        +WxMaMessageRouter rule()
        +WxMaMessageRouter handler(WxMaMessageHandler handler)
        +WxMaMessageRouter next()
        +WxMaMessageRouter async(boolean async)
        +WxMaMessageRouter content(String content)
        +WxMaMessageRouter end()
    }

    class WxMaMessageHandler {
        <<Interface>>
        +Object handle(WxMaMessage wxMessage, Map~String, Object~ context, WxMaService service, WxSessionManager sessionManager)
    }

    class WxMaMessage {
        +String getFromUser()
        +String toJson()
        +String toString()
    }

    class WxSessionManager {
    }

    class WxMaSubscribeMessage {
        -String templateId
        -List~MsgData~ data
        -String toUser
        +WxMaSubscribeMessage builder()
    }

    class WxMaKefuMessage {
        +WxMaKefuMessage newTextBuilder()
        +WxMaKefuMessage newImageBuilder()
        +WxMaKefuMessage toUser(String toUser)
        +WxMaKefuMessage content(String content)
        +WxMaKefuMessage mediaId(String mediaId)
        +WxMaKefuMessage build()
    }

    class WxMediaUploadResult {
        +String getMediaId()
    }

    class WxErrorException {
    }

    // Dependencies
    WxMaConfiguration --> WxMaProperties : depends on
    WxMaConfiguration --> WxMaService : creates and configures
    WxMaConfiguration --> WxMaMessageRouter : creates and configures routing rules
    WxMaConfiguration --> WxMaDefaultConfigImpl : configures WeChat Mini Program parameters
    WxMaServiceImpl ..|> WxMaService : implements
    WxMaMessageRouter --> WxMaMessageHandler : routes messages for handling
    WxMaMessageHandler --> WxMaMessage : handles message types
    WxMaMessageHandler --> WxSessionManager : uses session manager
    WxMaMessageHandler --> WxMaSubscribeMessage : builds subscription messages
    WxMaMessageHandler --> WxMaKefuMessage : builds customer service messages
    WxMaMessageHandler --> WxMediaUploadResult : gets media upload results
    WxMaMessageHandler --> WxErrorException : handles exceptions
```

This class diagram shows the structure and relationships of the WeChat Mini Program configuration class `WxMaConfiguration` and its related components. It mainly includes core elements such as service interfaces and implementations, message handlers, routing rules, and message and configuration objects, reflecting the core logic of system initialization, multi-configuration injection, and message distribution.


### Internal Method Call Graph

```mermaid
graph TD
    A["Class WxMaConfiguration"]
    B["Property: WxMaProperties properties"]
    C["Constructor: WxMaConfiguration(WxMaProperties properties)"]
    D["Bean method: wxMaService()"]
    E["Bean method: wxMaMessageRouter(WxMaService wxMaService)"]
    F["Message handler: subscribeMsgHandler"]
    G["Message handler: logHandler"]
    H["Message handler: textHandler"]
    I["Message handler: picHandler"]
    J["Message handler: qrcodeHandler"]

    A --> B
    A --> C
    A --> D
    A --> E
    A -.-> F
    A -.-> G
    A -.-> H
    A -.-> I
    A -.-> J

    D --> "Create WxMaServiceImpl instance"
    D --> "Check if configs is empty"
    D --> "Build config map and set to maService"

    E --> "Create WxMaMessageRouter instance"
    E --> "Register logHandler rule"
    E --> "Register subscribeMsgHandler rule"
    E --> "Register textHandler rule"
    E --> "Register picHandler rule"
    E --> "Register qrcodeHandler rule"
```

This flowchart illustrates the structure and main functionalities of the `WxMaConfiguration` class. It injects `WxMaProperties` via constructor, and defines two core Beans: `wxMaService` for WeChat Mini Program service configuration, and `wxMaMessageRouter` for routing different types of messages. Additionally, it includes five custom message handlers corresponding to different business logic processing. The overall structure clearly reflects the design pattern of the WeChat Mini Program configuration class in Spring Boot.

### Field List

| Name  | Type  | Description |
|-------|-------|------|
| logHandler = (wxMessage, context, service, sessionManager) -> {        log.info("收到消息：" + wxMessage.toString());        service.getMsgService().sendKefuMsg(WxMaKefuMessage.newTextBuilder().content("收到信息为：" + wxMessage.toJson())            .toUser(wxMessage.getFromUser()).build());        return null;    } | WxMaMessageHandler | This is a WeChat Mini Program message processor, responsible for logging received messages and sending confirmation replies to users. |
| properties | WxMaProperties | This is a private constant field declaration for a WeChat Mini Program configuration property. |
| textHandler = (wxMessage, context, service, sessionManager) -> {        service.getMsgService().sendKefuMsg(WxMaKefuMessage.newTextBuilder().content("回复文本消息")            .toUser(wxMessage.getFromUser()).build());        return null;    } | WxMaMessageHandler | This is a WeChat Mini Program message handler used to process text messages sent by users. When a text message is received, the system will automatically reply with "Reply to text message" to the sending user. This handler implements the message receiving and sending functions through the WeChat customer service messaging service. |
| subscribeMsgHandler = (wxMessage, context, service, sessionManager) -> {        service.getMsgService().sendSubscribeMsg(WxMaSubscribeMessage.builder()            .templateId("此处更换为自己的模板id")            .data(Lists.newArrayList(                new WxMaSubscribeMessage.MsgData("keyword1", "339208499")))            .toUser(wxMessage.getFromUser())            .build());        return null;    } | WxMaMessageHandler | This is a WeChat Mini Program message handler used to process user subscription messages. When a user triggers a subscription event, the system automatically sends a subscription message containing the specified template ID and keyword data to the user. The handler constructs a subscription message object, sets the template ID, data content, and target user, and then calls the message service to complete the sending. |
| picHandler = (wxMessage, context, service, sessionManager) -> {        try {            WxMediaUploadResult uploadResult = service.getMediaService()                .uploadMedia("image", "png",                    ClassLoader.getSystemResourceAsStream("tmp.png"));            service.getMsgService().sendKefuMsg(                WxMaKefuMessage                    .newImageBuilder()                    .mediaId(uploadResult.getMediaId())                    .toUser(wxMessage.getFromUser())                    .build());        } catch (WxErrorException e) {            e.printStackTrace();        }        return null;    } | WxMaMessageHandler | This is a WeChat Mini Program image message processor, used to upload local image resources and send them to users. The processor uploads temporary image files in PNG format through a service, then constructs a customer service message to send the uploaded media ID to the original message sender, achieving the image reply function. Exception situations will print error stack information. |
| qrcodeHandler = (wxMessage, context, service, sessionManager) -> {        try {            final File file = service.getQrcodeService().createQrcode("123", 430);            WxMediaUploadResult uploadResult = service.getMediaService().uploadMedia("image", file);            service.getMsgService().sendKefuMsg(                WxMaKefuMessage                    .newImageBuilder()                    .mediaId(uploadResult.getMediaId())                    .toUser(wxMessage.getFromUser())                    .build());        } catch (WxErrorException e) {            e.printStackTrace();        }        return null;    } | WxMaMessageHandler | This code defines a WeChat Mini Program QR code handler, whose main functions are: receiving messages, creating QR code images, uploading them to the WeChat server to obtain media IDs, and then sending the QR code images to users via the customer service message interface. The entire process includes exception handling mechanisms. |

### Method List

| Name  | Type  | Description |
|-------|-------|------|
| wxMaService | WxMaService | This code configures the WeChat Mini Program service, reads the configuration list, and initializes multiple mini program instances to support multi-appid management. It requires correct configuration of parameters such as appid and secret; otherwise, it throws a runtime exception. |
| wxMaMessageRouter | WxMaMessageRouter | This code configures a WeChat Mini Program message router, defining processing rules for various message types, including subscription messages, text, images, and QR codes, while supporting asynchronous processing and logging functions. |




