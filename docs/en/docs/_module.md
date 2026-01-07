# Basic Information

|      |      |
|------|------|
| Name | weixin-java-miniapp-demo |
| Language | .java |
| Code Path | weixin-java-miniapp-demo |
| Brief Description | This module provides backend core services for WeChat Mini Programs, supporting multi-instance configuration, user authentication, message processing, and media resource management. Request isolation is achieved through AppId routing and thread-local variables, while integration with the WxJava SDK enables communication according to WeChat protocols. The interfaces follow RESTful style, supporting Multipart file transfer, JSON/XML parsing, and AES-encrypted communication. Key dependencies include wx-java-miniapp-spring-boot-starter, commons-fileupload, and Spring Web-related components. Core data structures encompass WxMaConfig, WxMaUserInfo, WxMaJscode2SessionResult, and WxMpXmlMessage, among others. The module employs the JsonUtils utility class for JSON serialization operations, utilizing Jackson's ObjectMapper to configure null-value ignoring and formatted output functionalities. Structured according to standard Spring Boot conventions, initialization is guided by the WxMaDemoApplication startup class. This module integrates three major interaction flows of WeChat Mini Programs: user login, message push, and material management. Its interaction pattern resembles an event bus architecture, with requests centrally dispatched by the Portal Controller. It supports a complete lifecycle from configuration loading to service operation, binding multi-instance parameters via WxMaProperties and using a message router to distribute different types of events to handlers such as logging, text replies, or image responses. Typical applications include scanning to return QR codes and triggering message pushes via subscription notifications. API types cover HTTP interfaces at the Controller layer, business logic at the Service layer, and custom message handler registration mechanisms, suitable for deployment in Spring Boot microservice environments. Additionally, it incorporates a unified error page mechanism to enhance frontend experience consistency, enabling developers to quickly reuse and build custom error prompt interfaces.``` |

# Module List

| Name   | Type  | Description |
|-------|------|-------------|
| [_module.md](src/main/java/com/_module.md) | folder | This module provides backend services for WeChat Mini Programs, supporting multi-instance configuration, user authentication, media management, and message push notifications. It adopts a RESTful interface design, integrates the WxJava SDK with the Spring Boot framework, implements functionalities such as file upload, JSON parsing, and AES encrypted communication, and enhances system stability through a unified error handling mechanism. |


