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
- **Build Tool Version:** Maven 3.6+ (Recommended based on Spring Boot 2.6.3)
- **Network-connected Middleware Dependencies:**
  - Redis (Inferred via the `jedis` client dependency)

## Build Guide
### Maven Build
- Build Commands:
    - Clean build: `mvn clean`
    - Compile project: `mvn compile`
    - Package project: `mvn package`
    - Install to local repository: `mvn install`
    - Deploy project: `mvn deploy`
- Build Process: 
    The Maven build process follows the standard lifecycle, mainly including clean, compile, test, package, install, and deploy. In this project, the Spring Boot plugin is used for packaging, and Docker image building is supported through the docker-maven-plugin.
- Packaging Directory: 
    The packaged JAR file is located in the `target/` directory with the filename `${project.build.finalName}.jar`. Docker-related resources are placed in the `src/main/docker` directory, where the generated JAR will be copied into the Docker context path during image construction.

## Dependency Management
### Main Dependencies
- **Spring Boot Web Starter**: Provides support for web application development, including embedded Tomcat and Spring MVC.
- **Spring Boot Thymeleaf Starter**: Supports rendering HTML pages using the Thymeleaf template engine.
- **weixin-java-miniapp**: WeChat Mini Program Java SDK for interacting with WeChat Mini Program APIs; its version is controlled by property `${weixin-java-miniapp.version}`, currently set to `4.7.0`.
- **commons-fileupload**: Apache Commons file upload component, version `1.5`, used for handling file uploading functionality.
- **Spring Boot Test Starter**: Used for testing Spring Boot applications, scope is `test`.
- **Spring Boot Configuration Processor**: Assists in generating configuration metadata, marked as an optional dependency.
- **Lombok**: Simplifies Java code writing (e.g., auto-generating getters/setters), scope is `provided`.
- **Jedis**: Redis client library for operating Redis databases.

### Adding/Modifying Dependencies
- **Maven:** Add or modify `<dependency>` elements within the `<dependencies>` tag in the `pom.xml` file.

### Dependency Version Management
- **Maven (`<dependencyManagement>`):** This project inherits from `spring-boot-starter-parent`, which defines versions for many commonly-used dependencies, so some dependencies do not require explicit version numbers. For dependencies not managed by the parent POM (such as `weixin-java-miniapp` and `commons-fileupload`), versions must be specified manually. Versions can also be centrally declared in `<dependencyManagement>` for unified management.

## Project Structure

```text
weixin-java-miniapp-demo/
├── .git/                    # Git version control directory
├── .github/                 # GitHub-related configurations (e.g., FUNDING.yml)
├── src/
│   └── main/
│       ├── java/
│       │   └── com/
│       │       └── github/
│       │           └── binarywang/
│       │               └── demo/
│       │                   └── wx/
│       │                       └── miniapp/
│       │                           ├── WxMaDemoApplication.java  # Spring Boot startup class
│       │                           ├── config/                  # Configuration classes directory
│       │                           │   ├── WxMaProperties.java  # WeChat MiniApp properties configuration
│       │                           │   └── WxMaConfiguration.java  # WeChat MiniApp configuration initialization
│       │                           ├── controller/              # Controller layer (handles HTTP requests)
│       │                           │   ├── WxMaMediaController.java
│       │                           │   ├── WxMaUserController.java
│       │                           │   └── WxPortalController.java
│       │                           ├── error/                   # Error handling module
│       │                           │   ├── ErrorController.java
│       │                           │   └── ErrorPageConfiguration.java
│       │                           └── utils/                   # Utility classes directory
│       │                               └── JsonUtils.java
│       ├── resources/
│       │   ├── application.yml.template  # Application configuration template file
│       │   ├── tmp.png                  # Sample image resource (assumed)
│       │   ├── templates/               # Page templates directory (presumably used by Thymeleaf, etc.)
│       │   │   └── error.html           # Custom error page
│       │   └── META-INF/
│       │       └── additional-spring-configuration-metadata.json  # Spring configuration metadata extension
│       └── docker/
│           └── Dockerfile               # Docker image build script
├── .editorconfig            # Editor encoding style uniform configuration
├── .gitignore               # Git ignored files list
├── README.md                # Project documentation
├── .travis.yml              # Travis CI continuous integration configuration
├── pom.xml                  # Maven Project Object Model file
└── commit.sh                # Commit helper script (assumed)
```

Naming Convention: Java package names use reverse domain name structure (e.g., com.github.binarywang), with directories corresponding to code module functionalities  
Layered Structure: Based on standard Spring Boot layered architecture, including controller, config, utils, error modules  
Extended Design: Supports containerized deployment through the docker directory; supports static resources and template rendering under resources; provides configuration meta-information extension capabilities via META-INF