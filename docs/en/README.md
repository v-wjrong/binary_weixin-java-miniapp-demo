
# weixin-java-miniapp-demo

## Overview  
This project is a comprehensive backend system for WeChat Mini Programs, positioned as a "multi-tenant gateway hub," similar to an enterprise-level API Gateway. It primarily addresses three core challenges in the Mini Program ecosystem: multi-account configuration isolation, message protocol conversion, and unified error handling, implemented via a microservices architecture using Spring Boot. Centered around WxMaProperties for configuration, the system integrates with the WeChat SDK for protocol compatibility. Key features include dynamic route mapping (similar to Nginx's Location matching), multi-level error interception chains (inspired by Spring MVC's HandlerInterceptor), and tenant isolation strategies (e.g., database schema separation).  

The technical implementation adopts a layered design: the infrastructure layer leverages Lombok to simplify POJOs, the business layer employs MsgRouter to handle five types of message processors (text/image/event, etc.), and the presentation layer uses ErrorController to uniformly render error pages. Notable examples include dynamically updating AppID keystores via Zookeeper or caching message routing rules with Redis, showcasing deep integration with configuration centers and caching.  

## What is weixin-java-miniapp-demo?  
This system is a multi-account management solution for WeChat Mini Programs, akin to a bank's branch-counter system. Core modules include a configuration management center (analogous to K8s ConfigMap), a message routing engine (similar to MQ's Topic exchange), and an error handling factory (inspired by Struts2's global exception interception). The technical foundation relies on WeChat's open protocols, binding multiple sets of AppID/Secret via Spring's @ConfigurationProperties. MsgRouter adopts the chain-of-responsibility pattern to match message types, while the ErrorPage registry dynamically maps HTTP status codes to template pages.  

Typical use cases cover the entire e-commerce Mini Program lifecycle: user scan-to-login (AES decryption of OpenID) → order-triggered subscription messages (routed to marketing modules via Router) → payment failure redirects to custom 404 pages. Implementation examples include using ConcurrentHashMap for multi-tenant configuration isolation, leveraging WeChat MediaAPI for image CDN uploads, and JWT-based session maintenance. Together, these features form a "configuration-routing-fault tolerance" triad for Mini Program backends.

## Quick Navigation

### 👨‍💻 Developers

- [Development Guide](summary/dev_guide.md) - Quickly get started with project development


- [Module Description](docs/_module.md) - Detailed explanation of project modules


### 👨‍💻 Architect

- [System Architecture](summary/system_architecture.md) - System Architecture


### 📄 API Documentation

- [API Documentation](summary/api.md) - API Documentation

