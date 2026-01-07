# 基础信息

|      |      |
|------|------|
| 名称 | WxMaProperties |
| 编码语言 | .java |
| 代码路径 | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/config/WxMaProperties.java |
| 包名 | com.github.binarywang.demo.wx.miniapp.config |
| 依赖项 | ['java.util.List', 'org.springframework.boot.context.properties.ConfigurationProperties', 'lombok.Data'] |
| 概述说明 | 该类用于配置微信小程序相关参数，包含应用ID、密钥、消息服务器令牌和加密密钥等信息。 |

# 说明

该类是微信小程序配置属性类，用于存储和管理微信小程序的相关配置信息。类中包含一个配置列表，每个配置项包括小程序的appid、secret密钥、消息服务器token、加密密钥aesKey以及消息数据格式等核心参数，支持多小程序配置管理。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| WxMaProperties | class | 该类用于配置微信小程序相关参数，包含应用ID、密钥、消息服务器令牌和加密密钥等信息。 |



## 类 WxMaProperties

|      |      |
|------|------|
| 访问范围 | @Data;@ConfigurationProperties(prefix = "wx.miniapp");public |
| 类型 | class |
| 名称 | WxMaProperties |
| 说明 | 该类用于配置微信小程序相关参数，包含应用ID、密钥、消息服务器令牌和加密密钥等信息。 |


### UML类图

```mermaid
classDiagram
    class WxMaProperties {
        -List~Config~ configs
        +List~Config~ getConfigs()
        +void setConfigs(List~Config~ configs)
    }

    class Config {
        -String appid
        -String secret
        -String token
        -String aesKey
        -String msgDataFormat
        +String getAppid()
        +void setAppid(String appid)
        +String getSecret()
        +void setSecret(String secret)
        +String getToken()
        +void setToken(String token)
        +String getAesKey()
        +void setAesKey(String aesKey)
        +String getMsgDataFormat()
        +void setMsgDataFormat(String msgDataFormat)
    }

    WxMaProperties --> "包含" Config : 依赖
```

该类图描述了微信小程序配置属性的结构。`WxMaProperties` 类用于封装多个 `Config` 配置项，每个 `Config` 对应一个小程序的认证与消息配置信息，支持通过 `@ConfigurationProperties` 进行批量注入。两者之间为聚合关系，表示 `WxMaProperties` 包含多个 `Config` 实例。


### 内部方法调用关系图

```mermaid
graph TD
    A["类WxMaProperties"]
    B["注解: @Data"]
    C["注解: @ConfigurationProperties(prefix = 'wx.miniapp')"]
    D["属性: List<Config> configs"]
    E["内部类Config"]
    F["注解: @Data"]
    G["属性: String appid"]
    H["属性: String secret"]
    I["属性: String token"]
    J["属性: String aesKey"]
    K["属性: String msgDataFormat"]

    A --> B
    A --> C
    A --> D
    A --> E
    E --> F
    E --> G
    E --> H
    E --> I
    E --> J
    E --> K
```

该流程图展示了`WxMaProperties`配置类的结构，包括其绑定前缀为"wx.miniapp"的注解配置、包含的`List<Config>`属性以及内部静态类`Config`的各个字段定义。整体体现了微信小程序多配置项的组织方式。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| configs | List<Config> | 这是一个私有配置列表字段，用于存储Config类型的配置对象集合。 |

### 方法列表

| 名称  | 类型  | 说明 |
|-------|-------|------|




