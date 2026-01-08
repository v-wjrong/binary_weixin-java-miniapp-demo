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
- **构建工具版本:** Maven (推荐使用与 Spring Boot 2.6.3 兼容的 Maven 版本，通常为 Maven 3.6+)
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
    Maven 构建流程遵循标准的生命周期，主要包括清理（clean）、资源处理、编译、测试、打包、集成测试、验证、安装和部署等阶段。该项目为 Spring Boot 应用，通过 `spring-boot-maven-plugin` 插件支持可执行 JAR 包的构建。
- 打包目录: 
    构建完成后，默认生成的 JAR 文件位于 `target/` 目录下，文件名为 `${project.artifactId}-${project.version}.jar`，例如：`weixin-java-miniapp-demo-1.0.0-SNAPSHOT.jar`。Docker 相关配置也指向该目录以获取最终的 JAR 文件进行镜像构建。

## 依赖管理
### 主要依赖
- **Spring Boot Web Starter**: 提供 Web 应用开发支持，包括内嵌 Tomcat 和 Spring MVC。
- **Spring Boot Thymeleaf Starter**: 集成 Thymeleaf 模板引擎，用于服务端渲染页面。
- **weixin-java-miniapp**: 微信小程序 Java SDK，用于与微信接口交互，版本由属性 `${weixin-java-miniapp.version}` 控制，实际值为 `4.7.0`。
- **commons-fileupload**: Apache Commons 文件上传组件，版本为 `1.5`，用于处理文件上传功能。
- **Spring Boot Test Starter**: 提供测试支持，作用域为 `test`，用于单元测试和集成测试。
- **Spring Boot Configuration Processor**: 用于处理配置元数据，标记为可选依赖。
- **Lombok**: 简化 Java 代码编写（如 getter/setter），作用域为 `provided`，编译时生效。
- **Jedis**: Redis 客户端库，用于操作 Redis 数据库。

### 添加/修改依赖
- **Maven:** 在 `pom.xml` 文件的 `<dependencies>` 标签中添加或修改 `<dependency>` 元素。

### 依赖版本管理
- **Maven (`<dependencyManagement>`):** 本项目继承自 `spring-boot-starter-parent`，其中已定义了许多常用依赖的版本，因此大部分 Spring Boot 相关依赖无需显式指定版本号。对于非 Spring Boot 管理的依赖（如 `commons-fileupload` 和 `weixin-java-miniapp`），需要手动指定版本；也可以通过在 `<dependencyManagement>` 中集中声明来统一控制版本。



## 工程结构

```text
weixin-java-miniapp-demo/
├── .git/                    # Git版本控制目录
├── .github/                 # GitHub相关配置（如FUNDING.yml）
│   └── FUNDING.yml          # 资助项目信息文件
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
│       │                           │   ├── WxMaProperties.java  # 微信小程序属性配置类
│       │                           │   └── WxMaConfiguration.java  # 微信小程序配置初始化类
│       │                           ├── controller/              # 控制器层（处理HTTP请求）
│       │                           │   ├── WxMaMediaController.java
│       │                           │   ├── WxMaUserController.java
│       │                           │   └── WxPortalController.java
│       │                           ├── error/                   # 错误处理模块
│       │                           │   ├── ErrorController.java
│       │                           │   └── ErrorPageConfiguration.java
│       │                           └── utils/                   # 工具类目录
│       │                               └── JsonUtils.java       # JSON工具类
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
├── pom.xml                  # Maven项目对象模型定义文件
└── commit.sh                # 提交辅助脚本（推测）
```

命名规约：采用Java包命名规范（反向域名）+ 模块化小写中划线风格  
分层结构：Spring Boot标准分层结构，包括启动类、配置(config)、控制器(controller)、工具(utils)与错误处理(error)模块  
扩展设计：通过resources/META-INF支持Spring配置元数据扩展，并提供Docker部署能力及CI/CD集成配置（Travis）