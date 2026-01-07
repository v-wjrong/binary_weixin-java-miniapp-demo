# 基础信息

|      |      |
|------|------|
| 名称 | WxMaUserController |
| 编码语言 | .java |
| 代码路径 | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/controller/WxMaUserController.java |
| 包名 | com.github.binarywang.demo.wx.miniapp.controller |
| 依赖项 | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.bean.WxMaJscode2SessionResult', 'cn.binarywang.wx.miniapp.bean.WxMaPhoneNumberInfo', 'cn.binarywang.wx.miniapp.bean.WxMaUserInfo', 'cn.binarywang.wx.miniapp.util.WxMaConfigHolder', 'com.github.binarywang.demo.wx.miniapp.utils.JsonUtils', 'lombok.AllArgsConstructor', 'lombok.extern.slf4j.Slf4j', 'me.chanjar.weixin.common.error.WxErrorException', 'org.apache.commons.lang3.StringUtils', 'org.springframework.web.bind.annotation.GetMapping', 'org.springframework.web.bind.annotation.PathVariable', 'org.springframework.web.bind.annotation.RequestMapping', 'org.springframework.web.bind.annotation.RestController'] |
| 概述说明 | 该控制器提供微信小程序用户登录、信息获取及手机号解密功能，通过appid切换配置并处理会话验证与数据解密。 |

# 说明

该控制器提供微信小程序用户相关接口，包括登录、获取用户信息及绑定手机号。通过appID切换配置，处理JSCode登录凭证校验与会话管理，支持用户数据解密和验证签名，确保安全访问并返回JSON格式结果。所有操作完成后清理线程本地变量。

# 类列表 Class Summary

| 名称   | 类型  | 说明 |
|-------|------|-------------|
| WxMaUserController | class | 该控制器提供微信小程序用户登录、获取用户信息及绑定手机号功能，通过appid切换配置并处理会话验证与数据解密。 |



## 类 WxMaUserController

|      |      |
|------|------|
| 访问范围 | @RestController;@AllArgsConstructor;@Slf4j;@RequestMapping("/wx/user/{appid}");public |
| 类型 | class |
| 名称 | WxMaUserController |
| 说明 | 该控制器提供微信小程序用户登录、获取用户信息及绑定手机号功能，通过appid切换配置并处理会话验证与数据解密。 |


### UML类图

```mermaid
classDiagram

    class WxMaUserController {
        -WxMaService wxMaService
        +String login(String appid, String code)
        +String info(String appid, String sessionKey, String signature, String rawData, String encryptedData, String iv)
        +String phone(String appid, String sessionKey, String signature, String rawData, String encryptedData, String iv)
    }

    class WxMaService {
        <<Interface>>
        +boolean switchover(String appid)
        +WxMaUserService getUserService()
    }

    class WxMaUserService {
        <<Interface>>
        +WxMaJscode2SessionResult getSessionInfo(String jsCode) throws WxErrorException
        +boolean checkUserInfo(String sessionKey, String rawData, String signature)
        +WxMaUserInfo getUserInfo(String sessionKey, String encryptedData, String iv)
        +WxMaPhoneNumberInfo getPhoneNoInfo(String sessionKey, String encryptedData, String iv)
    }

    class WxMaConfigHolder {
        <<Utility Class>>
        +void remove()
    }

    class JsonUtils {
        <<Utility Class>>
        +String toJson(Object object)
    }

    class WxMaJscode2SessionResult {
        +String getSessionKey()
        +String getOpenid()
    }

    class WxMaUserInfo {
    }

    class WxMaPhoneNumberInfo {
    }

    class WxErrorException {
    }

    class StringUtils {
        <<Utility Class>>
        +boolean isBlank(CharSequence cs)
    }

    class RequestMapping {
    }

    class GetMapping {
    }

    class PathVariable {
    }

    class RestController {
    }

    class AllArgsConstructor {
    }

    class Slf4j {
    }


    // Dependencies
    WxMaUserController --> WxMaService : 依赖
    WxMaUserController --> WxMaConfigHolder : 调用remove()
    WxMaUserController --> JsonUtils : 序列化结果
    WxMaUserController --> StringUtils : 参数校验
    WxMaUserController --> WxMaJscode2SessionResult : 使用返回值
    WxMaUserController --> WxMaUserInfo : 返回用户信息
    WxMaUserController --> WxMaPhoneNumberInfo : 返回手机号信息
    WxMaUserController --> WxErrorException : 异常处理
    WxMaService --> WxMaUserService : 获取服务
```

该类图展示了微信小程序用户相关接口的控制器类 `WxMaUserController` 及其依赖的服务接口与工具类。控制器通过 `WxMaService` 切换配置并调用用户服务完成登录、获取用户信息及手机号等操作，同时结合日志记录、参数校验和异常处理机制保障接口稳定运行。


### 内部方法调用关系图

```mermaid
graph TD
    A["类WxMaUserController"]
    B["属性: WxMaService wxMaService"]
    C["方法: login(String appid, String code)"]
    D["方法: info(String appid, String sessionKey, String signature, String rawData, String encryptedData, String iv)"]
    E["方法: phone(String appid, String sessionKey, String signature, String rawData, String encryptedData, String iv)"]

    A --> B
    A --> C
    A --> D
    A --> E

    subgraph login流程
        C1["参数校验: code是否为空"]
        C2["切换appid配置: wxMaService.switchover(appid)"]
        C3["获取session信息: wxMaService.getUserService().getSessionInfo(code)"]
        C4["记录日志: log.info(sessionKey 和 openid)"]
        C5["返回session的JSON"]
        C6["捕获异常: WxErrorException"]
        C7["finally清理ThreadLocal"]

        C --> C1
        C1 --"code为空"--> R1["返回 'empty jscode'"]
        C1 --"code非空"--> C2
        C2 --"失败"--> E1["抛出IllegalArgumentException"]
        C2 --"成功"--> C3
        C3 --> C4
        C4 --> C5
        C3 --"异常"--> C6
        C6 --> R2["返回e.toString()"]
        C5 & R2 --> C7
    end

    subgraph info流程
        D1["切换appid配置: wxMaService.switchover(appid)"]
        D2["校验用户信息: wxMaService.getUserService().checkUserInfo(...)"]
        D3["解密用户信息: wxMaService.getUserService().getUserInfo(...)"]
        D4["返回userInfo的JSON"]
        D5["清理ThreadLocal"]

        D --> D1
        D1 --"失败"--> E2["抛出IllegalArgumentException"]
        D1 --"成功"--> D2
        D2 --"失败"--> R3["返回 'user check failed'"] --> D5
        D2 --"成功"--> D3
        D3 --> D4
        D4 --> D5
    end

    subgraph phone流程
        E1["切换appid配置: wxMaService.switchover(appid)"]
        E2["校验用户信息: wxMaService.getUserService().checkUserInfo(...)"]
        E3["解密手机号: wxMaService.getUserService().getPhoneNoInfo(...)"]
        E4["返回phoneNoInfo的JSON"]
        E5["清理ThreadLocal"]

        E --> E1
        E1 --"失败"--> E3_1["抛出IllegalArgumentException"]
        E1 --"成功"--> E2
        E2 --"失败"--> R4["返回 'user check failed'"] --> E5
        E2 --"成功"--> E3
        E3 --> E4
        E4 --> E5
    end
```

该流程图展示了微信小程序用户控制器（`WxMaUserController`）中三个核心接口的处理流程：登录（`login`）、获取用户信息（`info`）和获取手机号（`phone`）。每个接口均包含参数校验、配置切换、服务调用及异常处理等关键步骤，并在最后统一清理线程上下文，保障系统运行稳定性和安全性。

### 字段列表 Field List

| 名称  | 类型  | 说明 |
|-------|-------|------|
| wxMaService | WxMaService | 这是一个微信小程序服务接口的私有不可变实例变量，用于处理微信小程序相关业务逻辑。 |

### 方法列表

| 名称  | 类型  | 说明 |
|-------|-------|------|
| login | String | 该接口处理微信小程序登录请求，通过appid和code获取用户会话信息。首先校验code是否为空，然后切换到对应的微信配置，调用服务获取sessionKey和openid并记录日志，最后返回JSON格式的会话信息或错误信息，执行完毕后清理线程本地变量。 |
| info | String | 该接口用于获取微信小程序用户信息，通过appid切换配置并校验用户签名，最终返回解密后的用户数据。 |
| phone | String | 该接口用于获取用户手机号，通过校验用户信息并解密加密数据来实现。首先切换到指定appid配置，校验用户信息合法性，若校验失败则返回错误信息；校验成功后解密手机号相关信息并以JSON格式返回结果，最后清理线程本地变量。 |




