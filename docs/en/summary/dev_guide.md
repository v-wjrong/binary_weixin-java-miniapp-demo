[Go to Homepage](../README.md)

## Project Overview
### Basic Project Information
- **Name:** spring-boot-demo-for-wechat-miniapp  
- **GroupId (Maven):** com.github.binarywang  
- **ArtifactId (Maven):** weixin-java-miniapp-demo  
- **Version:** 1.0.0-SNAPSHOT  
- **Primary Programming Language:** Java  

## Prerequisites
- **JDK Version:** 1.8 (inferred from Maven's `<maven.compiler.source>` and `<maven.compiler.target>` properties)  
- **Build Tool Version:** Maven (identified from `pom.xml`, recommended to use a version compatible with Spring Boot 2.6.3, typically Maven 3.6.3+)  
- **Network Connection Middleware Dependencies:**  
  - Redis (inferred from `jedis` dependency, but no explicit version specified; defaults to the version managed by Spring Boot 2.6.3)  

## Build Guide
### Maven Build
- Build Commands:  
    - Clean build: `mvn clean`  
    - Compile project: `mvn compile`  
    - Package project: `mvn package`  
    - Install to local repository: `mvn install`  
    - Deploy project: `mvn deploy`  
- Build Process:  
    - This is a Maven project based on Spring Boot. The build process includes dependency download, code compilation, test execution, and packaging. The project uses the Spring Boot Maven plugin to support building executable JAR files. Additionally, it integrates the Docker Maven plugin, allowing Docker image builds via Maven commands.  
- Packaging Directory:  
    - The packaged JAR file is located in the `target/` directory, named `${project.build.finalName}.jar` (e.g., `weixin-java-miniapp-demo-1.0.0-SNAPSHOT.jar`).  
    - Files required for Docker builds are in the `src/main/docker/` directory. During packaging, this directory and the generated JAR file are used together to build the Docker image.  

## Dependency Management
### Key Dependencies
- **Spring Boot Starter Web:** Provides Spring MVC and embedded Tomcat for building web applications (version inherited from parent POM 2.6.3).  
- **Spring Boot Starter Thymeleaf:** Integrates the Thymeleaf templating engine (version inherited from parent POM).  
- **weixin-java-miniapp:** WeChat Mini Program Java SDK (explicit version 4.7.0 managed via property `weixin-java-miniapp.version`).  
- **commons-fileupload:** File upload utility library (explicit version 1.5).  
- **Spring Boot Starter Test:** Testing support (inherited version, scope=test).  
- **Lombok:** Annotation-driven code generation tool (scope=provided).  
- **Jedis:** Redis client (no explicit version specified; inherited from Spring Boot dependency management).  

### Adding/Modifying Dependencies
- **Maven:** Add or modify `<dependency>` elements within the `<dependencies>` tag in the `pom.xml` file. For example:  
  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
  </dependency>
  ```

### Dependency Version Management
- **Maven (`<dependencyManagement>`):**  
  1. Most dependency versions are centrally managed via the parent POM (`spring-boot-starter-parent`), eliminating the need for repeated version specifications in submodules.  
  2. Custom versions can be defined via `<properties>` (e.g., `weixin-java-miniapp.version`) and referenced in dependencies using `${property}`.  
  3. Explicitly specified versions (e.g., `commons-fileupload` version 1.5) override inherited version management.

## Project Structure

```text
weixin-java-miniapp-demo/
├── .editorconfig       # Editor configuration standards (cross-team collaboration)  
├── .git/              # Git version control directory  
├── .github/           # GitHub platform configuration directory  
│   └── FUNDING.yml    # GitHub sponsorship configuration  
├── .gitignore         # Git ignore rules  
├── .travis.yml        # Travis CI continuous integration configuration  
├── commit.sh          # Commit script (presumed automation script)  
├── pom.xml            # Maven project configuration file  
├── README.md          # Project documentation  
└── src/               # Source code directory  
    └── main/  
        ├── docker/    # Docker containerization configuration  
        │   └── Dockerfile  
        ├── java/      # Java source code  
        │   └── com/github/binarywang/demo/wx/miniapp/  
        │       ├── WxMaDemoApplication.java  # SpringBoot main class  
        │       ├── config/                   # WeChat Mini Program configuration classes  
        │       ├── controller/               # MVC controller layer  
        │       ├── error/                    # Error handling module  
        │       └── utils/                    # Utility classes  
        └── resources/ # Resource files  
            ├── application.yml.template      # Configuration template  
            ├── META-INF/                    # Metadata directory  
            ├── templates/                   # Template files  
            └── tmp.png                      # Temporary image resource  

Naming Conventions:  
- Standard Maven project structure (src/main/java/resources)  
- Package names follow Java reverse domain convention (com.github.binarywang)  
- Configuration files use .yml format (SpringBoot standard)  

Layered Architecture:  
1. Controller layer: `controller` directory handles HTTP requests  
2. Configuration layer: `config` directory manages WeChat SDK configurations  
3. Utility layer: `utils` provides foundational capabilities (e.g., JSON conversion)  
4. Error handling: Dedicated `error` directory for unified exception management  

Extension Design:  
1. Supports containerized deployment via `docker` directory  
2. Includes CI/CD configuration files (.travis.yml)  
3. Separates configuration templates from metadata (application.yml.template + META-INF)  
4. Decouples frontend templates from backend (templates directory)  
```