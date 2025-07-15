# Basic Information

|      |      |
|------|------|
| Name | WxMaConfiguration |
| Language | .java |
| Code Path | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/config/WxMaConfiguration.java |
| Package Name | com.github.binarywang.demo.wx.miniapp.config |
| Dependencies | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.api.impl.WxMaServiceImpl', 'cn.binarywang.wx.miniapp.bean.WxMaKefuMessage', 'cn.binarywang.wx.miniapp.bean.WxMaSubscribeMessage', 'cn.binarywang.wx.miniapp.config.impl.WxMaDefaultConfigImpl', 'cn.binarywang.wx.miniapp.config.impl.WxMaRedisConfigImpl', 'cn.binarywang.wx.miniapp.message.WxMaMessageHandler', 'cn.binarywang.wx.miniapp.message.WxMaMessageRouter', 'com.google.common.collect.Lists', 'lombok.extern.slf4j.Slf4j', 'me.chanjar.weixin.common.bean.result.WxMediaUploadResult', 'me.chanjar.weixin.common.error.WxErrorException', 'me.chanjar.weixin.common.error.WxRuntimeException', 'org.springframework.beans.factory.annotation.Autowired', 'org.springframework.boot.context.properties.EnableConfigurationProperties', 'org.springframework.context.annotation.Bean', 'org.springframework.context.annotation.Configuration', 'redis.clients.jedis.JedisPool', 'java.io.File', 'java.util.List', 'java.util.stream.Collectors'] |
| Brief Description | WeChat Mini Program configuration class, initializes services and message routing, handles subscription, text, image, and QR code messages. |

# Description

This is a configuration class for a WeChat Mini Program backend service, with main functionalities including initializing the WeChat Mini Program service and configuring message routing. The class injects configuration properties through its constructor and validates the configuration information when creating a WeChat Mini Program service instance, throwing an exception if any information is missing. The service supports multi-account configuration, with each account requiring parameters such as appid and secret key. The message routing section defines various message processing rules, including logic for handling logs, subscription messages, text, images, and QR code messages. Each processor responds to specific message types, such as sending customer service messages, uploading media files, or generating QR codes. Exception handling is implemented by printing stack traces.

# Class Summary

| Name   | Type  | Description |
|-------|------|-------------|
| WxMaConfiguration | class | This is a WeChat Mini Program configuration class that includes Bean definitions for WxMaService and WxMaMessageRouter, handling message types such as subscription messages, text, images, and QR codes. |



## Class WxMaConfiguration

|      |      |
|------|------|
| Access Modifier | @Slf4j;@Configuration;@EnableConfigurationProperties(WxMaProperties.class);public |
| Type | class |
| Name | WxMaConfiguration |
| Description | This is a WeChat Mini Program configuration class that includes Bean definitions for WxMaService and WxMaMessageRouter, handling message types such as subscription messages, text, images, and QR codes. |


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
        +List~Config~ getConfigs()
    }

    class WxMaProperties.Config {
        +String getAppid()
        +String getSecret()
        +String getToken()
        +String getAesKey()
        +String getMsgDataFormat()
    }

    class WxMaService {
        <<Interface>>
        +setMultiConfigs(Map~String, WxMaConfig~ configs)
    }

    class WxMaServiceImpl {
        +setMultiConfigs(Map~String, WxMaConfig~ configs)
    }

    class WxMaDefaultConfigImpl {
        +String getAppid()
        +void setAppid(String appid)
        +void setSecret(String secret)
        +void setToken(String token)
        +void setAesKey(String aesKey)
        +void setMsgDataFormat(String msgDataFormat)
    }

    class WxMaMessageRouter {
        +rule() WxMaMessageRouterRule
    }

    class WxMaMessageRouterRule {
        +handler(WxMaMessageHandler handler) WxMaMessageRouterRule
        +async(boolean async) WxMaMessageRouterRule
        +content(String content) WxMaMessageRouterRule
        +next() WxMaMessageRouterRule
        +end() WxMaMessageRouterRule
    }

    class WxMaMessageHandler {
        <<Interface>>
        +Object handle(WxMaMessage wxMessage, Map~String, Object~ context, WxMaService service, WxSessionManager sessionManager)
    }

    WxMaConfiguration --> WxMaProperties : depends
    WxMaConfiguration --> WxMaService : creates
    WxMaConfiguration --> WxMaMessageRouter : creates
    WxMaServiceImpl ..|> WxMaService : implements
    WxMaDefaultConfigImpl ..|> WxMaConfig : implements
    WxMaMessageRouter --> WxMaMessageRouterRule : contains
    WxMaMessageRouter --> WxMaMessageHandler : uses
    WxMaProperties --> WxMaProperties.Config : contains
```

This code represents a configuration class for a WeChat Mini Program backend service, whose main functions include initializing the WeChat Mini Program service (WxMaService) and message router (WxMaMessageRouter). WxMaConfiguration reads configuration information from WxMaProperties to create and configure WxMaService instances; it also constructs message routing rules to register corresponding handlers for different types of messages (subscription, text, image, QR code, etc.). The class diagram illustrates the dependency relationships between the configuration class and various components, including core modules such as property configuration, service implementation, message processing, and routing rules.


### Internal Method Call Graph

```mermaid
graph TD
    A["Class WxMaConfiguration"]
    B["Property: WxMaProperties properties"]
    C["Constructor: WxMaConfiguration(WxMaProperties properties)"]
    D["Bean Method: WxMaService wxMaService()"]
    E["Bean Method: WxMaMessageRouter wxMaMessageRouter(WxMaService wxMaService)"]
    F["Private Handler: subscribeMsgHandler"]
    G["Private Handler: logHandler"]
    H["Private Handler: textHandler"]
    I["Private Handler: picHandler"]
    J["Private Handler: qrcodeHandler"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F
    A --> G
    A --> H
    A --> I
    A --> J

    D --> D1["Check non-null configs"]
    D --> D2["Create WxMaServiceImpl instance"]
    D --> D3["Streaming config mapping"]
    D3 --> D3a["Set appid/secret parameters"]
    D --> D4["Return maService"]

    E --> E1["Create message router"]
    E1 --> E1a["Add logHandler rule"]
    E1a --> E1b["Add subscribeMsgHandler rule"]
    E1b --> E1c["Add textHandler rule"]
    E1c --> E1d["Add picHandler rule"]
    E1d --> E1e["Add qrcodeHandler rule"]
    E --> E2["Return router"]
```

This flowchart illustrates the core structure of the WeChat Mini Program configuration class WxMaConfiguration. The class injects configuration properties through its constructor and provides two key Beans: wxMaService initializes multi-account configuration services, including parameter validation and streaming configuration mapping; wxMaMessageRouter constructs a message routing rule chain with five message handlers. Each handler implements specific business logic such as logging, subscription message replies, and media file uploads. Arrows clearly indicate the hierarchical call relationships from the class to methods and then to internal processing steps.

### Field List

| Name  | Type  | Description |
|-------|-------|------|
| properties | WxMaProperties | Private immutable WeChat Mini Program configuration property object. |
| picHandler = (wxMessage, context, service, sessionManager) -> {        try {            WxMediaUploadResult uploadResult = service.getMediaService()                .uploadMedia("image", "png",                    ClassLoader.getSystemResourceAsStream("tmp.png"));            service.getMsgService().sendKefuMsg(                WxMaKefuMessage                    .newImageBuilder()                    .mediaId(uploadResult.getMediaId())                    .toUser(wxMessage.getFromUser())                    .build());        } catch (WxErrorException e) {            e.printStackTrace();        }        return null;    } | WxMaMessageHandler | Define a WeChat Mini Program image message handler to upload temporary images and send customer service messages, printing errors when exceptions occur. |
| logHandler = (wxMessage, context, service, sessionManager) -> {        log.info("收到消息：" + wxMessage.toString());        service.getMsgService().sendKefuMsg(WxMaKefuMessage.newTextBuilder().content("收到信息为：" + wxMessage.toJson())            .toUser(wxMessage.getFromUser()).build());        return null;    } | WxMaMessageHandler | Define the WeChat Mini Program message handling logic: Record received messages and automatically reply with the user's message content. |
| subscribeMsgHandler = (wxMessage, context, service, sessionManager) -> {        service.getMsgService().sendSubscribeMsg(WxMaSubscribeMessage.builder()            .templateId("此处更换为自己的模板id")            .data(Lists.newArrayList(                new WxMaSubscribeMessage.MsgData("keyword1", "339208499")))            .toUser(wxMessage.getFromUser())            .build());        return null;    } | WxMaMessageHandler | Define a WeChat Mini Program subscription message handler, which sends subscription messages using specified template IDs and user data, with the recipient being the message sender. |
| textHandler = (wxMessage, context, service, sessionManager) -> {        service.getMsgService().sendKefuMsg(WxMaKefuMessage.newTextBuilder().content("回复文本消息")            .toUser(wxMessage.getFromUser()).build());        return null;    } | WxMaMessageHandler | WeChat Mini Program text message handling: Automatically replies with "Reply to text message" upon receiving a user message. |
| qrcodeHandler = (wxMessage, context, service, sessionManager) -> {        try {            final File file = service.getQrcodeService().createQrcode("123", 430);            WxMediaUploadResult uploadResult = service.getMediaService().uploadMedia("image", file);            service.getMsgService().sendKefuMsg(                WxMaKefuMessage                    .newImageBuilder()                    .mediaId(uploadResult.getMediaId())                    .toUser(wxMessage.getFromUser())                    .build());        } catch (WxErrorException e) {            e.printStackTrace();        }        return null;    } | WxMaMessageHandler | Processing WeChat Mini Program messages, generating QR codes and uploading them as images, then sending them to users via customer service messages. Print errors when exceptions occur. |

### Method List

| Name  | Type  | Description |
|-------|-------|------|
| wxMaService | WxMaService | Create a WeChat Mini Program service instance, initialize multi-account configuration after checking the settings, and return the service object. |
| wxMaMessageRouter | WxMaMessageRouter | Define the message routing rules for WeChat Mini Programs, including logic for handling logs, subscription messages, text, images, and QR codes. |




