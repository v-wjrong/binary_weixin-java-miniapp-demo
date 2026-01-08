# Basic Information

|      |      |
|------|------|
| Name | WxMaConfiguration |
| Language | .java |
| Code Path | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/config/WxMaConfiguration.java |
| Package Name | com.github.binarywang.demo.wx.miniapp.config |
| Dependencies | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.api.impl.WxMaServiceImpl', 'cn.binarywang.wx.miniapp.bean.WxMaKefuMessage', 'cn.binarywang.wx.miniapp.bean.WxMaSubscribeMessage', 'cn.binarywang.wx.miniapp.config.impl.WxMaDefaultConfigImpl', 'cn.binarywang.wx.miniapp.config.impl.WxMaRedisConfigImpl', 'cn.binarywang.wx.miniapp.message.WxMaMessageHandler', 'cn.binarywang.wx.miniapp.message.WxMaMessageRouter', 'com.google.common.collect.Lists', 'lombok.extern.slf4j.Slf4j', 'me.chanjar.weixin.common.bean.result.WxMediaUploadResult', 'me.chanjar.weixin.common.error.WxErrorException', 'me.chanjar.weixin.common.error.WxRuntimeException', 'org.springframework.beans.factory.annotation.Autowired', 'org.springframework.boot.context.properties.EnableConfigurationProperties', 'org.springframework.context.annotation.Bean', 'org.springframework.context.annotation.Configuration', 'redis.clients.jedis.JedisPool', 'java.io.File', 'java.util.List', 'java.util.stream.Collectors'] |
| Brief Description | This configuration class is used to initialize WeChat Mini Program services and message routers, supporting multi-application configuration and various message processing logic. |

# Description

This configuration class is used to initialize the WeChat Mini Program service and related message routing processing mechanisms. It completes the registration and management of multiple mini program accounts by reading configuration properties, and defines processors for various message types, including logging, text replies, image and QR code responses, and other functions, to implement automated message interaction logic.

# Class Summary

| Name   | Type  | Description |
|-------|------|-------------|
| WxMaConfiguration | class | This configuration class is used to initialize WeChat Mini Program services and message routers, supporting multi-mini-program configurations, and defines various message processing logic, including subscription messages, text, image, and QR code reply functions. |



## Class WxMaConfiguration

|      |      |
|------|------|
| Access Modifier | @Slf4j;@Configuration;@EnableConfigurationProperties(WxMaProperties.class);public |
| Type | class |
| Name | WxMaConfiguration |
| Description | This configuration class is used to initialize WeChat Mini Program services and message routers, supporting multi-mini-program configurations, and defines various message processing logic, including subscription messages, text, image, and QR code reply functions. |


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

    class WxMaDefaultConfigImpl {
        +void setAppid(String appid)
        +void setSecret(String secret)
        +void setToken(String token)
        +void setAesKey(String aesKey)
        +void setMsgDataFormat(String msgDataFormat)
        +String getAppid()
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

    class MsgData {
        +MsgData(String key, String value)
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

    class WxErrorException {
        +printStackTrace()
    }

    // Dependencies
    WxMaConfiguration --> WxMaProperties : depends on
    WxMaConfiguration --> WxMaService : creates
    WxMaConfiguration --> WxMaServiceImpl : instantiates
    WxMaConfiguration --> WxMaMessageRouter : creates and configures
    WxMaConfiguration --> WxMaDefaultConfigImpl : configures WeChat parameters
    WxMaServiceImpl ..|> WxMaService : implements
    WxMaMessageRouter --> WxMaMessageHandler : uses handlers
    WxMaConfiguration --> WxMaSubscribeMessage : builds subscription messages
    WxMaConfiguration --> WxMaKefuMessage : sends customer service messages
    WxMaConfiguration --> WxMediaUploadResult : gets media ID
    WxMaConfiguration --> WxErrorException : handles exceptions
```

This class diagram shows the structure of the WeChat Mini Program configuration class `WxMaConfiguration` and its relationships with other core components. It achieves automatic bean wiring and binding of configuration properties through Spring annotations, responsible for initializing WeChat service instances and message routing rules, and defines multiple message handlers to respond to different types of message requests.


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
    D --> "Create WxMaServiceImpl instance"
    D --> "Read configs and validate non-null"
    D --> "Build config map and set to maService"
    E --> "Create WxMaMessageRouter"
    E --> "Register logHandler rule"
    E --> "Register subscribeMsgHandler rule"
    E --> "Register textHandler rule"
    E --> "Register picHandler rule"
    E --> "Register qrcodeHandler rule"
    F --> "Send subscription message"
    G --> "Log and reply customer service message"
    H --> "Reply text message"
    I --> "Upload image and send"
    J --> "Generate QR code and send"
```

This flowchart illustrates the structure and core logic of the `WxMaConfiguration` class, including the initialization of WeChat Mini Program services, multi-configuration support, and various message routing mechanisms. Two core components, `WxMaService` and `WxMaMessageRouter`, are created using the @Bean annotation, and multiple message handlers are registered to respond to different types of message requests.

### Field List

| Name  | Type  | Description |
|-------|-------|------|
| properties | WxMaProperties | This is a private constant instance of the WeChat Mini Program configuration property class, used to store and manage the relevant configuration information of the WeChat Mini Program. |
| subscribeMsgHandler = (wxMessage, context, service, sessionManager) -> {        service.getMsgService().sendSubscribeMsg(WxMaSubscribeMessage.builder()            .templateId("此处更换为自己的模板id")            .data(Lists.newArrayList(                new WxMaSubscribeMessage.MsgData("keyword1", "339208499")))            .toUser(wxMessage.getFromUser())            .build());        return null;    } | WxMaMessageHandler | This is a WeChat Mini Program message handler used to process user subscription messages. When a user triggers a subscription event, the system automatically sends a subscription message containing the specified template ID and keyword data to the user. The handler constructs a subscription message object, sets the template ID, message data, and recipient user, and then calls the message service to send the message. |
| picHandler = (wxMessage, context, service, sessionManager) -> {        try {            WxMediaUploadResult uploadResult = service.getMediaService()                .uploadMedia("image", "png",                    ClassLoader.getSystemResourceAsStream("tmp.png"));            service.getMsgService().sendKefuMsg(                WxMaKefuMessage                    .newImageBuilder()                    .mediaId(uploadResult.getMediaId())                    .toUser(wxMessage.getFromUser())                    .build());        } catch (WxErrorException e) {            e.printStackTrace();        }        return null;    } | WxMaMessageHandler | This is a WeChat Mini Program image message processor, used to upload local image resources and send them to users. The processor obtains media upload results through services, then constructs image customer service messages to send to specified users, including exception handling mechanisms. |
| logHandler = (wxMessage, context, service, sessionManager) -> {        log.info("收到消息：" + wxMessage.toString());        service.getMsgService().sendKefuMsg(WxMaKefuMessage.newTextBuilder().content("收到信息为：" + wxMessage.toJson())            .toUser(wxMessage.getFromUser()).build());        return null;    } | WxMaMessageHandler | This is a WeChat Mini Program message processor, responsible for logging received messages and sending confirmation replies to users. |
| textHandler = (wxMessage, context, service, sessionManager) -> {        service.getMsgService().sendKefuMsg(WxMaKefuMessage.newTextBuilder().content("回复文本消息")            .toUser(wxMessage.getFromUser()).build());        return null;    } | WxMaMessageHandler | This is a WeChat Mini Program message handler, used to process text messages and automatically reply "Reply Text Message" to the sending user. |
| qrcodeHandler = (wxMessage, context, service, sessionManager) -> {        try {            final File file = service.getQrcodeService().createQrcode("123", 430);            WxMediaUploadResult uploadResult = service.getMediaService().uploadMedia("image", file);            service.getMsgService().sendKefuMsg(                WxMaKefuMessage                    .newImageBuilder()                    .mediaId(uploadResult.getMediaId())                    .toUser(wxMessage.getFromUser())                    .build());        } catch (WxErrorException e) {            e.printStackTrace();        }        return null;    } | WxMaMessageHandler | This code defines a WeChat Mini Program message handler used to generate parameterized QR codes and send them to users. Upon receiving a message, the handler first creates a QR code image with the content "123" and size 430, then uploads the image to the WeChat server to obtain a media ID, and finally sends the QR code image to the message-initiating user via the customer service message interface. The entire process includes exception handling mechanisms. |

### Method List

| Name  | Type  | Description |
|-------|-------|------|
| wxMaService | WxMaService | This code configures the WeChat Mini Program service, reads the configuration list, and initializes the WxMaService instance. It supports multiple mini program configurations, including necessary parameter settings such as appid, secret, and token. |
| wxMaMessageRouter | WxMaMessageRouter | This code configures a WeChat Mini Program message router, defining processing rules for various message types, including subscription messages, text, images, and QR codes. Each message type specifies a corresponding processor for asynchronous or synchronous handling. |




