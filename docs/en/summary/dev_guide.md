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
- **Build Tool Version:** Maven 3.2+
- **Network-connected Middleware Dependencies:**
  - Redis (inferred via the `jedis` client dependency)

## Build Guide
### Maven Build
- Build Commands:
    - Clean build: `mvn clean`
    - Compile project: `mvn compile`
    - Package project: `mvn package`
    - Install to local repository: `mvn install`
    - Deploy project: `mvn deploy`
- Build Process: 
    The Maven build process is based on lifecycle phases, mainly including `clean`, `compile`, `test`, `package`, `install`, and `deploy`. This project uses the Spring Boot plugin to build executable JAR files and configures a Docker plugin for image packaging.
- Packaging Directory: 
    Build artifacts are located by default in the `target/` directory, primarily including:
    - Compiled class files in `target/classes/`
    - Packaged JAR file at `target/weixin-java-miniapp-demo-1.0.0-SNAPSHOT.jar`
    - If using the Docker plugin, Docker image-related resources will be generated in `target/docker/` or a specified directory.

## Dependency Management
### Main Dependencies
- **Spring Boot Web Starter**: Provides foundational support for web application development, such as embedded Tomcat and Spring MVC.
- **Spring Boot Thymeleaf Starter**: Supports view rendering using the Thymeleaf template engine.
- **weixin-java-miniapp**: WeChat Mini Program Java SDK used for interacting with WeChat servers and handling messages, login logic, etc. Its version is controlled by property `${weixin-java-miniapp.version}`, set to `4.7.0`.
- **commons-fileupload**: Apache Commons FileUpload component used for handling file upload functionality. Version is `1.5`.
- **Spring Boot Test Starter**: Provides testing support including frameworks like JUnit and Mockito. Scope is `test`.
- **Spring Boot Configuration Processor**: Used for processing configuration metadata to assist IDE auto-completion. Marked as an optional dependency.
- **Lombok**: Utility library that reduces boilerplate code (e.g., getters/setters). Scope is `provided`, effective during compilation.
- **Jedis**: Redis client library used for operating the Redis database.

### Adding/Modifying Dependencies
- **Maven:** Add or modify `<dependency>` elements within the `<dependencies>` tag of the `pom.xml` file.

### Dependency Version Management
- **Maven (`<dependencyManagement>`):** This project inherits from `spring-boot-starter-parent`, which already defines versions for many commonly used dependencies; thus, some dependencies do not require explicit version specification. For dependencies not managed by the parent POM (such as `weixin-java-miniapp` and `commons-fileupload`), version numbers must be manually specified. Alternatively, centralized control over dependency versions can also be achieved through declarations within `<dependencyManagement>`.

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
│       │                           ├── config/                   # Configuration class directory (Spring convention)
│       │                           ├── controller/               # Controller layer (MVC structure)
│       │                           ├── error/                    # Error handling module (assumed)
│       │                           └── utils/                    # Utility class directory (common naming)
│       ├── resources/
│       │   ├── application.yml.template  # Configuration template file (commonly used in Spring Boot)
│       │   ├── tmp.png                   # Sample image resource (assumed)
│       │   ├── templates/                # Page template directory (used by template engines like Thymeleaf)
│       │   │   └── error.html            # Custom error page
│       │   └── META-INF/
│       │       └── additional-spring-configuration-metadata.json  # Spring configuration metadata (for hints)
│       └── docker/
│           └── Dockerfile                # Docker build description file
├── .editorconfig             # Editor encoding style uniform configuration
├── .gitignore                # Git ignore rules configuration
├── .travis.yml               # Travis CI continuous integration configuration
├── README.md                 # Project documentation
├── pom.xml                   # Maven Project Object Model file (core of Java projects)
└── commit.sh                 # Commit script (presumably an auxiliary development tool)
```

Naming Convention: Standard Maven project structure + fully qualified reverse domain name for Java packages (com.github.binarywang)

Layered Structure: Typical Spring Boot project structure including common functional modules such as controller, config, utils, and error, conforming to lightweight web application layering

Extended Design: Supports containerized deployment via the docker directory; supports view rendering through resources/templates; enhances configuration readability using META-INF; provides GitHub and CI integration support