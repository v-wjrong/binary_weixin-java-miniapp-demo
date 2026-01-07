[Go to Homepage](../README.md)

## Project Overview
### Basic Project Information
- **Name:** spring-boot-demo-for-wechat-miniapp
- **GroupId (Maven):** com.github.binarywang
- **ArtifactId (Maven):** weixin-java-miniapp-demo
- **Version:** 1.0.0-SNAPSHOT
- **Primary Programming Language:** Java

## Prerequisites
- **JDK Version:** 1.8
- **Build Tool Version:** Maven (It is recommended to use a Maven version compatible with Spring Boot 2.6.3, typically Maven 3.6+)
- **Network-connected Middleware Dependencies:**
  - Redis (inferred through the `jedis` client dependency)

## Build Guide
### Maven Build
- Build Commands:
    - Clean build: `mvn clean`
    - Compile project: `mvn compile`
    - Package project: `mvn package`
    - Install to local repository: `mvn install`
    - Deploy project: `mvn deploy`
- Build Process: 
    The Maven build process follows the standard lifecycle, mainly including phases such as clean, compile, test, package, install, and deploy. This project is a Spring Boot application that uses the `spring-boot-maven-plugin` plugin to support packaging into an executable JAR.
- Packaging Directory: 
    The generated JAR file after packaging is located in the `target/` directory, with the filename `${project.build.finalName}.jar`, which defaults to `weixin-java-miniapp-demo-1.0.0-SNAPSHOT.jar`. Docker image-related resources are configured in the `src/main/docker` directory.

## Dependency Management
### Main Dependencies
- **Spring Boot Web Starter**: Provides support for web application development, including embedded Tomcat and Spring MVC.
- **Spring Boot Thymeleaf Starter**: Supports rendering HTML pages using the Thymeleaf template engine.
- **weixin-java-miniapp**: WeChat Mini Program Java SDK used for interacting with WeChat APIs, version controlled by property `${weixin-java-miniapp.version}`, actually `4.7.0`.
- **commons-fileupload**: Apache Commons file upload component, version `1.5`, used for handling file upload functionality.
- **Spring Boot Test Starter**: Testing framework support provided by Spring Boot, scope is `test`.
- **Spring Boot Configuration Processor**: Used for processing configuration metadata, marked as optional dependency.
- **Lombok**: A utility library to simplify Java code, such as auto-generating getters/setters, scope is `provided`.
- **Jedis**: Redis client library used for operating on Redis databases.

### Adding/Modifying Dependencies
- **Maven:** Add or modify `<dependency>` elements within the `<dependencies>` tag in the `pom.xml` file.

### Dependency Version Management
- **Maven (`<dependencyManagement>`):** This project inherits from `spring-boot-starter-parent`, which already includes version management for most official Spring Boot dependencies. For dependencies not managed by Spring Boot (such as `weixin-java-miniapp`, `commons-fileupload`), versions are explicitly declared via `<version>` or controlled uniformly using variables defined in `<properties>`.

## Project Structure

```text
weixin-java-miniapp-demo/
├── .git/                    # Git version control directory
├── .github/                 # GitHub-related configurations (e.g., FUNDING.yml)
├── src/
│   └── main/
│       ├── java/            # Java source code directory
│       │   └── com/
│       │       └── github/
│       │           └── binarywang/
│       │               └── demo/
│       │                   └── wx/
│       │                       └── miniapp/              # Main package for WeChat MiniApp demo project
│       │                           ├── WxMaDemoApplication.java  # Spring Boot startup class
│       │                           ├── controller/       # Controller layer (handles HTTP requests)
│       │                           ├── utils/            # Utility classes (e.g., JsonUtils)
│       │                           ├── error/            # Error handling module
│       │                           └── config/           # Configuration classes (e.g., WeChat configuration, property binding)
│       ├── resources/        # Resource files directory
│       │   ├── application.yml.template  # Application configuration template file
│       │   ├── tmp.png       # Sample image resource (assumed)
│       │   ├── templates/    # Template pages directory (e.g., Thymeleaf)
│       │   │   └── error.html  # Custom error page
│       │   └── META-INF/     # Meta-information directory
│       │       └── additional-spring-configuration-metadata.json  # Spring configuration metadata
│       └── docker/           # Docker build-related files
│           └── Dockerfile    # Docker image build script
├── .editorconfig             # Editor formatting uniformity configuration
├── .gitignore                # Git ignored files list
├── README.md                 # Project documentation
├── .travis.yml               # Travis CI continuous integration configuration
├── pom.xml                   # Maven Project Object Model file
└── commit.sh                 # Commit helper script (assumed)
```

Naming Convention: Uses Maven standard directory structure; Java package names follow reverse domain name + functional module division  
Layered Structure: Based on typical Spring Boot layers including controller, config, utils, and error modules, reflecting the principle of separation of concerns  
Extensibility Design: Supports containerized deployment via the docker directory; supports view rendering through resources/templates; enhances Spring configuration capabilities via META-INF