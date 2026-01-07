# 基础信息

|      |      |
|------|------|
| 名称 | WxMaMediaController |
| 编码语言 | .java |
| 代码路径 | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/controller/WxMaMediaController.java |
| 包名 | com.github.binarywang.demo.wx.miniapp.controller |
| 依赖项 | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.constant.WxMaConstants', 'cn.binarywang.wx.miniapp.util.WxMaConfigHolder', 'com.google.common.collect.Lists', 'com.google.common.io.Files', 'lombok.AllArgsConstructor', 'lombok.extern.slf4j.Slf4j', 'me.chanjar.weixin.common.bean.result.WxMediaUploadResult', 'me.chanjar.weixin.common.error.WxErrorException', 'org.springframework.web.bind.annotation', 'org.springframework.web.multipart.MultipartFile', 'org.springframework.web.multipart.MultipartHttpServletRequest', 'org.springframework.web.multipart.commons.CommonsMultipartResolver', 'javax.servlet.http.HttpServletRequest', 'java.io.File', 'java.io.IOException', 'java.util.Iterator', 'java.util.List'] |
| 概述说明 | 该控制器提供微信小程序临时素材的上传与下载功能，支持通过appid切换配置，上传接口返回media_id列表，下载接口根据media_id获取文件。 |

# 说明

该控制器提供微信小程序媒体文件的上传与下载功能。通过指定appid路由请求，支持上传临时图片素材并返回对应的media_id列表，同时提供根据mediaId下载临时素材的功能。上传时使用multipart格式接收文件，并借助CommonsMultipartResolver处理请求。每次操作完成后会清理线程本地变量以确保环境干净。若未找到对应appid配置则抛出异常提示。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| WxMaMediaController | class | 该控制器提供微信小程序临时素材的上传与下载功能，支持通过appid切换配置，上传接口返回media_id列表，下载接口根据media_id获取文件。 |



## 类 WxMaMediaController

|      |      |
|------|------|
| 访问范围 | @RestController;@AllArgsConstructor;@Slf4j;@RequestMapping("/wx/media/{appid}");public |
| 类型 | class |
| 名称 | WxMaMediaController |
| 说明 | 该控制器提供微信小程序临时素材的上传与下载功能，支持通过appid切换配置，上传接口返回media_id列表，下载接口根据media_id获取文件。 |


### UML类图

```mermaid
classDiagram
    class WxMaMediaController {
        -WxMaService wxMaService
        +List~String~ uploadMedia(String appid, HttpServletRequest request) throws WxErrorException
        +File getMedia(String appid, String mediaId) throws WxErrorException
    }

    class WxMaService {
        <<Interface>>
        +boolean switchover(String appid)
        +WxMediaService getMediaService()
    }

    class WxMediaService {
        <<Interface>>
        +WxMediaUploadResult uploadMedia(String type, File file) throws WxErrorException
        +File getMedia(String mediaId) throws WxErrorException
    }

    class WxMaConstants {
        <<Interface>>
        +String KefuMsgType.IMAGE
    }

    class WxMaConfigHolder {
        <<Interface>>
        +void remove()
    }

    class CommonsMultipartResolver {
        +boolean isMultipart(HttpServletRequest request)
    }

    class MultipartHttpServletRequest {
        +Iterator~String~ getFileNames()
        +MultipartFile getFile(String name)
    }

    class MultipartFile {
        +String getOriginalFilename()
        +void transferTo(File dest) throws IOException, IllegalStateException
    }

    class File {
        +File(File parent, String child)
    }

    class Lists {
        <<Interface>>
        +ArrayList~E~ newArrayList()
    }

    class HttpServletRequest {
    }

    class WxErrorException {
    }

    class IOException {
    }

    class Logger {
        +void info(String msg)
        +void error(String msg, Throwable t)
    }

    WxMaMediaController --> WxMaService : 依赖
    WxMaMediaController --> CommonsMultipartResolver : 使用
    WxMaMediaController --> MultipartHttpServletRequest : 强转使用
    WxMaMediaController --> MultipartFile : 获取文件
    WxMaMediaController --> File : 创建临时文件
    WxMaMediaController --> Lists : 创建列表
    WxMaMediaController --> WxMaConstants : 使用常量
    WxMaMediaController --> WxMaConfigHolder : 清理配置
    WxMaMediaController --> Logger : 日志记录
    WxMaService --> WxMediaService : 获取服务
```

该类图展示了微信小程序媒体控制器 `WxMaMediaController` 的结构及其相关依赖关系。它通过 Spring 注解实现 REST 接口，负责处理临时素材的上传与下载逻辑，并依赖于微信服务接口完成核心操作。同时涉及多部件请求解析、文件操作及异常处理等辅助功能模块。


### 内部方法调用关系图

```mermaid
graph TD
    A["WxMaMediaController类"]
    B["属性: WxMaService wxMaService"]
    C["uploadMedia方法"]
    D["getMedia方法"]
    E["switchover切换appid"]
    F["resolver.isMultipart判断是否为multipart请求"]
    G["遍历上传文件并保存到临时目录"]
    H["调用微信接口上传素材"]
    I["记录日志与异常处理"]
    J["返回media_id列表"]
    K["下载临时素材文件"]
    L["返回File对象"]
    M["清理ThreadLocal"]

    A --> B
    A --> C
    A --> D
    C --> E
    C --> F
    F -- 是 --> G
    G --> H
    H --> I
    I --> J
    F -- 否 --> M
    C --> M
    D --> E
    D --> K
    K --> L
    D --> M
```

该流程图展示了微信小程序媒体控制器中两个主要接口的执行逻辑：一个是处理上传临时素材的 `uploadMedia` 方法，另一个是用于下载临时素材的 `getMedia` 方法。整个过程包括了请求前置校验、文件处理、调用微信服务以及资源清理等关键步骤。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| wxMaService | WxMaService | 这是一个微信小程序服务接口的私有常量字段声明，用于在类中提供微信小程序相关功能调用。 |

### 方法列表

| 名称  | 类型  | 说明 |
|-------|-------|------|
| uploadMedia | List<String> | 该接口处理微信小程序媒体文件上传，支持多文件同时上传，返回媒体ID列表。首先验证appid配置，检查请求是否包含文件，遍历并保存每个文件到临时目录，调用微信API上传图片获取media_id，最后清理线程本地变量。若无文件则返回空列表。 |
| getMedia | File | 该接口用于下载微信媒体文件。通过appid和mediaId获取对应媒体文件，若appid配置不存在则抛出异常，成功获取后清理线程本地变量并返回文件。 |




