# Basic Information

|      |      |
|------|------|
| Name | WxMaUserController |
| Language | .java |
| Code Path | weixin-java-miniapp-demo/src/main/java/com/github/binarywang/demo/wx/miniapp/controller/WxMaUserController.java |
| Package Name | com.github.binarywang.demo.wx.miniapp.controller |
| Dependencies | ['cn.binarywang.wx.miniapp.api.WxMaService', 'cn.binarywang.wx.miniapp.bean.WxMaJscode2SessionResult', 'cn.binarywang.wx.miniapp.bean.WxMaPhoneNumberInfo', 'cn.binarywang.wx.miniapp.bean.WxMaUserInfo', 'cn.binarywang.wx.miniapp.util.WxMaConfigHolder', 'com.github.binarywang.demo.wx.miniapp.utils.JsonUtils', 'lombok.AllArgsConstructor', 'lombok.extern.slf4j.Slf4j', 'me.chanjar.weixin.common.error.WxErrorException', 'org.apache.commons.lang3.StringUtils', 'org.springframework.web.bind.annotation.GetMapping', 'org.springframework.web.bind.annotation.PathVariable', 'org.springframework.web.bind.annotation.RequestMapping', 'org.springframework.web.bind.annotation.RestController'] |
| Brief Description | This class is a controller for WeChat Mini Program user-related interfaces, providing login, user information retrieval, and phone number binding functions. It switches configurations through appid and processes data returned by WeChat. |

# Description

This controller provides WeChat Mini Program user-related interfaces, including login, obtaining user information, and binding mobile phone numbers. It switches configurations through appid, processes JSCode login and returns session information, supports user data decryption and verification. All operations are logged and thread local variables are cleaned up upon completion.

# Class Summary

| Name   | Type  | Description |
|-------|------|-------------|
| WxMaUserController | class | This controller provides WeChat Mini Program user login, information retrieval, and phone number decryption functions. It switches configurations through appid and handles session validation and data decryption. |



## Class WxMaUserController

|      |      |
|------|------|
| Access Modifier | @RestController;@AllArgsConstructor;@Slf4j;@RequestMapping("/wx/user/{appid}");public |
| Type | class |
| Name | WxMaUserController |
| Description | This controller provides WeChat Mini Program user login, information retrieval, and phone number decryption functions. It switches configurations through appid and handles session validation and data decryption. |


### UML Class Diagram

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
        +WxMaJscode2SessionResult getSessionInfo(String jsCode)
        +boolean checkUserInfo(String sessionKey, String rawData, String signature)
        +WxMaUserInfo getUserInfo(String sessionKey, String encryptedData, String iv)
        +WxMaPhoneNumberInfo getPhoneNoInfo(String sessionKey, String encryptedData, String iv)
    }

    class WxMaJscode2SessionResult {
        +String getSessionKey()
        +String getOpenid()
    }

    class WxMaUserInfo {
    }

    class WxMaPhoneNumberInfo {
    }

    class WxMaConfigHolder {
        +void remove()
    }

    class JsonUtils {
        +String toJson(Object object)
    }

    class StringUtils {
        +boolean isBlank(String str)
    }

    class WxErrorException {
    }

    class log {
        +void info(String msg)
        +void error(String msg, Throwable t)
    }

    // Dependencies
    WxMaUserController --> WxMaService : depends on
    WxMaUserController --> WxMaConfigHolder : clean ThreadLocal
    WxMaUserController --> JsonUtils : serialize response
    WxMaUserController --> StringUtils : parameter validation
    WxMaUserController --> WxErrorException : exception handling
    WxMaUserController --> log : logging
    WxMaService --> WxMaUserService : get user service
    WxMaUserService --> WxMaJscode2SessionResult : login session info
    WxMaUserService --> WxMaUserInfo : decrypt user info
    WxMaUserService --> WxMaPhoneNumberInfo : decrypt phone number info
```

This class diagram shows the controller `WxMaUserController` for WeChat Mini Program user-related interfaces, along with its dependent service interfaces and utility classes. The controller switches configurations via `WxMaService` and obtains user services to implement functions such as login, retrieving user information, and phone number, while also involving auxiliary operations like parameter validation, logging, and exception handling. Each component has clearly defined responsibilities and explicit dependency relationships.


### Internal Method Call Graph

```mermaid
graph TD
    A["Class WxMaUserController"]
    B["Property: WxMaService wxMaService"]
    C["Method: login(String appid, String code)"]
    D["Method: info(String appid, String sessionKey, String signature, String rawData, String encryptedData, String iv)"]
    E["Method: phone(String appid, String sessionKey, String signature, String rawData, String encryptedData, String iv)"]

    A --> B
    A --> C
    A --> D
    A --> E

    subgraph login method flow
        C1["Parameter validation: whether code is empty"]
        C2["Switch appid configuration: wxMaService.switchover(appid)"]
        C3["Get session information: wxMaService.getUserService().getSessionInfo(code)"]
        C4["Log record: log.info(sessionKey and openid)"]
        C5["Return session JSON"]
        C6["Catch exception: WxErrorException"]
        C7["Clean up ThreadLocal: WxMaConfigHolder.remove()"]
        
        C --> C1
        C1 -- "Empty" --> R1["Return 'empty jscode'"]
        C1 -- "Not empty" --> C2
        C2 -- "Failed" --> E1["Throw IllegalArgumentException"]
        C2 -- "Success" --> C3
        C3 --> C4
        C4 --> C5
        C3 -- Exception --> C6
        C6 --> R2["Return exception toString()"]
        C5 --> C7
        C6 --> C7
    end

    subgraph info method flow
        D1["Switch appid configuration: wxMaService.switchover(appid)"]
        D2["Validate user info: wxMaService.getUserService().checkUserInfo(...)"]
        D3["Decrypt user info: wxMaService.getUserService().getUserInfo(...)"]
        D4["Return userInfo JSON"]
        D5["Clean up ThreadLocal: WxMaConfigHolder.remove()"]

        D --> D1
        D1 -- "Failed" --> E2["Throw IllegalArgumentException"]
        D1 -- "Success" --> D2
        D2 -- "Failed" --> R3["Return 'user check failed'"] --> D5
        D2 -- "Success" --> D3
        D3 --> D4
        D4 --> D5
    end

    subgraph phone method flow
        E1["Switch appid configuration: wxMaService.switchover(appid)"]
        E2["Validate user info: wxMaService.getUserService().checkUserInfo(...)"]
        E3["Decrypt phone number: wxMaService.getUserService().getPhoneNoInfo(...)"]
        E4["Return phoneNoInfo JSON"]
        E5["Clean up ThreadLocal: WxMaConfigHolder.remove()"]

        E --> E1
        E1 -- "Failed" --> E3_1["Throw IllegalArgumentException"]
        E1 -- "Success" --> E2
        E2 -- "Failed" --> R4["Return 'user check failed'"] --> E5
        E2 -- "Success" --> E3
        E3 --> E4
        E4 --> E5
    end
```

This flowchart illustrates the execution flows of three core interfaces in the WeChat Mini Program user controller (`WxMaUserController`): the login interface (`login`), the user information retrieval interface (`info`), and the phone number retrieval interface (`phone`). Each interface includes key steps such as parameter validation, configuration switching, service invocation, and result processing, followed by unified resource cleanup at the end.

### Field List

| Name  | Type  | Description |
|-------|-------|------|
| wxMaService | WxMaService | This is a private constant field declaration for a WeChat Mini Program service interface, used to provide function calls related to WeChat Mini Programs. |

### Method List

| Name  | Type  | Description |
|-------|-------|------|
| info | String | This interface is used to obtain WeChat Mini Program user information. It switches configurations through the appid and validates the user session. If validation fails, it returns error information; if successful, it decrypts and returns the user information. |
| login | String | This interface handles WeChat Mini Program login requests, obtaining user session information through appid and code. First, it validates whether the code is empty, then switches to the corresponding WeChat configuration, calls the service to obtain sessionKey and openid, and finally returns JSON formatted session information or error information. |
| phone | String | This interface is used to obtain the user's phone number by verifying user information and decrypting encrypted data. First, switch to the specified appid configuration and verify the legitimacy of the user information. If the verification fails, clear the ThreadLocal and return the error message; if the verification succeeds, decrypt the phone number information and return the result in JSON format, finally clearing the ThreadLocal. |




