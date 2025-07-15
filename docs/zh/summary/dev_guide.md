[回到首页](../README.md)

## 项目概览
### 项目基本信息
- **名称:** spring-boot-demo-for-wechat-miniapp
- **GroupId (Maven):** com.github.binarywang
- **ArtifactId (Maven):** weixin-java-miniapp-demo
- **Version:** 1.0.0-SNAPSHOT
- **主要编程语言:** Java

## 先决条件
- **JDK 版本:** 1.8 (根据 Maven 的 `<maven.compiler.source>` 和 `<maven.compiler.target>` 属性配置)
- **构建工具版本:** Maven (根据 `pom.xml` 文件识别)
- **网络连接中间件依赖:**
  - Redis (通过 `jedis` 依赖推断，未明确指定版本，由 Spring Boot 父 POM 管理)

## 构建指南
### Maven 构建
- 构建命令:
    - 清理构建: `mvn clean`
    - 编译项目: `mvn compile`
    - 打包项目: `mvn package`
    - 安装到本地仓库: `mvn install`
    - 部署项目: `mvn deploy`
- 构建流程: 
    - Maven 的构建流程主要包括以下阶段：
        1. **清理 (clean)**: 删除之前构建生成的文件
        2. **编译 (compile)**: 编译项目源代码
        3. **测试 (test)**: 运行单元测试
        4. **打包 (package)**: 将编译后的代码打包成可分发的格式（如 JAR）
        5. **验证 (verify)**: 对集成测试结果进行检查
        6. **安装 (install)**: 将包安装到本地仓库
        7. **部署 (deploy)**: 将最终的包复制到远程仓库
- 打包目录: 
    - 打包后的 JAR 文件会生成在 `target/` 目录下
    - 项目使用 Spring Boot Maven 插件，会生成可执行的 JAR 文件
    - Docker 构建文件位于 `src/main/docker` 目录，打包时会包含在 Docker 镜像中

## 依赖管理
### 主要依赖
- **Spring Boot 基础支持**:
  - `spring-boot-starter-web` (Web 开发支持)
  - `spring-boot-starter-thymeleaf` (模板引擎)
  - `spring-boot-starter-test` (测试支持，scope: test)
  - `spring-boot-configuration-processor` (配置处理，optional: true)
- **微信小程序 SDK**:
  - `weixin-java-miniapp` (版本: 4.7.0，通过属性 `${weixin-java-miniapp.version}` 管理)
- **工具库**:
  - `commons-fileupload` (文件上传，版本: 1.5)
  - `lombok` (代码简化，scope: provided)
  - `jedis` (Redis 客户端)
- **Docker 支持**:
  - `docker-maven-plugin` (版本: 1.0.0)

### 添加/修改依赖
- **Maven:** 在 `pom.xml` 文件的 `<dependencies>` 标签中添加或修改 `<dependency>` 元素。例如：
  ```xml
  <dependency>
      <groupId>org.example</groupId>
      <artifactId>new-dependency</artifactId>
      <version>1.0.0</version>
  </dependency>
  ```

### 依赖版本管理
- **Maven (`<dependencyManagement>`):**  
  该文件通过 `<parent>` 继承了 `spring-boot-starter-parent` 的依赖管理（版本 2.6.3），隐式管理了大部分 Spring Boot 相关依赖的版本。自定义依赖版本可通过 `<properties>` 定义（如 `${weixin-java-miniapp.version}`），或在 `<dependency>` 中直接指定 `version` 标签。



## 工程结构

```text
weixin-java-miniapp-demo/
├── .editorconfig        # 编辑器配置（跨IDE统一风格）
├── .gitignore          # Git忽略规则
├── README.md           # 项目说明文档
├── .travis.yml         # CI/CD配置（Travis CI）
├── pom.xml             # Maven项目配置文件
├── commit.sh           # Git提交脚本（推测）
├── src/
│   └── main/
│       ├── java/
│       │   └── com/github/binarywang/demo/wx/miniapp/
│       │       ├── WxMaDemoApplication.java  # SpringBoot启动类
│       │       ├── controller/                # 微信小程序控制器层
│       │       │   ├── WxMaMediaController.java   # 媒体处理
│       │       │   ├── WxMaUserController.java    # 用户管理
│       │       │   └── WxPortalController.java    # 消息入口
│       │       ├── utils/                     # 工具类
│       │       │   └── JsonUtils.java          # JSON处理工具
│       │       ├── error/                     # 异常处理
│       │       │   ├── ErrorController.java    # 错误控制器
│       │       │   └── ErrorPageConfiguration.java # 错误页面配置
│       │       └── config/                    # 微信配置
│       │           ├── WxMaProperties.java     # 配置属性类
│       │           └── WxMaConfiguration.java  # 配置初始化
│       ├── resources/
│       │   ├── application.yml.template       # 配置模板
│       │   ├── templates/                     # 前端模板
│       │   │   └── error.html                 # 错误页模板
│       │   └── META-INF/                      # 元数据配置
│       │       └── additional-spring-configuration-metadata.json # Spring配置元数据
│       └── docker/
│           └── Dockerfile                     # 容器化部署配置
├── .git/                # Git版本控制目录
└── .github/
    └── FUNDING.yml      # GitHub赞助配置
```

命名规约：采用标准Java包命名（com.github.组织名），目录使用小写中划线风格  
分层结构：  
- 核心分层：SpringBoot标准结构（main/java + main/resources）  
- 业务分层：按微信功能模块划分（controller/config/utils）  
- 特殊分层：独立error处理层和docker部署层  

扩展设计：  
1. 通过config目录实现微信SDK配置解耦  
2. 提供application.yml.template支持配置模板化  
3. 包含完整的CI/CD支持（.travis.yml + Dockerfile）  
4. 错误处理系统化（独立error目录+HTML模板）