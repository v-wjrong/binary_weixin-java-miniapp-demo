[回到首页](../README.md)

## 项目概览
### 项目基本信息
- **名称:** spring-boot-demo-for-wechat-miniapp
- **GroupId (Maven):** com.github.binarywang
- **ArtifactId (Maven):** weixin-java-miniapp-demo
- **Version:** 1.0.0-SNAPSHOT
- **主要编程语言:** Java

## 先决条件
- **JDK 版本:** 1.8
- **构建工具版本:** Maven 3.2+
- **网络连接中间件依赖:**
  - Redis (通过 `jedis` 客户端依赖推断)

## 构建指南
### Maven 构建 
- 构建命令:
    - 清理构建: `mvn clean`
    - 编译项目: `mvn compile`
    - 打包项目: `mvn package`
    - 安装到本地仓库: `mvn install`
    - 部署项目: `mvn deploy`
- 构建流程: 
    Maven 构建流程基于生命周期，主要包括清理（clean）、默认（default）和站点（site）三个阶段。在默认生命周期中，会依次执行编译源码、运行测试、打包项目、集成测试以及部署等步骤。此项目为 Spring Boot 应用，通过 `spring-boot-maven-plugin` 插件支持可执行 JAR 包的生成。
- 打包目录: 
    打包后的主要文件位于 `target/` 目录下，包括编译后的 class 文件、资源文件及最终生成的可执行 JAR 文件（如 `weixin-java-miniapp-demo-1.0.0-SNAPSHOT.jar`）。Docker 镜像相关配置位于 `src/main/docker`，并通过 `docker-maven-plugin` 插件进行构建。

## 依赖管理
### 主要依赖
- **Spring Boot Web Starter**: 提供 Web 应用开发基础支持（内含嵌入式 Tomcat 和 Spring MVC）。
- **Spring Boot Thymeleaf Starter**: 支持使用 Thymeleaf 模板引擎渲染视图。
- **weixin-java-miniapp**: 微信小程序 Java SDK，用于与微信服务器交互，版本由属性 `${weixin-java-miniapp.version}` 控制，值为 `4.7.0`。
- **commons-fileupload**: Apache Commons FileUpload 库，用于处理文件上传，版本为 `1.5`。
- **Spring Boot Test Starter**: 提供测试支持，作用域为 `test`。
- **Spring Boot Configuration Processor**: 用于处理配置元数据，可选依赖。
- **Lombok**: 简化 Java 代码编写工具，作用域为 `provided`。
- **Jedis**: Redis 客户端库，用于操作 Redis 数据库。

### 添加/修改依赖
- **Maven:** 在 `pom.xml` 文件的 `<dependencies>` 标签中添加或修改 `<dependency>` 元素。

### 依赖版本管理
- **Maven (`<dependencyManagement>`):** 本项目继承自 `spring-boot-starter-parent`，其中定义了常用依赖的版本，因此大部分 Spring Boot 相关依赖无需显式指定版本。对于非 Spring Boot 管理的依赖（如 `weixin-java-miniapp`、`commons-fileupload`），需手动声明版本号；也可以通过在 `pom.xml` 中使用 `<dependencyManagement>` 来统一控制依赖版本。



## 工程结构

```text
weixin-java-miniapp-demo/
├── .git/                    # Git版本控制目录
├── .github/                 # GitHub相关配置（如FUNDING.yml）
├── src/
│   └── main/
│       ├── java/
│       │   └── com/
│       │       └── github/
│       │           └── binarywang/
│       │               └── demo/
│       │                   └── wx/
│       │                       └── miniapp/
│       │                           ├── WxMaDemoApplication.java  # Spring Boot启动类
│       │                           ├── config/                  # 配置类目录
│       │                           │   ├── WxMaProperties.java  # 微信小程序属性配置
│       │                           │   └── WxMaConfiguration.java  # 微信小程序配置初始化
│       │                           ├── controller/              # 控制器层（处理HTTP请求）
│       │                           │   ├── WxMaMediaController.java
│       │                           │   ├── WxMaUserController.java
│       │                           │   └── WxPortalController.java
│       │                           ├── error/                   # 错误处理模块
│       │                           │   ├── ErrorController.java
│       │                           │   └── ErrorPageConfiguration.java
│       │                           └── utils/                   # 工具类目录
│       │                               └── JsonUtils.java
│       ├── resources/
│       │   ├── application.yml.template  # 应用配置模板文件
│       │   ├── tmp.png                  # 示例图片资源（推测）
│       │   ├── templates/               # 页面模板目录（推测为Thymeleaf等使用）
│       │   │   └── error.html           # 自定义错误页面
│       │   └── META-INF/
│       │       └── additional-spring-configuration-metadata.json  # Spring配置元数据扩展
│       └── docker/
│           └── Dockerfile               # Docker镜像构建脚本
├── .editorconfig            # 编辑器编码风格统一配置
├── .gitignore               # Git忽略文件列表
├── README.md                # 项目说明文档
├── .travis.yml              # Travis CI持续集成配置
├── pom.xml                  # Maven项目对象模型文件
└── commit.sh                # 提交辅助脚本（推测）
```

命名规约：Java包名采用反向域名结构（如com.github.binarywang），目录与代码模块功能对应  
分层结构：基于Spring Boot的标准分层架构，包括controller、config、utils、error等模块  
扩展设计：通过docker目录支持容器化部署；resources下支持静态资源和模板渲染；META-INF提供配置元信息扩展能力