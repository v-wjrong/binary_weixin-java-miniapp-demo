# Basic Information

|      |      |
|------|------|
| Name | WxMaConfiguration |
| Language | .java |
| Code Path | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/config/WxMaConfiguration.java |
| Package Name | com.github.binarywang.demo.wx.miniapp.config |
| Dependencies | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.api.impl.WxMaServiceImpl', 'cn.binarywang.wx.miniapp.bean.WxMaKefuMessage', 'cn.binarywang.wx.miniapp.bean.WxMaSubscribeMessage', 'cn.binarywang.wx.miniapp.config.impl.WxMaDefaultConfigImpl', 'cn.binarywang.wx.miniapp.config.impl.WxMaRedisConfigImpl', 'cn.binarywang.wx.miniapp.message.WxMaMessageHandler', 'cn.binarywang.wx.miniapp.message.WxMaMessageRouter', 'com.google.common.collect.Lists', 'lombok.extern.slf4j.Slf4j', 'me.chanjar.weixin.common.bean.result.WxMediaUploadResult', 'me.chanjar.weixin.common.error.WxErrorException', 'me.chanjar.weixin.common.error.WxRuntimeException', 'org.springframework.beans.factory.annotation.Autowired', 'org.springframework.boot.context.properties.EnableConfigurationProperties', 'org.springframework.context.annotation.Bean', 'org.springframework.context.annotation.Configuration', 'redis.clients.jedis.JedisPool', 'java.io.File', 'java.util.List', 'java.util.stream.Collectors'] |
| Brief Description | This class is a configuration class for WeChat Mini Programs, initializing services and message routers, handling messages of types such as subscription messages, text, images, and QR codes, and supporting the sending of customer service messages and uploading media files. |

# Description

This configuration class is used to initialize the WeChat Mini Program service and related message routing processing mechanisms. It completes the access settings for multiple mini program accounts by reading configuration properties, and defines processors for various message types, including log recording, text replies, image and QR code responses, and also supports sending customer service messages and subscription message notifications to users.

# Class Summary

| Name   | Type  | Description |
|-------|------|-------------|
| WxMaConfiguration | class | This configuration class is used to initialize WeChat Mini Program services and message routers, supporting multi-mini program configurations, and defines various message processing logic, including logging, text replies, image sending, and QR code generation functions. |



## Class WxMaConfiguration

|      |      |
|------|------|
| Access Modifier | @Slf4j;@Configuration;@EnableConfigurationProperties(WxMaProperties.class);public |
| Type | class |
| Name | WxMaConfiguration |
| Description | This configuration class is used to initialize WeChat Mini Program services and message routers, supporting multi-mini program configurations, and defines various message processing logic, including logging, text replies, image sending, and QR code generation functions. |


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
        +WxMaSubscribeMessage templateId(String templateId)
        +WxMaSubscribeMessage data(List~MsgData~ data)
        +WxMaSubscribeMessage toUser(String toUser)
        +WxMaSubscribeMessage build()
    }

    class WxMaKefuMessage {
        +WxMaKefuMessage newTextBuilder()
        +WxMaKefuMessage newImageBuilder()
        +WxMaKefuMessage content(String content)
        +WxMaKefuMessage mediaId(String mediaId)
        +WxMaKefuMessage toUser(String toUser)
        +WxMaKefuMessage build()
    }

    class WxMediaUploadResult {
        +String getMediaId()
    }

    class WxMaMsgService {
        +void sendSubscribeMsg(WxMaSubscribeMessage message)
        +void sendKefuMsg(WxMaKefuMessage message)
    }

    class WxMaMediaService {
        +WxMediaUploadResult uploadMedia(String mediaType, String fileType, InputStream inputStream)
        +WxMediaUploadResult uploadMedia(String mediaType, File file)
    }

    class WxMaQrcodeService {
        +File createQrcode(String sceneStr, int width)
    }

    class WxRuntimeException {
        +WxRuntimeException(String message)
    }

    class Lists {
        +List~T~ newArrayList(T... elements)
    }

    class WxErrorException {
    }

    // Dependencies
    WxMaConfiguration --> WxMaProperties : depends on
    WxMaConfiguration --> WxMaService : creates and configures
    WxMaConfiguration --> WxMaMessageRouter : creates and configures routing rules
    WxMaConfiguration --> WxMaMessageHandler : defines multiple handler instances
    WxMaServiceImpl ..|> WxMaService : implements
    WxMaMessageRouter --> WxMaService : routing requires service support
    WxMaMessageRouter --> WxMaMessageHandler : registers message handlers
    WxMaMessageHandler --> WxMaMessage : processes WeChat message objects
    WxMaMessageHandler --> WxSessionManager : uses session manager
    WxMaMessageHandler --> WxMaSubscribeMessage : builds subscription messages
    WxMaMessageHandler --> WxMaKefuMessage : builds customer service messages
    WxMaMessageHandler --> WxMediaUploadResult : gets media upload results
    WxMaMessageHandler --> WxMaMsgService : invokes message sending services
    WxMaMessageHandler --> WxMaMediaService : performs media uploads
    WxMaMessageHandler --> WxMaQrcodeService : generates QR codes
    WxMaMessageHandler --> WxRuntimeException : handles exceptions
    WxMaMessageHandler --> Lists : data construction helper class
    WxMaMessageHandler --> WxErrorException : catches WeChat exceptions
```

This class diagram illustrates the structure of the backend configuration center for a WeChat Mini Program, `WxMaConfiguration`, and its relationships with other core components. It is responsible for initializing WeChat services, registering multi-account configurations, and defining various types of message handlers such as text, image, and QR code handlers, which are dispatched through a router. The entire system revolves around `WxMaService` and `WxMaMessageHandler`, reflecting a highly modular design philosophy.


### Internal Method Call Graph

```mermaid
graph TD
    A["Class WxMaConfiguration"]
    B["Property: WxMaProperties properties"]
    C["Constructor: WxMaConfiguration(WxMaProperties properties)"]
    D["Bean method: wxMaService()"]
    E["Bean method: wxMaMessageRouter(WxMaService wxMaService)"]
    F["subscribeMsgHandler processor"]
    G["logHandler processor"]
    H["textHandler processor"]
    I["picHandler processor"]
    J["qrcodeHandler processor"]

    A --> B
    A --> C
    A --> D
    A --> E
    D --> "Create WxMaServiceImpl instance"
    D --> "Set multi-configs via setMultiConfigs"
    D --> "Return maService"
    E --> "Create WxMaMessageRouter"
    E --> "Register logHandler rule"
    E --> "Register subscribeMsgHandler rule"
    E --> "Register textHandler rule"
    E --> "Register picHandler rule"
    E --> "Register qrcodeHandler rule"
    E --> "Return router"

    F -.-> "Send subscription message"
    G -.-> "Log and reply customer service message"
    H -.-> "Reply with text message"
    I -.-> "Upload image and send"
    J -.-> "Generate QR code and send"
```

This flowchart illustrates the structure and core logic of the WeChat Mini Program configuration class `WxMaConfiguration`. It mainly includes the initialization process of two Beans: `wxMaService`, which is responsible for loading multi-account configurations, and `wxMaMessageRouter`, used to register different types of message handling rules. Each message handler implements specific functions such as sending text, images, or QR codes, interacting through service interfaces. The overall design supports extensibility and modular processing.

### Field List

| Name  | Type  | Description |
|-------|-------|------|
| properties | WxMaProperties | This is a private constant field for WeChat Mini Program configuration properties, used to store immutable configuration information related to the WeChat Mini Program. |
| subscribeMsgHandler = (wxMessage, context, service, sessionManager) -> {        service.getMsgService().sendSubscribeMsg(WxMaSubscribeMessage.builder()            .templateId("此处更换为自己的模板id")            .data(Lists.newArrayList(                new WxMaSubscribeMessage.MsgData("keyword1", "339208499")))            .toUser(wxMessage.getFromUser())            .build());        return null;    } | WxMaMessageHandler | This is a WeChat Mini Program message handler used to process user subscription messages. When a user triggers a subscription event, the system automatically sends a subscription message containing the specified template ID and data to the user. The handler constructs a subscription message object, sets the template ID, message data, and recipient user, and then calls the message service to send the message. |
| textHandler = (wxMessage, context, service, sessionManager) -> {        service.getMsgService().sendKefuMsg(WxMaKefuMessage.newTextBuilder().content("回复文本消息")            .toUser(wxMessage.getFromUser()).build());        return null;    } | WxMaMessageHandler | This is a WeChat Mini Program message processor, used to handle text messages and automatically reply to users. |
| picHandler = (wxMessage, context, service, sessionManager) -> {        try {            WxMediaUploadResult uploadResult = service.getMediaService()                .uploadMedia("image", "png",                    ClassLoader.getSystemResourceAsStream("tmp.png"));            service.getMsgService().sendKefuMsg(                WxMaKefuMessage                    .newImageBuilder()                    .mediaId(uploadResult.getMediaId())                    .toUser(wxMessage.getFromUser())                    .build());        } catch (WxErrorException e) {            e.printStackTrace();        }        return null;    } | WxMaMessageHandler | This is a WeChat Mini Program image message processor, used to upload local image resources and send them to users. The processor obtains media upload results through services, then constructs image customer service messages to send to specified users, including exception handling mechanisms. |
| logHandler = (wxMessage, context, service, sessionManager) -> {        log.info("收到消息：" + wxMessage.toString());        service.getMsgService().sendKefuMsg(WxMaKefuMessage.newTextBuilder().content("收到信息为：" + wxMessage.toJson())            .toUser(wxMessage.getFromUser()).build());        return null;    } | WxMaMessageHandler | This is a WeChat Mini Program message processor, responsible for logging received messages and sending confirmation replies to users. |
| qrcodeHandler = (wxMessage, context, service, sessionManager) -> {        try {            final File file = service.getQrcodeService().createQrcode("123", 430);            WxMediaUploadResult uploadResult = service.getMediaService().uploadMedia("image", file);            service.getMsgService().sendKefuMsg(                WxMaKefuMessage                    .newImageBuilder()                    .mediaId(uploadResult.getMediaId())                    .toUser(wxMessage.getFromUser())                    .build());        } catch (WxErrorException e) {            e.printStackTrace();        }        return null;    } | WxMaMessageHandler | This code defines a WeChat Mini Program message handler used to generate parameterized QR codes and send them to users. After receiving a message, the handler calls a service to create a QR code image, uploads it to the WeChat server to obtain a media ID, and finally sends the QR code image to the specified user via the customer service message interface. The entire process includes an exception handling mechanism. |

### Method List

| Name  | Type  | Description |
|-------|-------|------|
| wxMaService | WxMaService | This code configures the WeChat Mini Program service, reads the configuration list, and initializes the WxMaService instance. It supports multi-mini-program configurations, including core parameter settings such as appid, secret, and token. |
| wxMaMessageRouter | WxMaMessageRouter | This code configures a WeChat Mini Program message router, defining processing rules for various message types including subscription messages, text, images, and QR codes, and sets it to execute synchronously. |




