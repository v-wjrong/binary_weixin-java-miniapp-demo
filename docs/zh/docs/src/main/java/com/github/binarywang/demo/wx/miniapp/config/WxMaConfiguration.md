# 基础信息

|      |      |
|------|------|
| 名称 | WxMaConfiguration |
| 编码语言 | .java |
| 代码路径 | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/config/WxMaConfiguration.java |
| 包名 | com.github.binarywang.demo.wx.miniapp.config |
| 依赖项 | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.api.impl.WxMaServiceImpl', 'cn.binarywang.wx.miniapp.bean.WxMaKefuMessage', 'cn.binarywang.wx.miniapp.bean.WxMaSubscribeMessage', 'cn.binarywang.wx.miniapp.config.impl.WxMaDefaultConfigImpl', 'cn.binarywang.wx.miniapp.config.impl.WxMaRedisConfigImpl', 'cn.binarywang.wx.miniapp.message.WxMaMessageHandler', 'cn.binarywang.wx.miniapp.message.WxMaMessageRouter', 'com.google.common.collect.Lists', 'lombok.extern.slf4j.Slf4j', 'me.chanjar.weixin.common.bean.result.WxMediaUploadResult', 'me.chanjar.weixin.common.error.WxErrorException', 'me.chanjar.weixin.common.error.WxRuntimeException', 'org.springframework.beans.factory.annotation.Autowired', 'org.springframework.boot.context.properties.EnableConfigurationProperties', 'org.springframework.context.annotation.Bean', 'org.springframework.context.annotation.Configuration', 'redis.clients.jedis.JedisPool', 'java.io.File', 'java.util.List', 'java.util.stream.Collectors'] |
| 概述说明 | 该类为微信小程序配置类，初始化服务和消息路由器，处理订阅消息、文本、图片及二维码等类型的消息，并支持发送客服消息与上传媒体文件。 |

# 说明

该配置类用于初始化微信小程序服务及相关消息路由处理机制，通过读取配置属性完成多小程序账号的接入设置，并定义了多种消息类型的处理器，包括日志记录、文本回复、图片及二维码响应等功能，同时支持发送客服消息与订阅消息通知用户。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| WxMaConfiguration | class | 该配置类用于初始化微信小程序服务及消息路由器，支持多小程序配置，并定义了多种消息处理逻辑，包括日志记录、文本回复、图片发送和二维码生成等功能。 |



## 类 WxMaConfiguration

|      |      |
|------|------|
| 访问范围 | @Slf4j;@Configuration;@EnableConfigurationProperties(WxMaProperties.class);public |
| 类型 | class |
| 名称 | WxMaConfiguration |
| 说明 | 该配置类用于初始化微信小程序服务及消息路由器，支持多小程序配置，并定义了多种消息处理逻辑，包括日志记录、文本回复、图片发送和二维码生成等功能。 |


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
    WxMaConfiguration --> WxMaProperties : 依赖
    WxMaConfiguration --> WxMaService : 创建并配置
    WxMaConfiguration --> WxMaMessageRouter : 创建并配置路由规则
    WxMaConfiguration --> WxMaMessageHandler : 定义多个处理器实例
    WxMaServiceImpl ..|> WxMaService : 实现
    WxMaMessageRouter --> WxMaService : 路由处理需服务支持
    WxMaMessageRouter --> WxMaMessageHandler : 注册消息处理器
    WxMaMessageHandler --> WxMaMessage : 处理微信消息对象
    WxMaMessageHandler --> WxSessionManager : 使用会话管理器
    WxMaMessageHandler --> WxMaSubscribeMessage : 构建订阅消息
    WxMaMessageHandler --> WxMaKefuMessage : 构建客服消息
    WxMaMessageHandler --> WxMediaUploadResult : 获取媒体上传结果
    WxMaMessageHandler --> WxMaMsgService : 发送消息服务调用
    WxMaMessageHandler --> WxMaMediaService : 媒体上传操作
    WxMaMessageHandler --> WxMaQrcodeService : 二维码生成服务
    WxMaMessageHandler --> WxRuntimeException : 异常处理
    WxMaMessageHandler --> Lists : 数据构建辅助类
    WxMaMessageHandler --> WxErrorException : 微信异常捕获
```

该类图展示了微信小程序后端服务配置中心 `WxMaConfiguration` 的结构及其与其他核心组件的关系。它负责初始化微信服务、注册多账号配置以及定义多种类型的消息处理器，如文本、图片、二维码等，并通过路由器分发处理逻辑。整个系统围绕着 `WxMaService` 和 `WxMaMessageHandler` 展开，体现了高度模块化的设计思想。


### 内部方法调用关系图

```mermaid
graph TD
    A["类WxMaConfiguration"]
    B["属性: WxMaProperties properties"]
    C["构造方法: WxMaConfiguration(WxMaProperties properties)"]
    D["Bean方法: wxMaService()"]
    E["Bean方法: wxMaMessageRouter(WxMaService wxMaService)"]
    F["subscribeMsgHandler处理器"]
    G["logHandler处理器"]
    H["textHandler处理器"]
    I["picHandler处理器"]
    J["qrcodeHandler处理器"]

    A --> B
    A --> C
    A --> D
    A --> E
    D --> "创建WxMaServiceImpl实例"
    D --> "设置多配置setMultiConfigs"
    D --> "返回maService"
    E --> "创建WxMaMessageRouter"
    E --> "注册logHandler规则"
    E --> "注册subscribeMsgHandler规则"
    E --> "注册textHandler规则"
    E --> "注册picHandler规则"
    E --> "注册qrcodeHandler规则"
    E --> "返回router"

    F -.-> "发送订阅消息"
    G -.-> "记录日志并回复客服消息"
    H -.-> "回复文本消息"
    I -.-> "上传图片并发送"
    J -.-> "生成二维码并发送"
```

该流程图展示了微信小程序配置类 `WxMaConfiguration` 的结构与核心逻辑。主要包括两个 Bean 的初始化过程：`wxMaService` 负责加载多账号配置，`wxMaMessageRouter` 用于注册不同类型的消息处理规则。每个消息处理器实现特定功能，如发送文本、图片或二维码等，并通过服务接口完成交互。整体设计支持扩展性和模块化处理。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| properties | WxMaProperties | 这是一个微信小程序配置属性的私有常量字段，用于存储不可变的微信小程序相关配置信息。 |
| subscribeMsgHandler = (wxMessage, context, service, sessionManager) -> {        service.getMsgService().sendSubscribeMsg(WxMaSubscribeMessage.builder()            .templateId("此处更换为自己的模板id")            .data(Lists.newArrayList(                new WxMaSubscribeMessage.MsgData("keyword1", "339208499")))            .toUser(wxMessage.getFromUser())            .build());        return null;    } | WxMaMessageHandler | 这是一个微信小程序消息处理器，用于处理用户订阅消息。当用户触发订阅事件时，系统会自动发送一条包含指定模板ID和数据的订阅消息给用户。处理器通过构建订阅消息对象，设置模板ID、消息数据和接收用户，然后调用消息服务发送消息。 |
| textHandler = (wxMessage, context, service, sessionManager) -> {        service.getMsgService().sendKefuMsg(WxMaKefuMessage.newTextBuilder().content("回复文本消息")            .toUser(wxMessage.getFromUser()).build());        return null;    } | WxMaMessageHandler | 这是一个微信小程序消息处理器，用于处理文本消息并自动回复用户。 |
| picHandler = (wxMessage, context, service, sessionManager) -> {        try {            WxMediaUploadResult uploadResult = service.getMediaService()                .uploadMedia("image", "png",                    ClassLoader.getSystemResourceAsStream("tmp.png"));            service.getMsgService().sendKefuMsg(                WxMaKefuMessage                    .newImageBuilder()                    .mediaId(uploadResult.getMediaId())                    .toUser(wxMessage.getFromUser())                    .build());        } catch (WxErrorException e) {            e.printStackTrace();        }        return null;    } | WxMaMessageHandler | 这是一个微信小程序图片消息处理器，用于上传本地图片资源并发送给用户。处理器通过服务获取媒体上传结果，然后构建图片客服消息发送给指定用户，包含异常处理机制。 |
| logHandler = (wxMessage, context, service, sessionManager) -> {        log.info("收到消息：" + wxMessage.toString());        service.getMsgService().sendKefuMsg(WxMaKefuMessage.newTextBuilder().content("收到信息为：" + wxMessage.toJson())            .toUser(wxMessage.getFromUser()).build());        return null;    } | WxMaMessageHandler | 这是一个微信小程序消息处理器，负责记录收到的消息日志，并向用户发送确认回复。 |
| qrcodeHandler = (wxMessage, context, service, sessionManager) -> {        try {            final File file = service.getQrcodeService().createQrcode("123", 430);            WxMediaUploadResult uploadResult = service.getMediaService().uploadMedia("image", file);            service.getMsgService().sendKefuMsg(                WxMaKefuMessage                    .newImageBuilder()                    .mediaId(uploadResult.getMediaId())                    .toUser(wxMessage.getFromUser())                    .build());        } catch (WxErrorException e) {            e.printStackTrace();        }        return null;    } | WxMaMessageHandler | 该代码定义了一个微信小程序消息处理器，用于生成带参数二维码并发送给用户。处理器接收消息后，调用服务创建二维码图片，上传至微信服务器获得媒体ID，最后通过客服消息接口将二维码图片发送给指定用户。整个过程包含异常处理机制。 |

### 方法列表

| 名称  | 类型  | 说明 |
|-------|-------|------|
| wxMaService | WxMaService | 该代码配置微信小程序服务，读取配置列表并初始化WxMaService实例，支持多小程序配置，包含appid、secret、token等核心参数设置。 |
| wxMaMessageRouter | WxMaMessageRouter | 该代码配置了一个微信小程序消息路由器，定义了多种消息类型的处理规则，包括订阅消息、文本、图片和二维码处理，并设置为同步执行。 |




