
# weixin-java-miniapp-demo

## Overview

This project is a backend service framework for WeChat Mini Programs, positioned as a lightweight and extensible access center for mini programs. It addresses core interaction issues in the WeChat ecosystem such as user authentication, message processing, and resource management. The framework supports multi-instance deployment with good isolation and stability. Its architecture is designed in the style of Spring Boot microservices, integrating the WxJava SDK to achieve efficient docking with WeChat's open protocols, similar to how distributed gateways forward requests and route them to different business units.

The module identifies different mini program instances through AppId and achieves context isolation using ThreadLocal to ensure concurrent safety. Core functionalities include login credential validation (exchanging code for session), message subscription and callback, as well as uploading and downloading media files such as images and audio. Additionally, the system has a built-in unified exception handling mechanism that centralizes error requests to the /error path, presenting friendly prompt pages via the Thymeleaf template engine, thereby improving development debugging efficiency and consistency of user experience.

## What is weixin-java-miniapp-demo?

This project is a backend demonstration engineering project for WeChat Mini Programs based on the WxJava open-source SDK, integrating multiple functional modules including user login, message receiving and event distribution, and material upload/download. Components work together coordinated by the Spring container, forming clear service invocation chains, similar to how message middleware forwards events to designated handlers by type. For example, when a text message is received, it will automatically match to TextMsgHandler for response construction.

Technically, it exposes interfaces using RESTful API style, supporting JSON/XML data exchange and AES encrypted communication to ensure transmission security. Through wx-java-miniapp-spring-boot-starter, the configuration process is simplified, allowing developers to enable corresponding instances by simply declaring WxMaConfig. Typical application scenarios include order notification push in e-commerce mini programs, message subscription confirmation in educational platforms, etc., featuring high reusability and adaptability for secondary development.

## Quick Navigation

### 💻 Developers

- [Development Guide](summary/dev_guide.md) - Quickly get started with project development


- [Module Description](docs/_module.md) - Detailed explanation of project modules


### 🏗️ Architect

- [System Architecture](summary/system_architecture.md) - System Architecture


### ↔️ API Documentation

- <a href='http://52.237.88.89/wiki/binary/weixin-java-miniapp-demo/acb95743d3a86fe1e043ca6537768e9719883ee0/api-viewer.html' target='_blank'>API Documentation</a> - API Documentation

