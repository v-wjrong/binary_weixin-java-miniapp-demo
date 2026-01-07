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
- **构建工具版本:** Maven 3.6+ (基于 spring-boot-starter-parent 推荐版本)
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
    Maven 构建流程遵循标准的生命周期，主要包括清理（clean）、编译（compile）、测试（test）、打包（package）、安装（install）和部署（deploy）阶段。本项目为 Spring Boot 应用，通过 `spring-boot-maven-plugin` 插件支持可执行 JAR 的打包方式。
- 打包目录: 
    打包后的主要文件位于 `target/` 目录下，生成的可执行 JAR 文件名为 `weixin-java-miniapp-demo-1.0.0-SNAPSHOT.jar`。Docker 镜像相关资源在 `src/main/docker` 中定义，并会将该 JAR 包含进镜像中。

## 依赖管理
### 主要依赖
- **Spring Boot Web Starter**: 提供 Web 应用开发基础支持，如内嵌 Tomcat、Spring MVC 等。
- **Spring Boot Thymeleaf Starter**: 集成 Thymeleaf 模板引擎用于页面渲染。
- **weixin-java-miniapp**: 微信小程序 Java SDK，用于与微信小程序接口交互，版本通过属性 `${weixin-java-miniapp.version}` 控制，当前为 `4.7.0`。
- **commons-fileupload**: Apache Commons 文件上传工具库，版本为 `1.5`。
- **Spring Boot Test Starter**: Spring Boot 测试框架支持，作用域为 `test`。
- **Spring Boot Configuration Processor**: 用于处理配置元数据，标记为可选依赖。
- **Lombok**: 提供简洁的 Java 实体类注解（如 `@Data`），作用域为 `provided`。
- **Jedis**: Redis 客户端库，用于操作 Redis 数据库。

### 添加/修改依赖
- **Maven:** 在 `pom.xml` 文件的 `<dependencies>` 标签中添加或修改 `<dependency>` 元素。

### 依赖版本管理
- **Maven (`<dependencyManagement>`):** Maven 使用 `<dependencyManagement>` 来统一管理依赖版本，通常在父 POM 中定义。本项目继承了 `spring-boot-starter-parent`，其中已包含大部分 Spring Boot 官方依赖的版本控制。对于未预定义的依赖（如 `weixin-java-miniapp` 和 `commons-fileupload`），则直接在 `<dependency>` 中指定版本或使用属性变量进行管理。



## 工程结构

```text
weixin-java-miniapp-demo/
├── .git/                    # Git版本控制目录
├── .github/                 # GitHub相关配置（如FUNDING.yml）
├── src/
│   └── main/
│       ├── java/            # Java源代码目录
│       │   └── com/
│       │       └── github/
│       │           └── binarywang/
│       │               └── demo/
│       │                   └── wx/
│       │                       └── miniapp/              # 微信小程序演示项目主包
│       │                           ├── WxMaDemoApplication.java  # Spring Boot启动类
│       │                           ├── controller/       # 控制器层（处理HTTP请求）
│       │                           ├── utils/            # 工具类（如JsonUtils）
│       │                           ├── error/            # 错误处理模块
│       │                           └── config/           # 配置类（如微信配置、属性绑定）
│       ├── resources/        # 资源文件目录
│       │   ├── application.yml.template  # 应用配置模板文件
│       │   ├── tmp.png       # 示例图片资源（推测）
│       │   ├── templates/    # 模板页面目录（如Thymeleaf）
│       │   │   └── error.html  # 自定义错误页面
│       │   └── META-INF/     # 元信息目录
│       │       └── additional-spring-configuration-metadata.json  # Spring配置元数据
│       └── docker/           # Docker构建相关文件
│           └── Dockerfile    # Docker镜像构建脚本
├── .editorconfig             # 编辑器格式统一配置
├── .gitignore                # Git忽略文件列表
├── README.md                 # 项目说明文档
├── .travis.yml               # Travis CI持续集成配置
├── pom.xml                   # Maven项目对象模型文件
└── commit.sh                 # 提交辅助脚本（推测）
```

命名规约：采用Maven标准目录结构，Java包名使用反向域名+功能模块划分  
分层结构：基于Spring Boot的典型分层，包括controller、config、utils、error等模块，体现关注点分离原则  
扩展设计：通过docker目录支持容器化部署；通过resources/templates支持视图渲染；通过META-INF增强Spring配置能力