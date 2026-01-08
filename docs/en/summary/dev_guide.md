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
- **Build Tool Version:** Maven 3.6+ (standard requirement based on spring-boot-starter-parent)
- **Network-connected Middleware Dependencies:**
  - Redis (inferred through jedis client dependency)

## Build Guide
### Maven Build
- Build Commands:
    - Clean build: `mvn clean`
    - Compile project: `mvn compile`
    - Package project: `mvn package`
    - Install to local repository: `mvn install`
    - Deploy project: `mvn deploy`
- Build Process: 
    The Maven build process follows the standard lifecycle, mainly including: clean, compile, test, package, verify, install, and deploy. This project uses the Spring Boot plugin for packaging executable JARs, and configures a Docker plugin for container image building.
- Packaging Directory: 
    After packaging, the main artifact is an executable JAR file located in the `target/` directory, with a filename similar to `weixin-java-miniapp-demo-1.0.0-SNAPSHOT.jar`. Docker-related resources are located in the `src/main/docker` directory, where the JAR file will be copied into the Docker context path during the build process for image creation.

## Dependency Management
### Main Dependencies
- **Spring Boot Web Starter**: Provides foundational support for web application development (embedded Tomcat, Spring MVC, etc.).
- **Spring Boot Thymeleaf Starter**: Supports page rendering using the Thymeleaf template engine.
- **weixin-java-miniapp**: WeChat Mini Program Java SDK used for interacting with WeChat servers (version specified by property `${weixin-java-miniapp.version}` as `4.7.0`).
- **commons-fileupload**: Handles file upload functionality (version `1.5`).
- **Spring Boot Test Starter**: Provides testing support (scope: `test`).
- **Spring Boot Configuration Processor**: Used for processing configuration metadata (optional dependency).
- **Lombok**: Simplifies Java code writing (e.g., auto-generating getters/setters; scope: `provided`).
- **Jedis**: Redis client library used for operating the Redis database.

### Adding/Modifying Dependencies
- **Maven:** Add or modify `<dependency>` elements within the `<dependencies>` tag of the `pom.xml` file.

### Dependency Version Management
- **Maven (`<dependencyManagement>`):** This project inherits from `spring-boot-starter-parent`, which already defines versions for many commonly used dependencies. Therefore, most Spring Boot-related dependencies do not require explicit version declarations. For non-Spring Boot managed dependencies (such as `commons-fileupload` and `weixin-java-miniapp`), version numbers must be manually specified, or uniformly managed under `<dependencyManagement>` to ensure consistency.

## Project Structure

```text
weixin-java-miniapp-demo/
├── .git/                    # Git version control directory
├── .github/                 # GitHub-related configurations (e.g., FUNDING.yml)
│   └── FUNDING.yml          # Project funding information file
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
│       │                           │   ├── WxMaProperties.java  # WeChat MiniApp properties configuration class
│       │                           │   └── WxMaConfiguration.java  # WeChat MiniApp configuration initialization class
│       │                           ├── controller/              # Controller layer (handles HTTP requests)
│       │                           │   ├── WxMaMediaController.java
│       │                           │   ├── WxMaUserController.java
│       │                           │   └── WxPortalController.java
│       │                           ├── error/                   # Error handling module
│       │                           │   ├── ErrorController.java
│       │                           │   └── ErrorPageConfiguration.java
│       │                           └── utils/                   # Utility classes directory
│       │                               └── JsonUtils.java       # JSON utility class
│       ├── resources/
│       │   ├── application.yml.template  # Application configuration template file
│       │   ├── tmp.png                  # Sample image resource (assumed)
│       │   ├── templates/               # Page templates directory (presumably for Thymeleaf, etc.)
│       │   │   └── error.html           # Custom error page
│       │   └── META-INF/
│       │       └── additional-spring-configuration-metadata.json  # Spring configuration metadata extension
│       └── docker/
│           └── Dockerfile               # Docker image build script
├── .editorconfig            # Editor encoding style uniform configuration
├── .gitignore               # Git ignored files list
├── README.md                # Project documentation
├── .travis.yml              # Travis CI continuous integration configuration
├── pom.xml                  # Maven Project Object Model definition file
└── commit.sh                # Commit helper script (assumed)
```

Naming Convention: Uses Java package naming convention (reverse domain name) + modular lowercase hyphen-separated style  
Layered Structure: Standard Spring Boot layered structure including startup class, configuration (config), controllers (controller), utilities (utils), and error handling (error) modules  
Extended Design: Supports Spring configuration metadata extension via resources/META-INF, provides Docker deployment capability, and includes CI/CD integration configuration (Travis)