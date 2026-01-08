# 基础信息

|      |      |
|------|------|
| 名称 | WxMaConfiguration |
| 编码语言 | .java |
| 代码路径 | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/config/WxMaConfiguration.java |
| 包名 | com.github.binarywang.demo.wx.miniapp.config |
| 依赖项 | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.api.impl.WxMaServiceImpl', 'cn.binarywang.wx.miniapp.bean.WxMaKefuMessage', 'cn.binarywang.wx.miniapp.bean.WxMaSubscribeMessage', 'cn.binarywang.wx.miniapp.config.impl.WxMaDefaultConfigImpl', 'cn.binarywang.wx.miniapp.config.impl.WxMaRedisConfigImpl', 'cn.binarywang.wx.miniapp.message.WxMaMessageHandler', 'cn.binarywang.wx.miniapp.message.WxMaMessageRouter', 'com.google.common.collect.Lists', 'lombok.extern.slf4j.Slf4j', 'me.chanjar.weixin.common.bean.result.WxMediaUploadResult', 'me.chanjar.weixin.common.error.WxErrorException', 'me.chanjar.weixin.common.error.WxRuntimeException', 'org.springframework.beans.factory.annotation.Autowired', 'org.springframework.boot.context.properties.EnableConfigurationProperties', 'org.springframework.context.annotation.Bean', 'org.springframework.context.annotation.Configuration', 'redis.clients.jedis.JedisPool', 'java.io.File', 'java.util.List', 'java.util.stream.Collectors'] |
| 概述说明 | 该配置类用于初始化微信小程序服务及消息路由器，支持多应用配置和多种消息处理逻辑。 |

# 说明

该配置类用于初始化微信小程序服务及相关消息路由处理机制，通过读取配置属性完成多小程序账号的注册与管理，并定义了多种消息类型的处理器，包括日志记录、文本回复、图片及二维码响应等功能，以实现自动化的消息交互逻辑。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| WxMaConfiguration | class | 该配置类用于初始化微信小程序服务及消息路由器，支持多小程序配置，并定义了多种消息处理逻辑，包括订阅消息、文本、图片和二维码回复等功能。 |



## 类 WxMaConfiguration

|      |      |
|------|------|
| 访问范围 | @Slf4j;@Configuration;@EnableConfigurationProperties(WxMaProperties.class);public |
| 类型 | class |
| 名称 | WxMaConfiguration |
| 说明 | 该配置类用于初始化微信小程序服务及消息路由器，支持多小程序配置，并定义了多种消息处理逻辑，包括订阅消息、文本、图片和二维码回复等功能。 |


### UML类图

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
    WxMaConfiguration --> WxMaProperties : 依赖
    WxMaConfiguration --> WxMaService : 创建
    WxMaConfiguration --> WxMaServiceImpl : 实例化
    WxMaConfiguration --> WxMaMessageRouter : 创建并配置
    WxMaConfiguration --> WxMaDefaultConfigImpl : 配置微信参数
    WxMaServiceImpl ..|> WxMaService : 实现
    WxMaMessageRouter --> WxMaMessageHandler : 使用处理器
    WxMaConfiguration --> WxMaSubscribeMessage : 构建订阅消息
    WxMaConfiguration --> WxMaKefuMessage : 发送客服消息
    WxMaConfiguration --> WxMediaUploadResult : 获取媒体ID
    WxMaConfiguration --> WxErrorException : 异常处理
```

该类图展示了微信小程序配置类 `WxMaConfiguration` 的结构及其与其他核心组件的关系。它通过 Spring 注解实现 Bean 的自动装配与配置属性绑定，负责初始化微信服务实例及消息路由规则，并定义了多种消息处理器以响应不同类型的消息请求。


### 内部方法调用关系图

```mermaid
graph TD
    A["类WxMaConfiguration"]
    B["属性: WxMaProperties properties"]
    C["构造方法: WxMaConfiguration(WxMaProperties properties)"]
    D["Bean方法: wxMaService()"]
    E["Bean方法: wxMaMessageRouter(WxMaService wxMaService)"]
    F["消息处理器: subscribeMsgHandler"]
    G["消息处理器: logHandler"]
    H["消息处理器: textHandler"]
    I["消息处理器: picHandler"]
    J["消息处理器: qrcodeHandler"]

    A --> B
    A --> C
    A --> D
    A --> E
    D --> "创建WxMaServiceImpl实例"
    D --> "读取configs并校验非空"
    D --> "构建config映射并设置到maService"
    E --> "创建WxMaMessageRouter"
    E --> "注册logHandler规则"
    E --> "注册subscribeMsgHandler规则"
    E --> "注册textHandler规则"
    E --> "注册picHandler规则"
    E --> "注册qrcodeHandler规则"
    F --> "发送订阅消息"
    G --> "记录日志并回复客服消息"
    H --> "回复文本消息"
    I --> "上传图片并发送"
    J --> "生成二维码并发送"
```

该流程图展示了`WxMaConfiguration`类的结构与核心逻辑，包括微信小程序服务的初始化、多配置支持以及多种消息路由处理机制。通过@Bean注解创建了`WxMaService`和`WxMaMessageRouter`两个核心组件，并注册了多个消息处理器用于响应不同类型的消息请求。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| properties | WxMaProperties | 这是一个微信小程序配置属性类的私有常量实例，用于存储和管理微信小程序的相关配置信息。 |
| subscribeMsgHandler = (wxMessage, context, service, sessionManager) -> {        service.getMsgService().sendSubscribeMsg(WxMaSubscribeMessage.builder()            .templateId("此处更换为自己的模板id")            .data(Lists.newArrayList(                new WxMaSubscribeMessage.MsgData("keyword1", "339208499")))            .toUser(wxMessage.getFromUser())            .build());        return null;    } | WxMaMessageHandler | 这是一个微信小程序消息处理器，用于处理用户订阅消息。当用户触发订阅事件时，系统会自动发送一条包含指定模板ID和关键词数据的订阅消息给用户。处理器通过构建订阅消息对象，设置模板ID、消息数据和接收用户，然后调用消息服务发送消息。 |
| picHandler = (wxMessage, context, service, sessionManager) -> {        try {            WxMediaUploadResult uploadResult = service.getMediaService()                .uploadMedia("image", "png",                    ClassLoader.getSystemResourceAsStream("tmp.png"));            service.getMsgService().sendKefuMsg(                WxMaKefuMessage                    .newImageBuilder()                    .mediaId(uploadResult.getMediaId())                    .toUser(wxMessage.getFromUser())                    .build());        } catch (WxErrorException e) {            e.printStackTrace();        }        return null;    } | WxMaMessageHandler | 这是一个微信小程序图片消息处理器，用于上传本地图片资源并发送给用户。处理器通过服务获取媒体上传结果，然后构建图片客服消息发送给指定用户，包含异常处理机制。 |
| logHandler = (wxMessage, context, service, sessionManager) -> {        log.info("收到消息：" + wxMessage.toString());        service.getMsgService().sendKefuMsg(WxMaKefuMessage.newTextBuilder().content("收到信息为：" + wxMessage.toJson())            .toUser(wxMessage.getFromUser()).build());        return null;    } | WxMaMessageHandler | 这是一个微信小程序消息处理器，负责记录收到的消息日志，并向用户发送确认回复。 |
| textHandler = (wxMessage, context, service, sessionManager) -> {        service.getMsgService().sendKefuMsg(WxMaKefuMessage.newTextBuilder().content("回复文本消息")            .toUser(wxMessage.getFromUser()).build());        return null;    } | WxMaMessageHandler | 这是一个微信小程序消息处理器，用于处理文本消息并自动回复"回复文本消息"给发送方用户。 |
| qrcodeHandler = (wxMessage, context, service, sessionManager) -> {        try {            final File file = service.getQrcodeService().createQrcode("123", 430);            WxMediaUploadResult uploadResult = service.getMediaService().uploadMedia("image", file);            service.getMsgService().sendKefuMsg(                WxMaKefuMessage                    .newImageBuilder()                    .mediaId(uploadResult.getMediaId())                    .toUser(wxMessage.getFromUser())                    .build());        } catch (WxErrorException e) {            e.printStackTrace();        }        return null;    } | WxMaMessageHandler | 该代码定义了一个微信小程序消息处理器，用于生成带参数二维码并发送给用户。处理器接收消息后，首先创建内容为"123"、尺寸430的二维码图片，然后将图片上传至微信服务器获得媒体ID，最后通过客服消息接口将二维码图片发送给消息发起用户。整个过程包含异常处理机制。 |

### 方法列表

| 名称  | 类型  | 说明 |
|-------|-------|------|
| wxMaService | WxMaService | 该代码配置微信小程序服务，读取配置列表并初始化WxMaService实例，支持多小程序配置，包含appid、secret、token等必要参数设置。 |
| wxMaMessageRouter | WxMaMessageRouter | 该代码配置了一个微信小程序消息路由器，定义了多种消息类型的处理规则，包括订阅消息、文本、图片和二维码等，每种消息类型都指定了相应的处理器进行异步或同步处理。 |




