
# weixin-java-miniapp-demo

## 概述  
该工程是微信小程序后端综合系统，定位为"多租户网关中枢"，类似企业级API Gateway。核心解决小程序生态中多账号配置隔离、消息协议转换和错误统一处理三大问题，通过Spring Boot实现微服务化架构。系统以WxMaProperties为配置核心，结合微信SDK实现协议对接，关键特征包括：动态路由映射（类似Nginx的Location匹配）、多级错误拦截链（仿Spring MVC的HandlerInterceptor）和租户隔离策略（如数据库Schema分离）。  

技术实现上采用分层设计：基础设施层依赖Lombok简化POJO，业务层通过MsgRouter实现五种消息处理器（文本/图片/事件等），表现层由ErrorController统一渲染错误页。典型如通过Zookeeper动态更新AppID密钥库，或使用Redis缓存消息路由规则，体现配置中心与缓存的深度集成。  

## 什么是weixin-java-miniapp-demo?  
该系统是微信小程序多账号管理解决方案，类似银行的分支行柜台系统。核心模块包括：配置管理中心（类比K8s ConfigMap）、消息路由引擎（类似MQ的Topic交换机）和错误处理工厂（仿Struts2的全局异常拦截）。技术原理基于微信开放协议，通过Spring的@ConfigurationProperties绑定多组AppID/Secret，MsgRouter采用责任链模式匹配消息类型，ErrorPage注册表实现HTTP状态码与模板页的动态映射。  

典型应用场景涵盖电商小程序全流程：用户扫码登录（AES解密OpenID）→下单时触发订阅消息（通过Router分发至营销模块）→支付失败跳转自定义404页。具体实现案例包括：使用ConcurrentHashMap维护多租户配置隔离，通过微信MediaAPI实现图片CDN上传，以及基于JWT的会话维护机制。这些功能共同构成小程序后端"配置-路由-容错"三位一体的解决方案。

## 快速导航

### 👨‍💻 开发者

- [开发指南](docs/zh/summary/dev_guide.md) - 快速上手项目开发


- [模块说明](docs/zh/docs/_module.md) - 项目模块详细说明


### 👨‍💻 架构师

- [系统架构](docs/zh/summary/system_architecture.md) - 系统架构


### 📄 API 文档

- [API 文档](summary/api.md) - API 文档

