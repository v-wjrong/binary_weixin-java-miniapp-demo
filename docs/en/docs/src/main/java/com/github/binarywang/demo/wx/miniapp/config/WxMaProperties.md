# Basic Information

|      |      |
|------|------|
| Name | WxMaProperties |
| Language | .java |
| Code Path | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/config/WxMaProperties.java |
| Package Name | com.github.binarywang.demo.wx.miniapp.config |
| Dependencies | ['java.util.List', 'org.springframework.boot.context.properties.ConfigurationProperties', 'lombok.Data'] |
| Brief Description | This is a WeChat Mini Program configuration property class that contains configuration information for multiple mini programs. Each configuration item has parameters such as appid, secret, token, aesKey, and message format. |

# Description

This class is a property configuration class used to configure WeChat Mini Program related parameters. Through the @ConfigurationProperties annotation, it specifies the configuration prefix as "wx.miniapp" and supports multiple sets of configurations. It defines a Config static inner class internally, containing five core configuration items: appid, secret, token, aesKey, and msgDataFormat, which correspond to key information such as the application identifier, secret key, message server token, encryption key, and message data format for WeChat Mini Programs.

# Class Summary

| Name   | Type  | Description |
|-------|------|-------------|
| WxMaProperties | class | This class is used to configure WeChat Mini Program related parameters, including application ID, secret key, message server token, and encryption key information. |



## Class WxMaProperties

|      |      |
|------|------|
| Access Modifier | @Data;@ConfigurationProperties(prefix = "wx.miniapp");public |
| Type | class |
| Name | WxMaProperties |
| Description | This class is used to configure WeChat Mini Program related parameters, including application ID, secret key, message server token, and encryption key information. |


### UML Class Diagram

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

    WxMaProperties --> "contains" Config : dependency
```

This class diagram describes the structure of WeChat Mini Program configuration properties. The `WxMaProperties` class is used to encapsulate multiple `Config` configuration items, each `Config` corresponding to an authentication and message processing parameters of a mini program, such as appid, secret, etc. The relationship between the two is aggregation, indicating that `WxMaProperties` contains multiple `Config` instances.


### Internal Method Call Graph

```mermaid
graph TD
    A["Class WxMaProperties"]
    B["Annotation: @Data"]
    C["Annotation: @ConfigurationProperties(prefix = 'wx.miniapp')"]
    D["Property: List<Config> configs"]
    E["Inner Class Config"]
    F["Property: String appid"]
    G["Property: String secret"]
    H["Property: String token"]
    I["Property: String aesKey"]
    J["Property: String msgDataFormat"]

    A --> B
    A --> C
    A --> D
    A --> E
    E --> F
    E --> G
    E --> H
    E --> I
    E --> J
```

This flowchart illustrates the structure of the `WxMaProperties` configuration class, including its annotation configuration with binding prefix "wx.miniapp", the contained `List<Config>` property, and the various field definitions of the inner static class `Config`. The overall structure is used for automatic assembly mapping of WeChat Mini Program related configurations.

### Field List

| Name  | Type  | Description |
|-------|-------|------|
| configs | List<Config> | This is a private configuration list member variable used to store a collection of configuration objects of type Config. |

### Method List

| Name  | Type  | Description |
|-------|-------|------|




