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
- **构建工具版本:** Maven (根据 `pom.xml` 文件识别，但未明确指定 Maven 版本，建议使用与 Spring Boot 2.6.3 兼容的版本)
- **网络连接中间件依赖:**
  - Redis (通过 `jedis` 依赖引入，但未明确指定版本)

## 构建指南
### Maven 构建
- 构建命令:
    - 清理构建: `mvn clean`
    - 编译项目: `mvn compile`
    - 打包项目: `mvn package`
    - 安装到本地仓库: `mvn install`
    - 部署项目: `mvn deploy`
- 构建流程: 
    - Maven 的构建流程主要包括清理、编译、测试、打包、验证、安装和部署等阶段。该项目使用了 Spring Boot 和 Docker 插件，因此构建时会生成可执行的 JAR 文件，并支持 Docker 镜像构建。
- 打包目录: 
    - 打包后的 JAR 文件位于 `target/` 目录下，文件名为 `${project.build.finalName}.jar`。
    - Docker 构建所需的文件位于 `src/main/docker/` 目录下，构建后的镜像名称为 `${docker.image.prefix}/${project.artifactId}`。

## 依赖管理
### 主要依赖
- **Spring Boot Web**: `spring-boot-starter-web` - 提供Spring Boot Web开发支持（版本通过父POM继承）
- **Spring Boot Thymeleaf**: `spring-boot-starter-thymeleaf` - 集成Thymeleaf模板引擎（版本通过父POM继承）
- **微信小程序SDK**: `weixin-java-miniapp` - 微信小程序Java SDK（版本：4.7.0，通过属性`weixin-java-miniapp.version`管理）
- **文件上传**: `commons-fileupload` - Apache Commons文件上传组件（版本：1.5）
- **测试支持**: `spring-boot-starter-test` - Spring Boot测试模块（scope: test）
- **配置处理**: `spring-boot-configuration-processor` - 配置元数据生成（optional: true）
- **Lombok**: `lombok` - 简化Java代码开发（scope: provided）
- **Redis客户端**: `jedis` - Redis Java客户端（版本通过父POM继承）

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
  该POM通过父POM（`spring-boot-starter-parent`）继承大部分依赖版本，局部版本通过`<properties>`标签集中管理（如`weixin-java-miniapp.version`）。若需自定义版本，可在`<dependencyManagement>`中覆盖父POM的版本声明。



## 工程结构

```text
weixin-java-miniapp-demo/
├── .editorconfig       # 编辑器配置规范（跨团队协作标准）
├── .git/              # Git版本控制目录
├── .github/           # GitHub平台配置目录
│   └── FUNDING.yml    # GitHub赞助配置文件
├── .gitignore         # Git忽略规则配置
├── .travis.yml        # Travis CI持续集成配置
├── commit.sh          # 提交脚本（推测为自动化脚本）
├── pom.xml            # Maven项目配置文件
├── README.md          # 项目说明文档
└── src/               # 源代码目录
    └── main/
        ├── docker/    # Docker容器化配置
        │   └── Dockerfile
        ├── java/      # Java源代码
        │   └── com/github/binarywang/demo/wx/miniapp/
        │       ├── WxMaDemoApplication.java  # SpringBoot启动类
        │       ├── config/                   # 微信小程序配置类
        │       ├── controller/               # MVC控制器层
        │       ├── error/                    # 错误处理模块
        │       └── utils/                    # 工具类
        └── resources/ # 资源文件
            ├── application.yml.template      # 配置模板
            ├── META-INF/                    # 元信息目录
            ├── templates/                   # 模板文件
            └── tmp.png                      # 临时图片资源
```

命名规约：
- 采用标准Maven项目结构（src/main/java/resources）
- 包名遵循Java反向域名规范（com.github.binarywang）
- 配置文件使用.yml格式（SpringBoot标准）

分层结构：
1. 控制层：controller目录处理HTTP请求
2. 配置层：config目录管理微信SDK配置
3. 工具层：utils提供JSON转换等基础能力
4. 错误处理：独立error目录统一管理异常

扩展设计：
1. 通过docker目录支持容器化部署
2. 提供CI/CD配置文件（.travis.yml）
3. 配置模板与元数据分离（application.yml.template + META-INF）
4. 前端模板与后端分离（templates目录）