# SpringBoot 从入门到精通

> **文档说明**：本文档基于 SpringBoot 3.5.x 版本编写，涵盖基础篇、开发篇、运维篇、原理篇四大核心模块，适用于 Java 17+ 开发环境。

---

## 📚 目录

- [一、SpringBoot 快速入门](#一springboot-快速入门)
  - [1.1 SpringBoot 简介](#11-springboot-简介)
  - [1.2 四种项目创建方式](#12-四种项目创建方式)
  - [1.3 核心原理剖析](#13-核心原理剖析)
- [二、SpringBoot 基础配置](#二springboot-基础配置)
  - [2.1 配置文件格式](#21-配置文件格式)
  - [2.2 YAML 语法详解](#22-yaml-语法详解)
  - [2.3 配置数据读取](#23-配置数据读取)
  - [2.4 多环境配置](#24-多环境配置)
- [三、SpringBoot 开发实战](#三springboot-开发实战)
  - [3.1 整合 JUnit](#31-整合-junit)
  - [3.2 整合 MyBatis](#32-整合-mybatis)
  - [3.3 整合 Druid](#33-整合-druid)
  - [3.4 Web 开发与 Thymeleaf](#34-web-开发与-thymeleaf)
- [四、SpringBoot 运维实战](#四springboot-运维实战)
  - [4.1 打包与部署](#41-打包与部署)
  - [4.2 日志管理](#42-日志管理)
  - [4.3 监控与可观测性](#43-监控与可观测性)
- [五、SpringBoot 原理深度解析](#五springboot-原理深度解析)
- [六、开发工具附录](#六开发工具附录)

---

## 一、SpringBoot 快速入门

### 1.1 SpringBoot 简介

#### 1.1.1 什么是 SpringBoot

SpringBoot 是由 Pivotal 团队提供的全新框架，其设计目的是用来**简化 Spring 应用的初始搭建以及开发过程**。它基于"约定优于配置"的理念，通过自动配置大大减少了开发工作量。

#### 1.1.2 SpringBoot vs Spring 传统开发

| 维度 | Spring 传统开发 | SpringBoot 开发 |
|------|---------------|-----------------|
| 依赖管理 | 手动添加多个依赖，需管理版本冲突 | 使用 Starter，自动管理版本 |
| 配置文件 | 需编写大量 XML 或 Java 配置类 | 零配置或少量 YAML 配置 |
| Web 服务器 | 需手动安装和配置 Tomcat/Jetty | 内嵌服务器，开箱即用 |
| 项目构建 | 手动整合各组件 | 脚手架快速生成项目结构 |
| 开发效率 | 较低，配置繁琐 | 高效，专注于业务逻辑 |

#### 1.1.3 SpringBoot 3.x 核心特性

**🔥 技术基线升级**
- **最低 JDK 要求**：Java 17+（推荐 Java 21）
- **Spring Framework**：6.1.x+
- **Jakarta EE**：9+（包名从 `javax.*` 迁移至 `jakarta.*`）

**🚀 新特性亮点**
1. **原生镜像支持**：基于 GraalVM 实现毫秒级启动
2. **虚拟线程集成**：Java 21+ 下提供高并发处理能力
3. **声明式 HTTP 客户端**：`@HttpExchange` 注解简化远程调用
4. **可观测性增强**：内置 Micrometer Tracing，支持 OpenTelemetry
5. **AOT 编译**：提前编译优化，提升冷启动性能
6. **结构化日志**：支持 ECS（Elastic Common Schema）格式输出

### 1.2 四种项目创建方式

#### 方式一：IDEA 向导创建（推荐新手）

**前置条件**
- JDK 17+ 已安装
- IDEA 2023.2+ 版本
- Maven 3.6.3+ 或 Gradle 7.5+

**操作步骤**

**步骤①**：创建新模块，选择 **Spring Initializr**

```
File → New → Project → Spring Initializr → Next
```

**步骤②**：配置项目基础信息

```yaml
Project: Maven Project
Language: Java
Spring Boot: 3.5.0  # 最新稳定版
Packaging: Jar
Java: 17
```

**步骤③**：选择技术集（Dependencies）

```yaml
Spring Web:          # Web 开发支持
Spring Data JPA:     # 数据持久化
Validation:           # 数据校验
Actuator:            # 监控端点
Lombok:              # 简化代码
```

**步骤④**：创建控制器测试

```java
@RestController
@RequestMapping("/api/news")
public class NewsController {
    
    @GetMapping
    public String hello() {
        return "SpringBoot 3.x is running!";
    }
}
```

**步骤⑤**：运行引导类

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

> **💡 温馨提示**：SpringBoot 内嵌 Tomcat 10.x（基于 Jakarta Servlet 6.0），无需手动配置。

---

#### 方式二：Spring 官网在线创建

**访问地址**：https://start.spring.io/

**操作步骤**

1. 选择项目元数据
   - Project: Maven
   - Language: Java
   - Spring Boot: 3.5.0
   - Packaging: Jar
   - Java: 17

2. 添加依赖（ADD DEPENDENCIES）
   - Spring Web
   - Spring Data JPA
   - MySQL Driver

3. 生成项目（GENERATE）
   - 下载 ZIP 压缩包
   - 解压后导入 IDEA

---

#### 方式三：阿里云镜像创建（国内推荐）

**镜像地址**：https://start.aliyun.com

**优势分析**
- ⚡ 下载速度快（国内 CDN 加速）
- 📦 集成阿里云中间件（Druid、Nacos 等）
- 🇨🇳 中文界面友好

**注意事项**
- 默认 SpringBoot 版本可能较旧，需手动升级
- 配置文件格式略有差异

---

#### 方式四：手工创建 Maven 项目

**适用场景**：离线环境、深度定制需求

**步骤①**：创建标准 Maven 项目

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.5.0</version>
    </parent>

    <groupId>com.example</groupId>
    <artifactId>springboot-demo</artifactId>
    <version>1.0.0</version>
    <properties>
        <java.version>17</java.version>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
</project>
```

**步骤②**：创建引导类

```java
package com.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

**步骤③**：创建 Controller 测试

```java
package com.example.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
    
    @GetMapping("/hello")
    public String hello() {
        return "Hello SpringBoot 3.x!";
    }
}
```

---

### 1.3 核心原理剖析

#### 1.3.1 起步依赖（Starter）

**作用**：将常用的依赖组合成一个整体，简化依赖配置

**官方 Starter 命名规则**：`spring-boot-starter-{技术名}`

| Starter 名称 | 功能描述 | 核心依赖 |
|-------------|---------|-----------|
| spring-boot-starter-web | Web 开发支持 | Spring MVC, Tomcat, Jackson |
| spring-boot-starter-data-jpa | JPA 持久化 | Hibernate, Spring Data JPA |
| spring-boot-starter-data-redis | Redis 缓存 | Spring Data Redis, Lettuce |
| spring-boot-starter-security | 安全框架 | Spring Security |
| spring-boot-starter-actuator | 监控端点 | Micrometer, Health Indicators |

**第三方 Starter 命名规则**：`{技术名}-spring-boot-starter`

| Starter 名称 | 功能描述 |
|-------------|---------|
| mybatis-spring-boot-starter | MyBatis 集成 |
| druid-spring-boot-starter | Druid 数据源 |
| pagehelper-spring-boot-starter | 分页插件 |

---

#### 1.3.2 自动配置（Auto-Configuration）

**@SpringBootApplication 注解解析**

```java
@SpringBootApplication
// 等价于以下三个注解的组合：
@SpringBootConfiguration  // 标识为配置类
@ComponentScan           // 组件扫描
@EnableAutoConfiguration  // 启用自动配置
```

**自动配置原理流程**

```
@EnableAutoConfiguration
    ↓
加载 META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports
    ↓
根据 @Conditional 条件筛选配置类
    ↓
满足条件 → 注册 Bean 到容器
不满足 → 跳过该配置
```

**常用条件注解**

| 注解 | 作用 | 示例 |
|------|------|------|
| @ConditionalOnClass | 类路径存在指定类时生效 | @ConditionalOnClass(DataSource.class) |
| @ConditionalOnMissingBean | 容器中不存在指定 Bean 时生效 | @ConditionalOnMissingBean(DataSource.class) |
| @ConditionalOnProperty | 配置文件中存在指定属性时生效 | @ConditionalOnProperty(name="spring.datasource.url") |
| @ConditionalOnWebApplication | Web 环境下生效 | @ConditionalOnWebApplication |

---

#### 1.3.3 内嵌服务器原理

**Tomcat 内嵌机制**

SpringBoot 将 Tomcat 以对象形式集成到应用中：

```java
// 自动配置类：ServletWebServerFactoryAutoConfiguration
@Bean
public TomcatServletWebServerFactory tomcatServletWebServerFactory() {
    return new TomcatServletWebServerFactory();
}
```

**切换内嵌服务器**

**排除 Tomcat**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

**引入 Jetty**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jetty</artifactId>
</dependency>
```

**引入 Undertow**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-undertow</artifactId>
</dependency>
```

---

## 二、SpringBoot 基础配置

### 2.1 配置文件格式

SpringBoot 支持三种配置文件格式：

| 格式 | 文件扩展名 | 优先级 | 特点 |
|------|------------|--------|------|
| Properties | `.properties` | 高 | 传统格式，键值对 |
| YAML | `.yml` | 中 | 层次清晰，推荐使用 |
| YAML | `.yaml` | 低 | 与 yml 相同 |

**优先级规则**：`application.properties > application.yml > application.yaml`

> **⚠️ 注意**：不同格式的配置文件中相同属性会按优先级覆盖，不同属性全部保留。

---

### 2.2 YAML 语法详解

#### 2.2.1 基础语法规则

```yaml
# 1. 大小写敏感
server:
  port: 8080

# 2. 使用缩进表示层级（只能用空格，不能用 Tab）
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/db

# 3. 属性值前必须有空格
name: SpringBoot  # ✅ 正确
name:SpringBoot     # ❌ 错误

# 4. # 号表示注释
# 这是注释内容
```

#### 2.2.2 数据类型示例

```yaml
# 基本数据类型
boolean: TRUE
int: 123
float: 3.14
string: HelloWorld
string2: "Hello World"  # 双引号包裹特殊字符
null: ~

# 日期时间
date: 2026-03-16
datetime: 2026-03-16T09:25:40+08:00

# 数组和集合
# 方式一：横线分隔
subject:
  - Java
  - 前端
  - 大数据

# 方式二：中括号
likes: [王者荣耀, 刺激战场]

# 对象
enterprise:
  name: cskt
  age: 16
  subject:
    - Java
    - 前端

# 对象数组
users:
  - name: Tom
    age: 4
  - name: Jerry
    age: 5

# 对象数组缩略格式
users2: [ { name:Tom , age:4 } , { name:Jerry , age:5 } ]
```

---

### 2.3 配置数据读取

#### 2.3.1 读取单一数据

**使用 @Value 注解**

```java
@RestController
public class ConfigController {
    
    @Value("${server.port}")
    private int port;
    
    @Value("${spring.application.name}")
    private String appName;
    
    @GetMapping("/config")
    public String getConfig() {
        return "Port: " + port + ", AppName: " + appName;
    }
}
```

**多层级读取**

```yaml
server:
  servlet:
    context-path: /api
```

```java
@Value("${server.servlet.context-path}")
private String contextPath;
```

---

#### 2.3.2 读取对象数据

**方式一：@ConfigurationProperties（推荐）**

**配置文件**

```yaml
servers:
  ip-address: 192.168.0.1
  port: 2345
  timeout: -1
```

**实体类**

```java
@Component
@Data
@ConfigurationProperties(prefix = "servers")
public class ServerConfig {
    private String ipAddress;
    private int port;
    private long timeout;
}
```

**使用**

```java
@Service
public class ServerService {
    
    @Autowired
    private ServerConfig serverConfig;
    
    public void printConfig() {
        System.out.println(serverConfig.getIpAddress());
    }
}
```

**方式二：@ConfigurationProperties 绑定第三方 Bean**

```java
@Configuration
public class DataSourceConfig {
    
    @Bean
    @ConfigurationProperties(prefix = "spring.datasource.hikari")
    public DataSource dataSource() {
        return new HikariDataSource();
    }
}
```

---

#### 2.3.3 宽松绑定（Relaxed Binding）

**支持的数据格式**

```yaml
server:
  ipAddress: 192.168.0.1      # 驼峰模式
  ip_address: 192.168.0.1     # 下划线模式
  ip-address: 192.168.0.1     # 烤肉串模式（推荐）
  IP_ADDRESS: 192.168.0.1      # 常量模式
```

**Java 属性**

```java
private String ipAddress;  // 上述四种格式都能匹配
```

> **⚠️ 注意**：宽松绑定仅适用于 `@ConfigurationProperties`，不适用于 `@Value`。

---

### 2.4 多环境配置

#### 2.4.1 单文件多环境（不推荐）

```yaml
spring:
  profiles:
    active: dev  # 激活开发环境

---
spring:
  config:
    activate:
      on-profile: dev
server:
  port: 8080

---
spring:
  config:
    activate:
      on-profile: test
server:
  port: 8081

---
spring:
  config:
    activate:
      on-profile: prod
server:
  port: 80
```

#### 2.4.2 多文件多环境（推荐）

**文件结构**

```
resources/
├── application.yml              # 主配置文件
├── application-dev.yml         # 开发环境
├── application-test.yml        # 测试环境
└── application-prod.yml        # 生产环境
```

**application.yml**

```yaml
spring:
  profiles:
    active: dev  # 激活的环境
```

**application-dev.yml**

```yaml
server:
  port: 8080
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/dev_db
```

**application-prod.yml**

```yaml
server:
  port: 80
spring:
  datasource:
    url: jdbc:mysql://prod-server:3306/prod_db
```

#### 2.4.3 环境分组（SpringBoot 2.4+）

**Why：为什么需要环境分组？**

在实际企业级项目中，单一环境配置往往无法满足复杂场景需求。例如：

- **开发环境**需要数据库连接、Redis缓存、邮件服务、文件存储、日志级别为DEBUG
- **测试环境**需要上述所有服务，但数据库和Redis与开发环境隔离
- **生产环境**需要独立的数据库集群、Redis集群、CDN、监控告警、日志级别为INFO

如果每次切换环境都需要手动激活多个配置文件，容易出错且效率低下。环境分组机制解决了这一痛点。

**What：环境分组是什么？**

环境分组是SpringBoot 2.4+引入的多环境配置管理特性。它允许开发者将多个相关配置文件组合成一个逻辑分组，通过激活分组名称一次性加载所有相关配置，而无需逐个指定每个配置文件。

**核心价值**
- **配置集中管理**：将相关联的配置文件组织在一起
- **一键切换环境**：激活分组即可自动加载所有相关配置
- **降低出错风险**：避免漏加载或错误加载配置文件
- **提升维护效率**：新增配置文件时只需加入对应分组

**分组与配置文件的关系**

```
┌─────────────────────────────────────────────────────────┐
│                    环境分组（Group）                      │
├─────────────────────────────────────────────────────────┤
│  dev分组  ──激活──→  devDB.yml + devRedis.yml + devMVC.yml │
│  test分组 ──激活──→  testDB.yml + testRedis.yml + testMVC.yml │
│  prod分组 ──激活──→  prodDB.yml + prodRedis.yml + prodMVC.yml │
└─────────────────────────────────────────────────────────┘
```

**How：如何使用环境分组？**

**步骤一：准备相关配置文件**

在`resources`目录下创建多个配置文件：

```
resources/
├── application.yml                    # 主配置文件
├── application-dev.yml               # 开发环境基础配置
├── application-devDB.yml             # 开发环境数据库配置
├── application-devRedis.yml          # 开发环境Redis配置
├── application-devMVC.yml            # 开发环境MVC配置
├── application-test.yml              # 测试环境基础配置
├── application-testDB.yml            # 测试环境数据库配置
├── application-testRedis.yml         # 测试环境Redis配置
├── application-testMVC.yml           # 测试环境MVC配置
├── application-prod.yml              # 生产环境基础配置
├── application-prodDB.yml            # 生产环境数据库配置
├── application-prodRedis.yml         # 生产环境Redis配置
└── application-prodMVC.yml           # 生产环境MVC配置
```

**步骤二：配置环境分组**

在`application.yml`中定义环境分组：

```yaml
spring:
  profiles:
    active: dev  # 激活dev分组
    
    # 定义环境分组
    group:
      # 开发环境分组：激活dev时，同时加载devDB、devRedis、devMVC三个配置文件
      "dev": devDB, devRedis, devMVC
      
      # 测试环境分组：激活test时，同时加载testDB、testRedis、testMVC三个配置文件
      "test": testDB, testRedis, testMVC
      
      # 生产环境分组：激活prod时，同时加载prodDB、prodRedis、prodMVC三个配置文件
      "prod": prodDB, prodRedis, prodMVC
```

**配置文件命名规则**

分组配置文件遵循以下命名规范：`application-{分组名}.yml`

| 配置文件名 | 所属分组 | 用途 |
|-----------|---------|------|
| application-devDB.yml | dev | 开发环境数据库配置 |
| application-devRedis.yml | dev | 开发环境Redis配置 |
| application-devMVC.yml | dev | 开发环境MVC配置 |
| application-testDB.yml | test | 测试环境数据库配置 |
| application-testRedis.yml | test | 测试环境Redis配置 |
| application-testMVC.yml | test | 测试环境MVC配置 |
| application-prodDB.yml | prod | 生产环境数据库配置 |
| application-prodRedis.yml | prod | 生产环境Redis配置 |
| application-prodMVC.yml | prod | 生产环境MVC配置 |

**步骤三：编写各分组配置文件**

**开发环境数据库配置（application-devDB.yml）**

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/dev_db
    username: dev_user
    password: dev_password
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    show-sql: true  # 开发环境显示SQL
```

**开发环境Redis配置（application-devRedis.yml）**

```yaml
spring:
  data:
    redis:
      host: localhost
      port: 6379
      password: ""
      database: 0
```

**开发环境MVC配置（application-devMVC.yml）**

```yaml
server:
  port: 8080
  servlet:
    context-path: /dev-api

logging:
  level:
    com.example: DEBUG
```

**生产环境数据库配置（application-prodDB.yml）**

```yaml
spring:
  datasource:
    url: jdbc:mysql://prod-db-server:3306/prod_db?useSSL=true
    username: prod_user
    password: ${DB_PASSWORD}  # 使用环境变量
    driver-class-name: com.mysql.cj.jdbc.Driver
    hikari:
      maximum-pool-size: 50
      minimum-idle: 10
  jpa:
    show-sql: false  # 生产环境不显示SQL
```

**生产环境Redis配置（application-prodRedis.yml）**

```yaml
spring:
  data:
    redis:
      host: prod-redis-cluster
      port: 6379
      password: ${REDIS_PASSWORD}  # 使用环境变量
      database: 0
      timeout: 5000
```

**生产环境MVC配置（application-prodMVC.yml）**

```yaml
server:
  port: 80
  servlet:
    context-path: /api

logging:
  level:
    com.example: INFO
  file:
    name: /var/log/application/prod.log
```

**步骤四：激活环境分组**

**方式一：通过配置文件激活（默认环境）**

```yaml
# application.yml
spring:
  profiles:
    active: dev  # 激活dev分组
```

**方式二：通过命令行参数激活**

```bash
# 激活开发环境
java -jar app.jar --spring.profiles.active=dev

# 激活测试环境
java -jar app.jar --spring.profiles.active=test

# 激活生产环境
java -jar app.jar --spring.profiles.active=prod
```

**方式三：通过环境变量激活**

```bash
# Linux/Mac
export SPRING_PROFILES_ACTIVE=prod
java -jar app.jar

# Windows
set SPRING_PROFILES_ACTIVE=prod
java -jar app.jar
```

**方式四：通过IDEA配置激活**

1. 打开 `Run` → `Edit Configurations`
2. 在 `VM options` 或 `Environment variables` 中设置
3. 添加：`spring.profiles.active=prod`

**环境分组加载流程**

```
启动应用
    ↓
读取 application.yml
    ↓
发现 spring.profiles.active=dev
    ↓
查找 dev 分组配置
    ↓
自动加载：
    - application-devDB.yml
    - application-devRedis.yml
    - application-devMVC.yml
    ↓
合并所有配置
    ↓
应用启动完成
```

**实际应用场景示例**

**场景一：本地开发**

```yaml
# application.yml
spring:
  profiles:
    active: dev
```

启动后自动加载：
- `application-dev.yml`（开发环境基础配置）
- `application-devDB.yml`（连接本地MySQL）
- `application-devRedis.yml`（连接本地Redis）
- `application-devMVC.yml`（端口8080，DEBUG日志）

**场景二：测试环境部署**

```bash
# 通过命令行激活测试环境
java -jar app.jar --spring.profiles.active=test
```

自动加载：
- `application-test.yml`（测试环境基础配置）
- `application-testDB.yml`（连接测试数据库）
- `application-testRedis.yml`（连接测试Redis）
- `application-testMVC.yml`（端口8081，INFO日志）

**场景三：生产环境部署**

```bash
# 通过环境变量激活生产环境
export SPRING_PROFILES_ACTIVE=prod
java -jar app.jar
```

自动加载：
- `application-prod.yml`（生产环境基础配置）
- `application-prodDB.yml`（连接生产数据库集群）
- `application-prodRedis.yml`（连接生产Redis集群）
- `application-prodMVC.yml`（端口80，INFO日志，日志文件输出）

**环境分组最佳实践**

**1. 分组命名规范**

```yaml
group:
  # 环境分组：使用环境名称前缀
  "dev": devDB, devRedis, devMVC
  "test": testDB, testRedis, testMVC
  "staging": stagingDB, stagingRedis, stagingMVC
  "prod": prodDB, prodRedis, prodMVC
  
  # 功能分组：按业务模块组织
  "ecommerce": ecommerceDB, ecommerceCache, ecommercePayment
  "social": socialDB, socialCache, socialNotification
```

**2. 配置文件职责划分**

| 配置文件类型 | 命名规范 | 职责描述 |
|-------------|---------|---------|
| 基础配置 | `application-{env}.yml` | 环境通用配置（端口、上下文路径、日志级别） |
| 数据库配置 | `application-{env}DB.yml` | 数据库连接、连接池配置、JPA配置 |
| 缓存配置 | `application-{env}Redis.yml` | Redis连接、缓存策略配置 |
| 中间件配置 | `application-{env}{Service}.yml` | 各中间件连接配置（MQ、ES、Kafka等） |
| 业务配置 | `application-{env}{Module}.yml` | 模块化业务配置 |

**3. 配置文件复用技巧**

```yaml
# application-commonDB.yml（公共数据库配置，不激活）
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    hikari:
      minimum-idle: 5
      maximum-pool-size: 20

# application-devDB.yml（开发环境数据库配置）
spring:
  profiles:
    group:
      "dev": commonDB  # 引用公共配置
  datasource:
    url: jdbc:mysql://localhost:3306/dev_db
    username: dev_user
    password: dev_password
```

**4. 环境分组与配置优先级**

配置加载优先级（从高到低）：
1. 命令行参数
2. 环境变量
3. 外部配置文件（按加载顺序）
4. 内部配置文件（按加载顺序）

在同一环境中，后加载的配置会覆盖先加载的同名属性。

```yaml
# 假设加载顺序：
# 1. application-devDB.yml
# 2. application-devRedis.yml
# 3. application-devMVC.yml

# 如果三个文件都配置了 logging.level
# 则 application-devMVC.yml 中的配置最终生效
```

**5. 敏感信息处理**

生产环境配置中使用环境变量或配置中心：

```yaml
# application-prodDB.yml
spring:
  datasource:
    url: jdbc:mysql://${DB_HOST:prod-db-server}:3306/${DB_NAME:prod_db}
    username: ${DB_USERNAME:prod_user}
    password: ${DB_PASSWORD}  # 必须通过环境变量注入
```

**环境分组常见问题**

**Q1：分组配置文件必须以分组名开头吗？**

A：不是必须的，但强烈建议遵循命名规范。配置文件命名规则为 `application-{分组名}.yml`，SpringBoot会自动识别。

**Q2：可以同时激活多个分组吗？**

A：可以。使用逗号分隔多个分组名：

```bash
java -jar app.jar --spring.profiles.active=dev,ecommerce
```

**Q3：分组配置文件之间如何共享配置？**

A：可以创建公共配置文件，然后在各分组中引用：

```yaml
# application-common.yml（公共配置，不激活）
spring:
  jackson:
    time-zone: Asia/Shanghai
    date-format: yyyy-MM-dd HH:mm:ss

# application-dev.yml
spring:
  profiles:
    group:
      "dev": common  # 引用公共配置
  datasource:
    url: jdbc:mysql://localhost:3306/dev_db
```

**Q4：如何查看当前激活的配置文件？**

A：通过Actuator端点查看：

```bash
curl http://localhost:8080/actuator/env
```

或在代码中获取：

```java
@Autowired
private Environment env;

public void printActiveProfiles() {
    String[] activeProfiles = env.getActiveProfiles();
    System.out.println("激活的配置文件：" + Arrays.toString(activeProfiles));
}
```

**Q5：环境分组会影响应用启动性能吗？**

A：影响微乎其微。分组只是配置文件的逻辑组织方式，SpringBoot在启动时只会加载激活分组对应的配置文件，不会加载其他分组的配置。

---

## 三、SpringBoot 开发实战

### 3.1 整合 JUnit

#### 3.1.1 传统 Spring 整合 JUnit

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class UserServiceTest {
    
    @Autowired
    private UserService userService;
    
    @Test
    public void testFindById() {
        User user = userService.findById(1L);
        assertNotNull(user);
    }
}
```

**缺点分析**
- 需要指定运行器 `@RunWith`
- 需要手动指定配置类 `@ContextConfiguration`

#### 3.1.2 SpringBoot 整合 JUnit（推荐）

**基本用法**

```java
@SpringBootTest
class UserServiceTest {
    
    @Autowired
    private UserService userService;
    
    @Test
    void testFindById() {
        User user = userService.findById(1L);
        assertNotNull(user);
    }
}
```

**指定引导类**

```java
@SpringBootTest(classes = Application.class)
class UserServiceTest {
    // ...
}
```

**测试环境配置**

```yaml
# application-test.yml
spring:
  datasource:
    url: jdbc:h2:mem:testdb
```

```java
@SpringBootTest
@ActiveProfiles("test")
class UserServiceTest {
    // ...
}
```

---

### 3.2 整合 MyBatis

#### 3.2.1 添加依赖

```xml
<dependencies>
    <!-- MyBatis Starter -->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>3.0.3</version>
    </dependency>
    
    <!-- MySQL 驱动 -->
    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <scope>runtime</scope>
    </dependency>
    
    <!-- 分页插件 -->
    <dependency>
        <groupId>com.github.pagehelper</groupId>
        <artifactId>pagehelper-spring-boot-starter</artifactId>
        <version>2.1.0</version>
    </dependency>
</dependencies>
```

#### 3.2.2 配置文件

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=Asia/Shanghai
    username: root
    password: root

mybatis:
  # Mapper XML 文件位置
  mapper-locations: classpath:mapper/*.xml
  # 实体类别名包
  type-aliases-package: com.example.entity
  configuration:
    # 下划线转驼峰
    map-underscore-to-camel-case: true
    # 日志输出
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl

# 分页插件配置
pagehelper:
  helper-dialect: mysql
  reasonable: true
  support-methods-arguments: true
```

#### 3.2.3 Mapper 接口使用方式

**方式一：XML 配置版（适用于复杂查询）**

数据库初始化脚本：

```sql
资料中有提供SQL脚本
```

实体类定义：

```java
资料中有提供实体类
```

Mapper 接口：

```java
@Mapper
public interface NewsMapper {
    
    News findById(Integer id);
    
    List<News> findAll();
    
    List<News> findByStatus(Integer status);
    
    List<News> findByAuthor(String author);
    
    List<News> search(String keyword);
    
    int insert(News news);
    
    int update(News news);
    
    int delete(Integer id);
    
    int incrementViewCount(Integer id);
    
    int count();
    
    int countByStatus(Integer status);
}
```

NewsMapper.xml：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.mapper.NewsMapper">
    
    <select id="findById" resultType="News">
        SELECT * FROM news WHERE id = #{id}
    </select>
    
    <select id="findAll" resultType="News">
        SELECT * FROM news ORDER BY create_time DESC
    </select>
    
    <select id="findByStatus" resultType="News">
        SELECT * FROM news WHERE status = #{status} ORDER BY create_time DESC
    </select>
    
    <select id="findByAuthor" resultType="News">
        SELECT * FROM news WHERE author = #{author} ORDER BY create_time DESC
    </select>
    
    <select id="search" resultType="News">
        SELECT * FROM news 
        WHERE title LIKE CONCAT('%', #{keyword}, '%') 
           OR content LIKE CONCAT('%', #{keyword}, '%') 
        ORDER BY create_time DESC
    </select>
    
    <insert id="insert" parameterType="News" useGeneratedKeys="true" keyProperty="id">
        INSERT INTO news(title, content, author, cover_image, view_count, status) 
        VALUES(#{title}, #{content}, #{author}, #{coverImage}, #{viewCount}, #{status})
    </insert>
    
    <update id="update" parameterType="News">
        UPDATE news SET 
            title = #{title}, 
            content = #{content}, 
            author = #{author}, 
            cover_image = #{coverImage}, 
            status = #{status} 
        WHERE id = #{id}
    </update>
    
    <delete id="delete">
        DELETE FROM news WHERE id = #{id}
    </delete>
    
    <update id="incrementViewCount">
        UPDATE news SET view_count = view_count + 1 WHERE id = #{id}
    </update>
    
    <select id="count" resultType="int">
        SELECT COUNT(*) FROM news
    </select>
    
    <select id="countByStatus" resultType="int">
        SELECT COUNT(*) FROM news WHERE status = #{status}
    </select>
</mapper>
```

**适用场景**：
- SQL语句复杂，需要动态SQL
- 需要使用MyBatis的高级特性（如关联查询、复杂条件判断）
- SQL语句较长，需要单独维护
- 团队习惯使用XML配置方式

**方式二：注解版（适用于简单查询）**

Mapper 接口（使用注解方式）：

```java
package com.example.mapper;

import com.example.entity.News;
import org.apache.ibatis.annotations.*;

import java.util.List;

@Mapper
public interface NewsMapper {

    @Select("SELECT * FROM news WHERE id = #{id}")
    News findById(Integer id);

    @Select("SELECT * FROM news ORDER BY create_time DESC")
    List<News> findAll();

    @Select("SELECT * FROM news WHERE status = #{status} ORDER BY create_time DESC")
    List<News> findByStatus(Integer status);

    @Select("SELECT * FROM news WHERE author = #{author} ORDER BY create_time DESC")
    List<News> findByAuthor(String author);

    @Select("SELECT * FROM news WHERE title LIKE CONCAT('%', #{keyword}, '%') " +
            "OR content LIKE CONCAT('%', #{keyword}, '%') " +
            "ORDER BY create_time DESC")
    List<News> search(String keyword);

    @Insert("INSERT INTO news(title, content, author, cover_image, view_count, status) " +
            "VALUES(#{title}, #{content}, #{author}, #{coverImage}, #{viewCount}, #{status})")
    @Options(useGeneratedKeys = true, keyProperty = "id")
    int insert(News news);

    @Update("UPDATE news SET title = #{title}, content = #{content}, " +
            "author = #{author}, cover_image = #{coverImage}, status = #{status} " +
            "WHERE id = #{id}")
    int update(News news);

    @Delete("DELETE FROM news WHERE id = #{id}")
    int delete(Integer id);

    @Update("UPDATE news SET view_count = view_count + 1 WHERE id = #{id}")
    int incrementViewCount(Integer id);

    @Select("SELECT COUNT(*) FROM news")
    int count();

    @Select("SELECT COUNT(*) FROM news WHERE status = #{status}")
    int countByStatus(Integer status);
}
```

**适用场景**：
- SQL语句简单，不需要复杂逻辑
- 快速开发，减少XML文件配置
- SQL语句较短，维护成本较低
- 团队习惯使用注解方式

**两种方式对比**

| 对比维度 | XML配置版 | 注解版 |
|---------|----------|--------|
| SQL复杂度 | 适合复杂SQL | 适合简单SQL |
| 维护性 | XML单独维护，清晰度高 | SQL和代码在一起，维护较集中 |
| 可读性 | SQL独立，易于阅读 | SQL混在代码中，可能影响可读性 |
| 动态SQL | 支持，功能强大 | 支持，但配置稍显复杂 |
| 开发效率 | 需要创建XML文件，稍慢 | 直接写注解，快速开发 |
| 团队协作 | XML文件可单独维护 | 接口和SQL在一起 |

### 3.3 整合 Druid

#### 3.3.1 添加依赖

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.20</version>
</dependency>
```

#### 3.3.2 配置文件

```yaml
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    druid:
      # 基础配置
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/db
      username: root
      password: root
      
      # 连接池配置
      initial-size: 5
      min-idle: 5
      max-active: 20
      max-wait: 60000
      
      # 监控配置
      stat-view-servlet:
        enabled: true
        url-pattern: /druid/*
        login-username: admin
        login-password: admin
      
      # 过滤配置
      filter:
        stat:
          log-slow-sql: true
          slow-sql-millis: 2000
        wall:
          enabled: true
```

#### 3.3.3 访问监控页面

访问地址：`http://localhost:8080/druid/index.html`

---

### 3.4 Web 开发与 Thymeleaf

#### Thymeleaf

##### Thymeleaf 介绍

Thymeleaf 是一个现代化的服务器端 Java 模板引擎，专为 Web 和独立环境设计。它能够处理 HTML、XML、JavaScript、CSS 甚至纯文本。

**核心特性**

- **自然模板**：模板文件在浏览器中可直接预览，无需服务器渲染
- **Spring 深度集成**：完美支持 Spring 表达式语言（SpEL）
- **Web 标准化**：完全符合 HTML5 标准
- **扩展性强**：支持自定义方言和处理器

**适用场景**

- 传统服务器端渲染应用（SSR）
- 需要 SEO 优化的项目
- 前端开发人员需要独立预览模板的场景

---

##### 基本使用

**步骤一：添加依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

**步骤二：创建模板文件**

默认模板文件位置：`src/main/resources/templates/`

创建 `index.html` 文件：

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Thymeleaf 入门示例</title>
</head>
<body>
    <h1 th:text="${title}">默认标题</h1>
    <p th:text="${message}">默认消息内容</p>
</body>
</html>
```

**步骤三：创建 Controller**

```java
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HelloController {
    
    @GetMapping("/")
    public String index(Model model) {
        model.addAttribute("title", "Thymeleaf 欢迎页面");
        model.addAttribute("message", "Hello Thymeleaf!");
        return "index";
    }
}
```

**运行效果**

访问 `http://localhost:8080/`，页面显示：

```
Thymeleaf 欢迎页面
Hello Thymeleaf!
```

---

##### 静态资源处理

**默认静态资源目录**

SpringBoot 默认从以下位置加载静态资源（优先级从高到低）：

```
src/main/resources/
├── classpath:/META-INF/resources/
├── classpath:/resources/
├── classpath:/static/
└── classpath:/public/
```

**静态资源访问规则**

- 访问路径：`http://localhost:8080/{文件名}`
- 示例：在 `src/main/resources/static/css/style.css` 创建文件
- 访问地址：`http://localhost:8080/css/style.css`

**自定义静态资源路径**

```yaml
spring:
  web:
    resources:
      static-locations: classpath:/custom-static/, classpath:/static/
```

**在 Thymeleaf 中引用静态资源**

```html
<link rel="stylesheet" th:href="@{/css/style.css}">
<img th:src="@{/images/logo.png}" alt="Logo">
<script th:src="@{/js/app.js}"></script>
```

##### 路径处理

**相对路径与绝对路径**

```html
<!-- 绝对路径 -->
<a th:href="@{/user/list}">用户列表</a>

<!-- 相对路径（相对于当前 URL） -->
<a th:if="${prevPage}" th:href="@{list}">列表页</a>
```

**路径参数传递**

```html
<!-- 单个参数 -->
<a th:href="@{/user(id=1)}">用户1</a>

<!-- 多个参数 -->
<a th:href="@{/search(keyword='SpringBoot', page=1)}">搜索</a>

<!-- 使用变量 -->
<a th:href="@{/user/detail(id=${user.id})}">查看</a>

<!-- 携带锚点 -->
<a th:href="@{/section#top}">回到顶部</a>
```

**RESTful 路径拼接**

```html
<!-- 路径变量 -->
<a th:href="@{/api/users/{id}/orders(id=${user.id})}">订单列表</a>

<!-- 多段路径拼接 -->
<a th:href="@{/api/v1/departments/{deptId}/users/{userId}(deptId=${dept.id}, userId=${user.id})}">
    部门用户
</a>
```

---

##### 页面逻辑处理

**条件判断**

**th:if 和 th:unless**

```html
<!-- th:if：条件成立时渲染 -->
<div th:if="${user.age >= 18}">
    <p>已成年</p>
</div>

<!-- th:unless：条件不成立时渲染 -->
<div th:unless="${user.age >= 18}">
    <p>未成年</p>
</div>
```

**多条件判断**

```html
<!-- 逻辑与 -->
<div th:if="${user != null and user.age >= 18}">
    <p>用户存在且已成年</p>
</div>

<!-- 三元表达式（Thymeleaf 3.1+ 简化语法） -->
<p th:text="${user.vip ? 'VIP用户' : '普通用户'}"></p>

<!-- Elvis 运算符 -->
<p th:text="${user.nickname ?: '匿名用户'}"></p>
```

**switch-case 语句**

```html
<div th:switch="${user.level}">
    <p th:case="'gold'">金牌会员</p>
    <p th:case="'silver'">银牌会员</p>
    <p th:case="'bronze'">铜牌会员</p>
    <p th:case="*">普通会员</p>
</div>
```

**复杂条件判断场景**

```html
<!-- 判断集合是否为空 -->
<div th:if="${users != null and !#lists.isEmpty(users)}">
    <p>共有 <span th:text="${users.size()}"></span> 个用户</p>
</div>

<!-- 判断字符串是否为空 -->
<div th:if="${#strings.isEmpty(message)}">
    <p>暂无消息</p>
</div>

<!-- 判断权限 -->
<div th:if="${#authorization.expression('hasRole(\"ADMIN\")')}">
    <a href="/admin">管理后台</a>
</div>

<!-- 嵌套条件 -->
<div th:if="${user}">
    <div th:if="${user.vip and user.level == 'gold'}">
        <p>金牌 VIP 用户</p>
    </div>
</div>
```

---

##### 数据迭代处理

**基础遍历**

```html
<ul>
    <li th:each="user : ${users}">
        <span th:text="${user.name}"></span>
    </li>
</ul>
```

**迭代状态变量（Thymeleaf 3.1+ 简化）**

```html
<table>
    <tr th:each="user, status : ${users}">
        <td th:text="${status.index}">序号（从0开始）</td>
        <td th:text="${status.count}">计数（从1开始）</td>
        <td th:text="${user.id}">用户ID</td>
        <td th:text="${user.name}">用户名</td>
        <td th:if="${status.first}" class="first-row">第一行</td>
        <td th:if="${status.last}" class="last-row">最后一行</td>
        <td th:if="${status.odd}" class="odd-row">奇数行</td>
        <td th:if="${status.even}" class="even-row">偶数行</td>
    </tr>
</table>
```

**迭代状态变量属性表**

| 属性 | 类型 | 说明 |
|------|------|------|
| index | int | 当前迭代索引，从 0 开始 |
| count | int | 当前迭代计数，从 1 开始 |
| size | int | 集合总大小 |
| current | Object | 当前迭代对象 |
| first | boolean | 是否为第一个元素 |
| last | boolean | 是否为最后一个元素 |
| even | boolean | 当前索引是否为偶数 |
| odd | boolean | 当前索引是否为奇数 |

**嵌套迭代**

```html
<div th:each="dept : ${departments}">
    <h3 th:text="${dept.name}">部门名称</h3>
    <ul>
        <li th:each="user : ${dept.users}" th:text="${user.name}"></li>
    </ul>
</div>
```

**Map 遍历**

```html
<!-- 遍历 Map 条目 -->
<div th:each="entry : ${userMap}">
    <p>Key: <span th:text="${entry.key}"></span>, 
       Value: <span th:text="${entry.value.name}"></span></p>
</div>

<!-- 遍历 Map 的 Key -->
<div th:each="key : ${#maps.keySet(userMap)}">
    <span th:text="${key}"></span>
</div>

<!-- 遍历 Map 的 Value -->
<div th:each="value : ${#maps.values(userMap)}">
    <span th:text="${value.name}"></span>
</div>
```

**数组遍历**

```html
<div th:each="item : ${items}">
    <span th:text="${item}"></span>
</div>

<!-- 数组长度 -->
<p th:text="${#arrays.length(items)}"></p>

<!-- 数组包含判断 -->
<div th:if="${#arrays.contains(items, 'target')}">
    包含目标项
</div>
```

---

3.4.7 包含指令

**th:insert、th:replace 和 th:include 对比**

**定义公共片段（common/header.html）**

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
</head>
<body>

    <!-- 定义导航片段 -->
    <div th:fragment="header">
        <nav class="main-nav">
            <a href="/">首页</a>
            <a href="/user">用户</a>
            <a href="/about">关于</a>
        </nav>
    </div>

    <!-- 定义页脚片段 -->
    <div th:fragment="footer">
        <p>&copy; 2026 SpringBoot 示例</p>
    </div>

</body>
</html>
```

**th:insert：插入整个片段**

```html
<!-- 插入片段（保留外层 div 标签） -->
<div th:insert="~{common/header :: header}"></div>

<!-- 渲染结果 -->
<div>
    <nav class="main-nav">
        <a href="/">首页</a>
        <a href="/user">用户</a>
        <a href="/about">关于</a>
    </nav>
</div>
```

**th:replace：替换当前标签**

```html
<!-- 替换当前 div 标签为片段内容 -->
<div th:replace="~{common/header :: header}"></div>

<!-- 渲染结果 -->
<nav class="main-nav">
    <a href="/">首页</a>
    <a href="/user">用户</a>
    <a href="/about">关于</a>
</nav>
```

**th:include：仅插入片段内容**

```html
<!-- 只插入 fragment 内部内容 -->
<div th:include="~{common/header :: header}"></div>

<!-- 渲染结果 -->
<div>
    <nav class="main-nav">
        <a href="/">首页</a>
        <a href="/user">用户</a>
        <a href="/about">关于</a>
    </nav>
</div>
```

**三种指令对比**

| 指令 | 行为 | 使用场景 | 结果示例 |
|------|------|----------|----------|
| th:insert | 插入整个片段 | 保留外层标签结构 | `<div><nav>...</nav></div>` |
| th:replace | 替换当前标签 | 直接替换为目标片段 | `<nav>...</nav>` |
| th:include | 仅插入片段内容 | 只需要片段内部内容 | `<div><nav>...</nav></div>` |

**片段选择器**

```html
<!-- 选择指定文件中的片段 -->
<div th:insert="~{common/header :: header}"></div>

<!-- 使用 CSS 选择器 -->
<div th:insert="~{common/header :: div.nav}"></div>

<!-- 使用 ID 选择器 -->
<div th:insert="~{common/header :: #main-nav}"></div>

<!-- 使用标签选择器 -->
<div th:insert="~{common/header :: nav}"></div>
```

**传递参数给片段**

```html
<!-- 片段定义（common/header.html） -->
<div th:fragment="header(title1, title2)">
    <nav>
        <a href="/"><span th:text="${title1}">首页</span></a>
        <a href="/user"><span th:text="${title2}">用户</span></a>
    </nav>
</div>

<!-- 调用片段并传递参数 -->
<div th:insert="~{common/header :: header('首页', '用户')}"></div>

<!-- 使用变量传递参数 -->
<div th:insert="~{common/header :: header(navTitle1, navTitle2)}"></div>
```

**片段复用完整示例**

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>页面标题</title>
</head>
<body>

    <!-- 使用 th:replace 替换整个标签 -->
    <header th:replace="~{common/header :: header}"></header>

    <!-- 主内容区域 -->
    <main class="container">
        <h1>页面内容</h1>
    </main>

    <!-- 使用 th:insert 保留外层标签 -->
    <footer th:insert="~{common/footer :: footer}"></footer>

</body>
</html>
```

---

##### Thymeleaf 数据处理

**1、thymeleaf 属性**

**文本内容处理**

```html
<!-- th:text：设置元素文本内容（自动转义 HTML 特殊字符） -->
<p th:text="${user.name}">默认文本</p>

<!-- 示例：自动转义 -->
<p th:text="${'<script>alert(1)</script>'}">
    &lt;script&gt;alert(1)&lt;/script&gt;
</p>
```

**非转义文本处理**

```html
<!-- th:utext：不转义 HTML 标签 -->
<div th:utext="${htmlContent}"></div>

<!-- 示例：保留 HTML 标签 -->
<div th:utext="'<p><b>粗体</b>文本</p>'"></div>
<!-- 渲染结果 -->
<div>
    <p><b>粗体</b>文本</p>
</div>
```

**表单元素值设置**

```html
<!-- 文本输入框 -->
<input type="text" th:value="${user.name}" placeholder="请输入用户名">

<!-- 单选框 -->
<input type="radio" name="gender" value="male" th:checked="${user.gender == 'male'}">
<input type="radio" name="gender" value="female" th:checked="${user.gender == 'female'}">

<!-- 复选框 -->
<input type="checkbox" name="hobbies" value="reading" th:checked="${#lists.contains(user.hobbies, 'reading')}">
<input type="checkbox" name="hobbies" value="music" th:checked="${#lists.contains(user.hobbies, 'music')}">

<!-- 下拉选择框 -->
<select name="city">
    <option value="">请选择城市</option>
    <option th:each="city : ${cities}" 
            th:value="${city.code}" 
            th:text="${city.name}"
            th:selected="${user.city == city.code}">
        城市
    </option>
</select>

<!-- 文本域 -->
<textarea th:text="${user.description}"></textarea>
```

**通用属性设置（th:attr）**

```html
<!-- 设置单个属性 -->
<input th:attr="placeholder=${placeholder}" />

<!-- 设置多个属性（逗号分隔） -->
<input th:attr="type='text', placeholder='请输入用户名', required=true" />

<!-- 动态属性名 -->
<div th:attr="data-id=${user.id}"></div>
<div th:attr="data-username=${user.name}"></div>

<!-- 组合使用 -->
<a th:attr="data-toggle='modal', data-target='#userModal'">
    打开模态框
</a>
```

**常用属性快捷方式**

| Thymeleaf 属性 | 等价的 HTML 属性 | 示例 |
|----------------|------------------|------|
| th:src | src | `<img th:src="@{/images/logo.png}">` |
| th:href | href | `<a th:href="@{/user/detail}">详情</a>` |
| th:class | class | `<div th:class="'active ' + extraClass">` |
| th:id | id | `<div th:id="'user-' + ${user.id}">` |
| th:action | action | `<form th:action="@{/login}">` |
| th:method | method | `<form th:method="post">` |

**样式设置（Thymeleaf 3.1+ 简化语法）**

```html
<!-- 动态样式 -->
<div th:style="'background-color:' + ${user.color}">内容</div>

<!-- 多个样式（使用逗号分隔） -->
<div th:style="'color:' + ${textColor} + '; font-size:' + ${fontSize} + 'px'">内容</div>

<!-- 使用对象 -->
<div th:style="${customStyle}">内容</div>

<!-- 响应式样式 -->
<div th:style="${isDark ? 'background:#333; color:#fff' : 'background:#fff; color:#333'}">
    响应式内容
</div>
```

**类名追加（Thymeleaf 3.1+ 简化）**

```html
<!-- th:classappend：追加类名 -->
<div class="box" th:classappend="${active ? 'active' : ''}">
    内容
</div>
<!-- 结果：active 为 true 时，class="box active" -->

<!-- 多条件类名 -->
<div class="btn" th:classappend="${isPrimary ? 'btn-primary' : ''} ${isDisabled ? 'btn-disabled' : ''}">
    按钮
</div>
<!-- 结果：isPrimary=true, isDisabled=true 时，class="btn btn-primary btn-disabled" -->

<!-- 响应式类名 -->
<div class="card" th:classappend="${size == 'large' ? 'card-large' : 'card-small'}">
    卡片
</div>
```

**属性值追加（Thymeleaf 3.1+ 简化）**

```html
<!-- th:attrappend：追加属性值 -->
<button th:attrappend="onclick=${',alert(1)'}">点击</button>
<!-- 结果：onclick="alert(1),alert(1)" -->

<!-- th:attrprepend：在属性值前追加 -->
<input th:attrprepend="placeholder=${'请输入'}">
<!-- 结果：placeholder="请输入用户名" -->

<!-- 实际应用 -->
<a th:attrappend="href=${'?page=' + currentPage}">下一页</a>
<!-- 结果：href="/user/list?page=2" -->
```

---

**2、工具类**

Thymeleaf 内置了丰富的工具类，通过 `#` 访问：

**#strings（字符串工具）**

```html
<!-- 字符串判断 -->
<div th:if="${#strings.isEmpty(message)}">消息为空</div>
<div th:if="${!#strings.isEmpty(message)}">消息不为空</div>

<!-- 字符串包含 -->
<div th:if="${#strings.contains(name, 'admin')}">包含 admin</div>

<!-- 字符串前缀/后缀判断 -->
<div th:if="${#strings.startsWith(name, 'user_')}">以 user_ 开头</div>
<div th:if="${#strings.endsWith(name, '@example.com')}">以 @example.com 结尾</div>

<!-- 字符串截取 -->
<p th:text="${#strings.substring(description, 0, 50)}"></p>
<p th:text="${#strings.substringAfter(email, '@')}"></p>

<!-- 字符串长度 -->
<p th:text="'长度：' + ${#strings.length(name)}"></p>

<!-- 大小写转换 -->
<p th:text="${#strings.toUpperCase(name)}"></p>
<p th:text="${#strings.toLowerCase(name)}"></p>

<!-- 首字母大写 -->
<p th:text="${#strings.capitalize(name)}"></p>
<p th:text="${#strings.uncapitalize(name)}"></p>

<!-- 字符串替换 -->
<p th:text="${#strings.replace(text, 'old', 'new')}"></p>

<!-- 字符串拼接 -->
<p th:text="${#strings.arrayPreFix(items, 'Item: ')}"></p>

<!-- 缩略文本 -->
<p th:text="${#strings.abbreviate(longText, 50)}"></p>
```

**#numbers（数字工具）**

```html
<!-- 格式化数字（小数位数） -->
<p th:text="${#numbers.formatDecimal(price, 1, 2)}"></p>
<!-- 输出：1,234.56 -->

<!-- 格式化货币 -->
<p th:text="${#numbers.formatCurrency(price)}"></p>
<!-- 输出：￥1,234.56 -->

<!-- 格式化百分比 -->
<p th:text="${#numbers.formatPercent(rate, 0, 2)}"></p>
<!-- 输出：25.50% -->

<!-- 整数序列 -->
<div th:each="i : ${#numbers.sequence(1, 10)}">
    <span th:text="${i}"></span>
</div>
<!-- 输出：1 2 3 4 5 6 7 8 9 10 -->

<!-- 指定步长 -->
<div th:each="i : ${#numbers.sequence(1, 10, 2)}">
    <span th:text="${i}"></span>
</div>
<!-- 输出：1 3 5 7 9 -->

<!-- 数组序列 -->
<div th:each="i : ${#numbers.sequence(0, ${#arrays.length(items)} - 1)}">
    <span th:text="${i}"></span>
</div>

<!-- 最小值、最大值 -->
<p th:text="${#numbers.min(5, 10)}"></p> <!-- 输出：5 -->
<p th:text="${#numbers.max(5, 10)}"></p> <!-- 输出：10 -->

<!-- 四舍五入 -->
<p th:text="${#numbers.formatInteger(3.14159, 0)}"></p> <!-- 输出：3 -->

<!-- 科学计数法 -->
<p th:text="${#numbers.formatScientific(1000000)}"></p> <!-- 输出：1E+6 -->
```

**#dates（日期工具）**

```html
<!-- 格式化日期 -->
<p th:text="${#dates.format(now, 'yyyy-MM-dd')}"></p>
<p th:text="${#dates.format(now, 'yyyy-MM-dd HH:mm:ss')}"></p>
<p th:text="${#dates.format(now, 'MM/dd/yyyy')}"></p>

<!-- 获取日期组成部分 -->
<p th:text="${#dates.year(now)}年"></p>
<p th:text="${#dates.month(now)}月"></p>
<p th:text="${#dates.day(now)}日"></p>
<p th:text="${#dates.dayOfWeek(now)}">星期</p>
<p th:text="${#dates.hour(now)}时"></p>
<p th:text="${#dates.minute(now)}分"></p>
<p th:text="${#dates.second(now)}秒"></p>

<!-- 创建日期 -->
<p th:text="${#dates.createNow()}"></p>
<p th:text="${#dates.createToday()}"></p>
<p th:text="${#dates.create(2026, 3, 16)}"></p>
<p th:text="${#dates.create(2026, 3, 16, 9, 25, 40)}"></p>

<!-- 日期运算 -->
<p th:text="${#dates.addDays(now, 7)}"></p> <!-- 加7天 -->
<p th:text="${#dates.addMonths(now, 1)}"></p> <!-- 加1月 -->
<p th:text="${#dates.addYears(now, 1)}"></p> <!-- 加1年 -->

<!-- 日期比较 -->
<div th:if="${#dates.isAfter(now, deadline)}">已过期</div>
<div th:if="${#dates.isBefore(now, deadline)}">未到期</div>
```

**#calendars（日历工具）**

```html
<!-- 与 #dates 类似，但操作 Calendar 对象 -->
<p th:text="${#calendars.format(calendar, 'yyyy-MM-dd')}"></p>
<p th:text="${#calendars.year(calendar)}年"></p>
<p th:text="${#calendars.month(calendar)}月"></p>
<p th:text="${#calendars.day(calendar)}日"></p>
```

**#lists（列表工具）**

```html
<!-- 判断列表是否为空 -->
<div th:if="${#lists.isEmpty(users)}">用户列表为空</div>
<div th:if="${!#lists.isEmpty(users)}">用户列表不为空</div>

<!-- 获取列表大小 -->
<p th:text="'共' + ${#lists.size(users)} + '条记录'"></p>

<!-- 获取指定索引元素 -->
<p th:text="${#lists.get(users, 0)}"></p>

<!-- 判断列表包含元素 -->
<div th:if="${#lists.contains(users, targetUser)}">包含目标用户</div>

<!-- 排序 -->
<p th:text="${#lists.sort(users)}"></p>

<!-- 列表转字符串 -->
<p th:text="${#lists.toString(users)}"></p>
```

**#arrays（数组工具）**

```html
<!-- 数组长度 -->
<p th:text="${#arrays.length(array)}"></p>

<!-- 数组包含判断 -->
<div th:if="${#arrays.contains(array, value)}">包含该值</div>

<!-- 数组转列表 -->
<p th:text="${#arrays.toList(array)}"></p>

<!-- 数组转字符串 -->
<p th:text="${#arrays.toString(array)}"></p>
```

**#sets（集合工具）**

```html
<!-- 集合大小 -->
<p th:text="${#sets.size(userSet)}"></p>

<!-- 集合包含判断 -->
<div th:if="${#sets.contains(userSet, user)}">包含该用户</div>

<!-- 集合交集 -->
<p th:text="${#sets.intersection(set1, set2)}"></p>

<!-- 集合并集 -->
<p th:text="${#sets.union(set1, set2)}"></p>

<!-- 集合差集 -->
<p th:text="${#sets.difference(set1, set2)}"></p>
```

**#maps（Map 工具）**

```html
<!-- Map 大小 -->
<p th:text="${#maps.size(userMap)}"></p>

<!-- 判断包含 Key -->
<div th:if="${#maps.containsKey(userMap, 'admin')}">包含 admin</div>

<!-- 判断包含 Value -->
<div th:if="${#maps.containsValue(userMap, adminUser)}">包含管理员用户</div>

<!-- 获取所有 Key -->
<div th:each="key : ${#maps.keySet(userMap)}">
    <span th:text="${key}"></span>
</div>

<!-- 获取所有 Value -->
<div th:each="value : ${#maps.values(userMap)}">
    <span th:text="${value.name}"></span>
</div>
```

**#aggregates（聚合工具）**

```html
<!-- 求和 -->
<p th:text="${#aggregates.sum(numbers)}"></p>
<p th:text="${#aggregates.sum(userList, 'age')}"></p>

<!-- 求平均值 -->
<p th:text="${#aggregates.avg(numbers)}"></p>

<!-- 求最小值 -->
<p th:text="${#aggregates.min(numbers)}"></p>
<p th:text="${#aggregates.min(userList, 'age')}"></p>

<!-- 求最大值 -->
<p th:text="${#aggregates.max(numbers)}"></p>
<p th:text="${#aggregates.max(userList, 'age')}"></p>
```

---

**3、表达式**

**变量表达式 ${}**

```html
<!-- 访问对象属性 -->
<p th:text="${user.name}"></p>

<!-- 访问嵌套属性 -->
<p th:text="${user.address.city}"></p>

<!-- 调用方法（无参数） -->
<p th:text="${user.getName()}"></p>
<p th:text="${user.getFullName()}"></p>

<!-- 调用方法（带参数） -->
<p th:text="${user.calculateAge(1990)}"></p>

<!-- 空安全访问（Thymeleaf 3.1+） -->
<p th:text="${user?.address?.city}"></p>

<!-- Map 访问 -->
<p th:text="${userMap['admin']}"></p>
<p th:text="${userMap.admin}"></p>
```

**选择表达式 *{}（Thymeleaf 3.1+ 简化）**

```html
<!-- 在 th:object 上下文中使用 -->
<div th:object="${user}">
    <p th:text="*{name}"></p>  <!-- 等价于 ${user.name} -->
    <p th:text="*{age}"></p>   <!-- 等价于 ${user.age} -->
    <p th:text="*{email}"></p>  <!-- 等价于 ${user.email} -->
</div>

<!-- 嵌套对象 -->
<div th:object="${user}">
    <p th:text="*{address.city}"></p>
    <p th:text="*{address.country}"></p>
</div>
```

**消息表达式 #{ }（国际化）**

```html
<!-- messages.properties -->
welcome.message=欢迎，{0}！
user.info=用户：{0}，年龄：{1}
greeting=早上好，{0}！现在是{1}

<!-- 模板文件中使用 -->
<p th:text="#{welcome.message(${user.name})}"></p>
<p th:text="#{user.info(${user.name}, ${user.age})}"></p>
<p th:text="#{greeting(user.name, #dates.format(now, 'HH:mm'))}"></p>

<!-- 使用多个参数 -->
<p th:text="#{welcome.message(user.name, user.role)}"></p>

<!-- 使用 key 变量 -->
<p th:text="#{${messageKey}}"></p>
```

**链接表达式 @{ }**

```html
<!-- 绝对路径 -->
<a th:href="@{/user/list}">用户列表</a>

<!-- 相对路径（相对于当前 URL） -->
<a th:if="${prevPage}" th:href="@{list}">列表页</a>

<!-- 带单个参数 -->
<a th:href="@{/user/detail(id=${user.id})}">详情</a>
<a th:href="@{/user/{id}(id=${user.id})}">详情</a>  <!-- RESTful 风格 -->

<!-- 带多个参数 -->
<a th:href="@{/search(keyword=${key}, page=1, size=10)}">搜索</a>

<!-- 带锚点 -->
<a th:href="@{/section#top}">回到顶部</a>
<a th:href="@{/user/faq#question1}">常见问题</a>

<!-- 带协议（绝对 URL） -->
<a th:href="@{https://www.example.com}">外部链接</a>

<!-- 上下文路径自动处理 -->
<a th:href="@{/user/list}">用户列表</a>
<!-- 如果 context-path=/demo，生成：/demo/user/list -->

<!-- 动态参数 -->
<a th:href="@{/user/list(page=${currentPage + 1})}">下一页</a>
```

**字面量**

```html
<!-- 字符串字面量 -->
<p th:text="'Hello World'"></p>
<p th:text="'SpringBoot 3.x'"></p>

<!-- 数字字面量 -->
<p th:text="${100 + 200}"></p>
<p th:text="${3.14 * 2}"></p>
<p th:text="${100 / 3}"></p>

<!-- 布尔字面量 -->
<div th:if="${true}">真</div>
<div th:unless="${false}">非假</div>

<!-- null 字面量 -->
<div th:if="${user == null}">用户为空</div>
<div th:if="${!user}">用户不存在</div>
```

**运算符**

```html
<!-- 算术运算 -->
<p th:text="${1 + 2}"></p>  <!-- 3 -->
<p th:text="${10 - 3}"></p> <!-- 7 -->
<p th:text="${2 * 5}"></p>  <!-- 10 -->
<p th:text="${10 / 2}"></p> <!-- 5 -->
<p th:text="${10 % 3}"></p> <!-- 1 -->

<!-- 比较运算 -->
<div th:if="${age >= 18}">成年</div>
<div th:if="${score > 60}">及格</div>
<div th:if="${status == 'active'}">激活</div>
<div th:if="${role != 'admin'}">普通用户</div>
<div th:if="${score >= 60 and score <= 100}">成绩有效</div>

<!-- 逻辑运算 -->
<div th:if="${user != null and user.age >= 18}">用户存在且成年</div>
<div th:if="${vip or admin}">特权用户</div>
<div th:if="${!hidden}">可见</div>
<div th:if="${!(user == null)}">用户存在</div>

<!-- 三元运算符 -->
<p th:text="${vip ? 'VIP用户' : '普通用户'}"></p>

<!-- Elvis 运算符（简化三元运算符） -->
<p th:text="${name ?: '默认名称'}"></p>
<!-- 等价于 ${name != null ? name : '默认名称'} -->

<!-- Elvis 运算符与表达式结合 -->
<p th:text="${user?.name ?: '匿名用户'}"></p>

<!-- 无操作（No-Operation）运算符 _ -->
<input type="text" th:value="${user.name ?: _}" placeholder="请输入用户名">
```

---

**4、域对象操作**

**request 域**

```java
// Controller 中设置
@GetMapping("/")
public String index(HttpServletRequest request) {
    request.setAttribute("message", "Request 数据");
    request.setAttribute("currentTime", new Date());
    return "index";
}
```

```html
<!-- 模板中访问 -->
<p th:text="${#request.getAttribute('message')}"></p>
<!-- 或直接访问 -->
<p th:text="${message}"></p>

<!-- 获取所有 request 属性 -->
<div th:each="attr : ${#request.getAttributeNames()}">
    <p th:text="${attr}"></p>
</div>
```

**session 域**

```java
// Controller 中设置
@GetMapping("/")
public String index(HttpServletRequest request) {
    request.getSession().setAttribute("currentUser", user);
    request.getSession().setAttribute("loginTime", new Date());
    return "index";
}
```

```html
<!-- 模板中访问 -->
<p th:text="${#session.getAttribute('currentUser')}"></p>
<!-- 或使用简写 -->
<p th:text="${session.currentUser}"></p>

<!-- 获取 session ID -->
<p th:text="${#session.id}"></p>

<!-- 获取所有 session 属性 -->
<div th:each="attr : ${#session.getAttributeNames()}">
    <p th:text="${attr}"></p>
</div>
```

**application 域（ServletContext）**

```java
// Controller 中设置
@GetMapping("/")
public String index(ServletContext context) {
    context.setAttribute("appInfo", "Application 数据");
    context.setAttribute("version", "3.5.0");
    return "index";
}
```

```html
<!-- 模板中访问 -->
<p th:text="${#servletContext.getAttribute('appInfo')}"></p>

<!-- 或使用简写（Thymeleaf 3.1+） -->
<p th:text="${application.appInfo}"></p>
<p th:text="${application.version}"></p>
```

**所有域对象访问方式汇总**

| 域对象 | 访问方式 | 示例 | 简写方式 |
|--------|---------|------|----------|
| Request | `${#request.getAttribute('key')}` 或 `${key}` | `${message}` | 无 |
| Session | `${#session.getAttribute('key')}` 或 `${session.key}` | `${session.currentUser}` | `${session.key}` |
| Application | `${#servletContext.getAttribute('key')}` | `${#servletContext.getAttribute('appInfo')}` | `${application.key}` |

**参数获取**

```html
<!-- 获取 URL 参数 -->
<p th:text="${#request.getParameter('id')}"></p>
<p th:text="${#request.getParameterValues('ids')}"></p>

<!-- 获取所有参数 -->
<div th:each="entry : ${#request.getParameterMap()}">
    <p th:text="${entry.key} + ': ' + ${entry.value}"></p>
</div>

<!-- 获取请求信息 -->
<p th:text="${#request.requestURI}"></p>
<p th:text="${#request.requestURL}"></p>
<p th:text="${#request.contextPath}"></p>
<p th:text="${#request.servletPath}"></p>
```

**Cookie 获取**

```java
// 设置 Cookie
@GetMapping("/")
public String index(HttpServletResponse response) {
    Cookie cookie = new Cookie("theme", "dark");
    cookie.setMaxAge(3600);
    response.addCookie(cookie);
    return "index";
}
```

```html
<!-- 获取 Cookie（直接访问） -->
<p th:text="${#httpServletRequest.getCookies()}"></p>

<!-- 使用工具类获取指定 Cookie -->
<div th:if="${#cookies.getCookie('theme') != null}">
    <p th:text="${#cookies.getCookie('theme').value}"></p>
</div>

<!-- 获取 Cookie 值 -->
<p th:text="${#cookieValue.get('theme')}"></p>

<!-- Cookie 相关判断 -->
<div th:if="${#cookies.getCookie('theme') != null and #cookies.getCookie('theme').value == 'dark'}">
    <p>当前主题：暗色模式</p>
</div>
```

**其他 Servlet 对象访问**

```html
<!-- 获取 Request 对象 -->
<p th:text="${#request.method}"></p>
<p th:text="${#request.remoteAddr}"></p>

<!-- 获取 Session 对象 -->
<p th:text="${#session.creationTime}"></p>
<p th:text="${#session.lastAccessedTime}"></p>
<p th:text="${#session.maxInactiveInterval}"></p>

<!-- 获取 ServletContext 对象 -->
<p th:text="${#servletContext.serverInfo}"></p>
<p th:text="${#servletContext.contextPath}"></p>
```

##### Thymeleaf 案例

**用户列表页面**

**Controller**

```java
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import java.util.Arrays;
import java.util.Date;

@Controller
@RequestMapping("/user")
public class UserController {
    
    @GetMapping("/list")
    public String list(Model model) {
        // 模拟用户数据
        User user1 = new User(1L, "张三", 25, "male", "gold");
        User user2 = new User(2L, "李四", 20, "female", "silver");
        User user3 = new User(3L, "王五", 28, "male", "bronze");
        
        model.addAttribute("users", Arrays.asList(user1, user2, user3));
        model.addAttribute("pageTitle", "用户列表");
        model.addAttribute("now", new Date());
        model.addAttribute("totalUsers", 3);
        model.addAttribute("activeUserCount", 2);
        
        return "user/list";
    }
}
```

**user/list.html（完整模板）**

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title th:text="${pageTitle}">用户列表</title>
    <link rel="stylesheet" th:href="@{/css/style.css}">
</head>
<body>
    <!-- 公共头部 -->
    <header th:replace="~{common/header :: header}"></header>

    <div class="container">
        <!-- 页面标题 -->
        <h1 th:text="${pageTitle}">用户列表</h1>
        
        <!-- 统计信息面板 -->
        <div class="info-panel">
            <div class="info-item">
                <label>当前时间：</label>
                <span th:text="${#dates.format(now, 'yyyy-MM-dd HH:mm:ss')}"></span>
            </div>
            <div class="info-item">
                <label>用户总数：</label>
                <span th:text="${totalUsers}"></span>
            </div>
            <div class="info-item">
                <label>活跃用户：</label>
                <span th:text="${activeUserCount}"></span>
            </div>
        </div>

        <!-- 搜索表单 -->
        <form th:action="@{/user/search}" th:method="get" class="search-form">
            <input type="text" name="keyword" placeholder="搜索用户名">
            <select name="level">
                <option value="">所有等级</option>
                <option value="gold">金牌</option>
                <option value="silver">银牌</option>
                <option value="bronze">铜牌</option>
            </select>
            <button type="submit">搜索</button>
        </form>

        <!-- 用户表格 -->
        <table class="table">
            <thead>
                <tr>
                    <th>序号</th>
                    <th>ID</th>
                    <th>姓名</th>
                    <th>年龄</th>
                    <th>性别</th>
                    <th>等级</th>
                    <th>状态</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody>
                <tr th:each="user, status : ${users}">
                    <td th:text="${status.count}">1</td>
                    <td th:text="${user.id}">1</td>
                    <td th:text="${user.name}">张三</td>
                    <td th:text="${user.age}">25</td>
                    <td th:text="${user.gender == 'male' ? '男' : '女'}">男</td>
                    <td>
                        <span th:if="${user.level == 'gold'}" class="tag gold">金牌</span>
                        <span th:if="${user.level == 'silver'}" class="tag silver">银牌</span>
                        <span th:if="${user.level == 'bronze'}" class="tag bronze">铜牌</span>
                    </td>
                    <td>
                        <span th:if="${status.odd}" class="status odd">奇数行</span>
                        <span th:if="${status.even}" class="status even">偶数行</span>
                    </td>
                    <td>
                        <a th:href="@{/user/detail(id=${user.id})}">详情</a>
                        <a th:href="@{/user/edit(id=${user.id})}">编辑</a>
                        <a th:href="@{/user/delete(id=${user.id})}" 
                           onclick="return confirm('确认删除？')">删除</a>
                    </td>
                </tr>
            </tbody>
        </table>

        <!-- 空状态 -->
        <div th:if="${#lists.isEmpty(users)}" class="empty-state">
            <p>暂无用户数据</p>
            <a th:href="@{/user/create}">添加用户</a>
        </div>

        <!-- 分页 -->
        <div class="pagination" th:if="${!#lists.isEmpty(users)}">
            <a th:href="@{/user/list(page=1)}">首页</a>
            <a th:href="@{/user/list(page=${currentPage - 1})}" 
               th:if="${currentPage > 1}">上一页</a>
            <span th:text="${currentPage}"></span>
            <a th:href="@{/user/list(page=${currentPage + 1})}">下一页</a>
            <a th:href="@{/user/list(page=${totalPages})}">末页</a>
        </div>
    </div>

    <!-- 公共页脚 -->
    <footer th:replace="~{common/footer :: footer}"></footer>

    <!-- JavaScript -->
    <script th:src="@{/js/app.js}"></script>
</body>
</html>
```

#### 拦截器（Interceptor）

**拦截器简介**

拦截器是Spring MVC中的一种机制，用于在请求处理前后进行预处理和后处理。它与过滤器（Filter）的主要区别是拦截器是Spring MVC框架的组件，而过滤器是Servlet规范的一部分。

**拦截器应用场景**

- 用户登录验证和权限控制
- 请求日志记录
- 性能监控
- 跨域处理
- 防止重复提交

**创建拦截器**

```java
package com.example.interceptor;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

/**
 * 登录验证拦截器
 */
@Component
public class LoginInterceptor implements HandlerInterceptor {

    /**
     * 请求处理前执行
     * 返回true：继续执行后续的拦截器和Controller
     * 返回false：中断请求处理，直接返回
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, 
                             Object handler) throws Exception {
        
        // 获取请求路径
        String requestURI = request.getRequestURI();
        
        // 放行登录页面和静态资源
        if (requestURI.contains("/login") || requestURI.contains("/static/")) {
            return true;
        }
        
        // 检查用户是否登录
        Object user = request.getSession().getAttribute("user");
        if (user == null) {
            // 未登录，重定向到登录页
            response.sendRedirect("/login");
            return false;
        }
        
        // 已登录，放行
        return true;
    }

    /**
     * 请求处理后、视图渲染前执行
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, 
                          Object handler, ModelAndView modelAndView) throws Exception {
        
        // 可以在ModelAndView中添加公共数据
        if (modelAndView != null) {
            modelAndView.addObject("currentTime", new java.util.Date());
        }
    }

    /**
     * 请求处理完成后执行（视图渲染后）
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, 
                               Object handler, Exception ex) throws Exception {
        
        // 可以记录请求耗时等
        long startTime = (Long) request.getAttribute("startTime");
        long endTime = System.currentTimeMillis();
        System.out.println("请求耗时：" + (endTime - startTime) + "ms");
    }
}
```

**配置拦截器**

```java
package com.example.config;

import com.example.interceptor.LoginInterceptor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

/**
 * Web MVC 配置类
 */
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    @Autowired
    private LoginInterceptor loginInterceptor;

    /**
     * 注册拦截器
     */
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        
        registry.addInterceptor(loginInterceptor)
                .addPathPatterns("/**")  // 拦截所有路径
                .excludePathPatterns(  // 排除以下路径
                    "/login",           // 登录页
                    "/register",        // 注册页
                    "/static/**",       // 静态资源
                    "/css/**",          // CSS文件
                    "/js/**",           // JS文件
                    "/images/**",       // 图片
                    "/favicon.ico",     // 网站图标
                    "/error"            // 错误页
                );
    }
}
```

**性能监控拦截器示例**

```java
package com.example.interceptor;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;

/**
 * 性能监控拦截器
 */
@Component
public class PerformanceInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, 
                             Object handler) throws Exception {
        
        // 记录请求开始时间
        request.setAttribute("startTime", System.currentTimeMillis());
        return true;
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, 
                               Object handler, Exception ex) throws Exception {
        
        // 计算请求耗时
        Long startTime = (Long) request.getAttribute("startTime");
        if (startTime != null) {
            long endTime = System.currentTimeMillis();
            long executeTime = endTime - startTime;
            
            // 打印请求日志
            System.out.printf("[%s] %s 耗时: %dms%n", 
                request.getMethod(), 
                request.getRequestURI(), 
                executeTime);
            
            // 慢请求告警（超过1秒）
            if (executeTime > 1000) {
                System.out.println("警告：慢请求！" + request.getRequestURI());
            }
        }
    }
}
```

**拦截器链配置**

```java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    @Autowired
    private LoginInterceptor loginInterceptor;

    @Autowired
    private PerformanceInterceptor performanceInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        
        // 拦截器链执行顺序：按注册顺序执行
        
        // 第一个拦截器：性能监控
        registry.addInterceptor(performanceInterceptor)
                .addPathPatterns("/**");
        
        // 第二个拦截器：登录验证
        registry.addInterceptor(loginInterceptor)
                .addPathPatterns("/api/**")  // 只拦截API请求
                .excludePathPatterns("/api/user/login", "/api/user/register");
    }
}
```

**拦截器执行流程**

```
请求到达
    ↓
拦截器1 preHandle() ──返回false?→ 中断
    ↓ (返回true)
拦截器2 preHandle() ──返回false?→ 中断
    ↓ (返回true)
Controller处理
    ↓
拦截器2 postHandle()
    ↓
拦截器1 postHandle()
    ↓
视图渲染
    ↓
拦截器2 afterCompletion()
    ↓
拦截器1 afterCompletion()
    ↓
响应返回
```

**拦截器与过滤器对比**

| 对比项 | 拦截器（Interceptor） | 过滤器（Filter） |
|-------|---------------------|------------------|
| 所属规范 | Spring MVC | Servlet |
| 作用范围 | 仅Spring MVC处理的请求 | 所有请求 |
| 执行时机 | Controller前后 | 请求进入容器后 |
| 访问Spring容器 | 可以 | 不可以 |
| 拦截对象 | Handler（Controller方法） | ServletRequest/ServletResponse |
| 配置方式 | Java配置类 | @WebFilter或web.xml |

---

#### 配置错误页

**错误页面配置方式**

SpringBoot提供了多种错误页配置方式，从简单到复杂依次是：

**方式一：默认错误页**

SpringBoot默认提供了简单的错误页面（/error），显示错误信息。

**方式二：自定义错误页面**

在`src/main/resources/templates/error/`目录下创建错误页面：

```
src/main/resources/templates/error/
├── 404.html      # 页面不存在
├── 500.html      # 服务器错误
├── 403.html      # 禁止访问
└── error.html    # 通用错误页
```

**404.html（页面不存在）**

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>404 - 页面不存在</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 100px;
            background: #f5f5f5;
        }
        h1 {
            font-size: 120px;
            color: #ff6b6b;
            margin: 0;
        }
        p {
            font-size: 18px;
            color: #666;
            margin: 20px 0;
        }
        a {
            color: #007bff;
            text-decoration: none;
        }
        a:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body>
    <h1>404</h1>
    <p>抱歉，您访问的页面不存在</p>
    <p>错误路径：<span th:text="${path}"></span></p>
    <a href="/">返回首页</a>
</body>
</html>
```

**500.html（服务器错误）**

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>500 - 服务器错误</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 100px;
            background: #f5f5f5;
        }
        h1 {
            font-size: 120px;
            color: #ffa500;
            margin: 0;
        }
        .error-info {
            background: white;
            padding: 30px;
            border-radius: 8px;
            margin: 20px auto;
            max-width: 600px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        pre {
            background: #f8f8f8;
            padding: 15px;
            border-radius: 4px;
            text-align: left;
            overflow-x: auto;
        }
    </style>
</head>
<body>
    <h1>500</h1>
    <div class="error-info">
        <p>抱歉，服务器发生错误</p>
        <p>错误信息：<span th:text="${message}"></span></p>
        <div th:if="${exception}">
            <p>异常类型：<span th:text="${exception}"></span></p>
        </div>
        <div th:if="${trace}">
            <p>堆栈跟踪：</p>
            <pre th:text="${trace}"></pre>
        </div>
    </div>
    <a href="/">返回首页</a>
</body>
</html>
```

**403.html（禁止访问）**

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>403 - 禁止访问</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 100px;
            background: #f5f5f5;
        }
        h1 {
            font-size: 120px;
            color: #dc3545;
            margin: 0;
        }
        p {
            font-size: 18px;
            color: #666;
            margin: 20px 0;
        }
        a {
            color: #007bff;
            text-decoration: none;
        }
    </style>
</head>
<body>
    <h1>403</h1>
    <p>抱歉，您没有权限访问此页面</p>
    <p>请求路径：<span th:text="${path}"></span></p>
    <a href="/login">前往登录</a> |
    <a href="/">返回首页</a>
</body>
</html>
```

**error.html（通用错误页）**

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title th:text="${'错误 - ' + status}">错误</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 100px;
            background: #f5f5f5;
        }
        .error-box {
            background: white;
            padding: 40px;
            border-radius: 8px;
            max-width: 500px;
            margin: 0 auto;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .status-code {
            font-size: 80px;
            color: #666;
            margin: 0;
        }
        .message {
            font-size: 20px;
            color: #333;
            margin: 20px 0;
        }
        .timestamp {
            font-size: 14px;
            color: #999;
        }
    </style>
</head>
<body>
    <div class="error-box">
        <h1 class="status-code" th:text="${status}">500</h1>
        <p class="message" th:text="${message}">服务器错误</p>
        <p class="timestamp" th:text="${#temporals.format(timestamp, 'yyyy-MM-dd HH:mm:ss')}"></p>
        <a href="/">返回首页</a>
    </div>
</body>
</html>
```

**方式三：通过配置文件自定义错误页**

```yaml
server:
  error:
    # 是否包含堆栈跟踪信息
    include-exception: false
    include-stacktrace: never
    # 错误路径
    path: /error
    # 是否开启 whitelabel 错误页
    whitelabel:
      enabled: false  # 关闭默认错误页，使用自定义错误页
```

**方式四：自定义ErrorController**

```java
package com.example.controller;

import jakarta.servlet.http.HttpServletRequest;
import org.springframework.boot.web.servlet.error.ErrorController;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * 自定义错误控制器
 */
@Controller
public class CustomErrorController implements ErrorController {

    @RequestMapping("/error")
    public String handleError(HttpServletRequest request, Model model) {
        
        // 获取错误状态码
        Integer statusCode = (Integer) request.getAttribute("jakarta.servlet.error.status_code");
        
        // 获取异常信息
        Exception exception = (Exception) request.getAttribute("jakarta.servlet.error.exception");
        
        // 获取请求路径
        String path = (String) request.getAttribute("jakarta.servlet.error.request_uri");
        
        // 添加到模型
        model.addAttribute("statusCode", statusCode);
        model.addAttribute("exception", exception);
        model.addAttribute("path", path);
        model.addAttribute("timestamp", new java.util.Date());
        
        // 根据状态码返回不同的错误页
        if (statusCode == 404) {
            model.addAttribute("message", "您访问的页面不存在");
            return "error/404";
        } else if (statusCode == 403) {
            model.addAttribute("message", "您没有权限访问此页面");
            return "error/403";
        } else if (statusCode == 500) {
            model.addAttribute("message", "服务器内部错误");
            return "error/500";
        } else {
            model.addAttribute("message", "系统错误");
            return "error/error";
        }
    }
}
```

**错误信息访问对象**

| 对象 | 说明 | 访问方式 |
|------|------|----------|
| status | HTTP状态码 | `${status}` |
| error | 错误信息 | `${error}` |
| exception | 异常对象 | `${exception}` |
| message | 异常消息 | `${message}` |
| trace | 堆栈跟踪 | `${trace}` |
| path | 请求路径 | `${path}` |
| timestamp | 时间戳 | `${timestamp}` |

---

#### 全局异常处理

**全局异常处理简介**

全局异常处理是通过`@ControllerAdvice`和`@ExceptionHandler`注解实现的，可以统一处理Controller中抛出的异常，避免每个Controller都单独处理异常。

**创建全局异常处理器**

```java
package com.example.exception;

import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.servlet.ModelAndView;

import java.util.HashMap;
import java.util.Map;

/**
 * 全局异常处理器
 */
@ControllerAdvice
public class GlobalExceptionHandler {

    /**
     * 处理自定义业务异常
     */
    @ExceptionHandler(BusinessException.class)
    @ResponseBody
    public Map<String, Object> handleBusinessException(BusinessException e) {
        Map<String, Object> result = new HashMap<>();
        result.put("code", e.getCode());
        result.put("message", e.getMessage());
        result.put("timestamp", System.currentTimeMillis());
        return result;
    }

    /**
     * 处理参数校验异常
     */
    @ExceptionHandler(jakarta.validation.ConstraintViolationException.class)
    @ResponseBody
    public Map<String, Object> handleValidationException(jakarta.validation.ConstraintViolationException e) {
        Map<String, Object> result = new HashMap<>();
        result.put("code", 400);
        result.put("message", "参数校验失败：" + e.getMessage());
        result.put("timestamp", System.currentTimeMillis());
        return result;
    }

    /**
     * 处理空指针异常
     */
    @ExceptionHandler(NullPointerException.class)
    @ResponseBody
    public Map<String, Object> handleNullPointerException(NullPointerException e) {
        Map<String, Object> result = new HashMap<>();
        result.put("code", 500);
        result.put("message", "系统错误：空指针异常");
        result.put("timestamp", System.currentTimeMillis());
        // 生产环境不返回详细错误信息
        e.printStackTrace();
        return result;
    }

    /**
     * 处理所有未捕获的异常
     */
    @ExceptionHandler(Exception.class)
    public ModelAndView handleException(Exception e, Model model) {
        ModelAndView mav = new ModelAndView();
        
        // 判断是否为Ajax请求
        // 如果是Ajax请求，返回JSON
        // 如果是普通请求，返回错误页面
        
        mav.addObject("code", 500);
        mav.addObject("message", "系统错误：" + e.getMessage());
        mav.addObject("exception", e.getClass().getSimpleName());
        mav.addObject("timestamp", new java.util.Date());
        
        // 返回错误页面
        mav.setViewName("error/500");
        return mav;
    }
}
```

**自定义业务异常**

```java
package com.example.exception;

/**
 * 自定义业务异常
 */
public class BusinessException extends RuntimeException {

    private Integer code;

    public BusinessException(String message) {
        super(message);
        this.code = 400;
    }

    public BusinessException(Integer code, String message) {
        super(message);
        this.code = code;
    }

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }
}
```

**在Controller中抛出异常**

```java
package com.example.controller;

import com.example.exception.BusinessException;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.HashMap;
import java.util.Map;

@RestController
@RequestMapping("/api/user")
public class UserController {

    /**
     * 根据ID查询用户
     */
    @GetMapping("/{id}")
    public Map<String, Object> getUser(@PathVariable Long id) {
        
        // 模拟用户不存在的情况
        if (id > 1000) {
            throw new BusinessException(404, "用户不存在");
        }
        
        // 返回用户信息
        Map<String, Object> user = new HashMap<>();
        user.put("id", id);
        user.put("name", "张三");
        user.put("age", 25);
        return user;
    }

    /**
     * 模拟空指针异常
     */
    @GetMapping("/error")
    public Map<String, Object> error() {
        String str = null;
        // 这里会抛出空指针异常
        int length = str.length();
        return null;
    }
}
```

**RESTful API全局异常处理器**

```java
package com.example.exception;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.BindException;
import org.springframework.validation.FieldError;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import java.util.HashMap;
import java.util.Map;

/**
 * RESTful API 全局异常处理器
 */
@RestControllerAdvice
public class GlobalApiExceptionHandler {

    /**
     * 处理业务异常
     */
    @ExceptionHandler(BusinessException.class)
    public ResponseEntity<Map<String, Object>> handleBusinessException(BusinessException e) {
        Map<String, Object> result = new HashMap<>();
        result.put("success", false);
        result.put("code", e.getCode());
        result.put("message", e.getMessage());
        result.put("timestamp", System.currentTimeMillis());
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(result);
    }

    /**
     * 处理参数校验异常（@Valid）
     */
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, Object>> handleValidationException(
            MethodArgumentNotValidException e) {
        
        Map<String, Object> result = new HashMap<>();
        result.put("success", false);
        result.put("code", 400);
        result.put("message", "参数校验失败");
        
        // 提取详细错误信息
        Map<String, String> errors = new HashMap<>();
        for (FieldError error : e.getBindingResult().getFieldErrors()) {
            errors.put(error.getField(), error.getDefaultMessage());
        }
        result.put("errors", errors);
        result.put("timestamp", System.currentTimeMillis());
        
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(result);
    }

    /**
     * 处理绑定异常
     */
    @ExceptionHandler(BindException.class)
    public ResponseEntity<Map<String, Object>> handleBindException(BindException e) {
        Map<String, Object> result = new HashMap<>();
        result.put("success", false);
        result.put("code", 400);
        result.put("message", "参数绑定失败");
        
        Map<String, String> errors = new HashMap<>();
        e.getBindingResult().getFieldErrors().forEach(error -> 
            errors.put(error.getField(), error.getDefaultMessage())
        );
        result.put("errors", errors);
        result.put("timestamp", System.currentTimeMillis());
        
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(result);
    }

    /**
     * 处理所有未捕获的异常
     */
    @ExceptionHandler(Exception.class)
    public ResponseEntity<Map<String, Object>> handleException(Exception e) {
        Map<String, Object> result = new HashMap<>();
        result.put("success", false);
        result.put("code", 500);
        result.put("message", "服务器内部错误");
        result.put("timestamp", System.currentTimeMillis());
        
        // 生产环境不返回详细异常信息
        // 开发环境可以记录日志
        e.printStackTrace();
        
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(result);
    }
}
```

**异常处理器执行顺序**

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    // 1. 最具体的异常（优先匹配）
    @ExceptionHandler(NullPointerException.class)
    public String handleNull(NullPointerException e) { ... }

    // 2. 相对具体的异常
    @ExceptionHandler(RuntimeException.class)
    public String handleRuntime(RuntimeException e) { ... }

    // 3. 最通用的异常（兜底）
    @ExceptionHandler(Exception.class)
    public String handleAll(Exception e) { ... }
}
```

**异常处理器注解详解**
@ControllerAdvice 标记全局异常处理器类
@RestControllerAdvice 标记RESTful全局异常处理器（自动添加@ResponseBody） 
@ExceptionHandler 指定处理的异常类型 

---

#### 文件上传

**文件上传配置**

**方式一：配置文件**

```yaml
spring:
  servlet:
    multipart:
      # 是否启用文件上传
      enabled: true
      # 单个文件最大大小
      max-file-size: 10MB
      # 总上传文件最大大小
      max-request-size: 100MB
      # 文件上传的临时目录
      location: ${java.io.tmpdir}
      # 是否延迟解析
      file-size-threshold: 2MB

# 自定义文件上传配置
file:
  upload:
    # 上传文件保存路径
    path: /data/uploads
    # 允许上传的文件类型
    allowed-types: jpg,jpeg,png,gif,pdf,doc,docx
    # 访问URL前缀
    url-prefix: /uploads
```

**方式二：Java配置**

```java
package com.example.config;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.web.servlet.MultipartConfigFactory;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.util.unit.DataSize;

import jakarta.servlet.MultipartConfigElement;

@Configuration
public class FileUploadConfig {

    @Value("${file.upload.path:/data/uploads}")
    private String uploadPath;

    @Bean
    public MultipartConfigElement multipartConfigElement() {
        MultipartConfigFactory factory = new MultipartConfigFactory();
        
        // 设置上传文件保存路径
        factory.setLocation(uploadPath);
        
        // 设置单个文件最大大小
        factory.setMaxFileSize(DataSize.ofMegabytes(10));
        
        // 设置总上传文件最大大小
        factory.setMaxRequestSize(DataSize.ofMegabytes(100));
        
        return factory.createMultipartConfig();
    }
}
```

**文件上传工具类**

```java
package com.example.util;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
import org.springframework.web.multipart.MultipartFile;

import java.io.File;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

/**
 * 文件上传工具类
 */
@Component
public class FileUploadUtil {

    @Value("${file.upload.path:/data/uploads}")
    private String uploadPath;

    @Value("${file.upload.url-prefix:/uploads}")
    private String urlPrefix;

    /**
     * 上传单个文件
     */
    public String uploadFile(MultipartFile file) throws IOException {
        
        // 检查文件是否为空
        if (file.isEmpty()) {
            throw new RuntimeException("上传文件不能为空");
        }
        
        // 获取原始文件名
        String originalFilename = file.getOriginalFilename();
        if (originalFilename == null) {
            throw new RuntimeException("文件名不能为空");
        }
        
        // 检查文件类型
        String fileExtension = getFileExtension(originalFilename);
        if (!isAllowedFileType(fileExtension)) {
            throw new RuntimeException("不支持的文件类型：" + fileExtension);
        }
        
        // 生成新的文件名（UUID + 时间戳）
        String newFileName = generateFileName(fileExtension);
        
        // 创建日期目录（YYYY/MM/DD）
        String datePath = generateDatePath();
        String fullPath = uploadPath + File.separator + datePath;
        Path filePath = Paths.get(fullPath);
        
        // 如果目录不存在，创建目录
        if (!Files.exists(filePath)) {
            Files.createDirectories(filePath);
        }
        
        // 保存文件
        Path targetPath = Paths.get(fullPath, newFileName);
        file.transferTo(targetPath.toFile());
        
        // 返回访问URL
        return urlPrefix + "/" + datePath + "/" + newFileName;
    }

    /**
     * 上传多个文件
     */
    public List<String> uploadFiles(MultipartFile[] files) throws IOException {
        List<String> urls = new ArrayList<>();
        
        for (MultipartFile file : files) {
            String url = uploadFile(file);
            urls.add(url);
        }
        
        return urls;
    }

    /**
     * 删除文件
     */
    public boolean deleteFile(String fileUrl) {
        try {
            // 从URL中提取文件路径
            String relativePath = fileUrl.substring(urlPrefix.length());
            String fullPath = uploadPath + relativePath;
            
            Path filePath = Paths.get(fullPath);
            
            // 删除文件
            if (Files.exists(filePath)) {
                Files.delete(filePath);
                return true;
            }
            
            return false;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 获取文件扩展名
     */
    private String getFileExtension(String filename) {
        int dotIndex = filename.lastIndexOf(".");
        if (dotIndex > 0) {
            return filename.substring(dotIndex + 1).toLowerCase();
        }
        return "";
    }

    /**
     * 检查文件类型是否允许
     */
    private boolean isAllowedFileType(String extension) {
        String[] allowedTypes = {"jpg", "jpeg", "png", "gif", "pdf", "doc", "docx", "txt"};
        for (String type : allowedTypes) {
            if (type.equalsIgnoreCase(extension)) {
                return true;
            }
        }
        return false;
    }

    /**
     * 生成新的文件名
     */
    private String generateFileName(String extension) {
        return UUID.randomUUID().toString().replace("-", "") + 
               "." + extension;
    }

    /**
     * 生成日期目录路径
     */
    private String generateDatePath() {
        LocalDateTime now = LocalDateTime.now();
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy/MM/dd");
        return now.format(formatter);
    }
}
```

**文件上传Controller**

```java
package com.example.controller;

import com.example.util.FileUploadUtil;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * 文件上传控制器
 */
@RestController
@RequestMapping("/api/file")
public class FileUploadController {

    @Autowired
    private FileUploadUtil fileUploadUtil;

    /**
     * 上传单个文件
     */
    @PostMapping("/upload")
    public Map<String, Object> upload(@RequestParam("file") MultipartFile file) {
        Map<String, Object> result = new HashMap<>();
        
        try {
            // 上传文件
            String fileUrl = fileUploadUtil.uploadFile(file);
            
            result.put("success", true);
            result.put("message", "文件上传成功");
            result.put("fileUrl", fileUrl);
            result.put("fileName", file.getOriginalFilename());
            result.put("fileSize", file.getSize());
            
        } catch (Exception e) {
            result.put("success", false);
            result.put("message", "文件上传失败：" + e.getMessage());
        }
        
        return result;
    }

    /**
     * 上传多个文件
     */
    @PostMapping("/uploads")
    public Map<String, Object> uploads(@RequestParam("files") MultipartFile[] files) {
        Map<String, Object> result = new HashMap<>();
        
        try {
            // 上传多个文件
            List<String> fileUrls = fileUploadUtil.uploadFiles(files);
            
            result.put("success", true);
            result.put("message", "文件上传成功");
            result.put("fileUrls", fileUrls);
            result.put("count", fileUrls.length);
            
        } catch (Exception e) {
            result.put("success", false);
            result.put("message", "文件上传失败：" + e.getMessage());
        }
        
        return result;
    }

    /**
     * 删除文件
     */
    @DeleteMapping("/delete")
    public Map<String, Object> delete(@RequestParam("fileUrl") String fileUrl) {
        Map<String, Object> result = new HashMap<>();
        
        try {
            boolean deleted = fileUploadUtil.deleteFile(fileUrl);
            
            if (deleted) {
                result.put("success", true);
                result.put("message", "文件删除成功");
            } else {
                result.put("success", false);
                result.put("message", "文件不存在或删除失败");
            }
            
        } catch (Exception e) {
            result.put("success", false);
            result.put("message", "文件删除失败：" + e.getMessage());
        }
        
        return result;
    }
}
```

**静态资源配置**

```java
package com.example.config;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

/**
 * Web MVC 配置类
 */
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    @Value("${file.upload.path:/data/uploads}")
    private String uploadPath;

    @Value("${file.upload.url-prefix:/uploads}")
    private String urlPrefix;

    /**
     * 配置静态资源映射
     */
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        
        // 映射上传文件目录
        registry.addResourceHandler(urlPrefix + "/**")
                .addResourceLocations("file:" + uploadPath + "/");
        
        // 其他静态资源配置
        registry.addResourceHandler("/static/**")
                .addResourceLocations("classpath:/static/");
    }
}
```

**前端上传文件示例**

**HTML表单**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>文件上传</title>
</head>
<body>
    <h2>单文件上传</h2>
    <form action="/api/file/upload" method="post" enctype="multipart/form-data">
        <input type="file" name="file" required>
        <button type="submit">上传</button>
    </form>

    <h2>多文件上传</h2>
    <form action="/api/file/uploads" method="post" enctype="multipart/form-data">
        <input type="file" name="files" multiple required>
        <button type="submit">上传</button>
    </form>
</body>
</html>
```

**JavaScript Ajax上传**

```javascript
// 单文件上传
function uploadFile(file) {
    const formData = new FormData();
    formData.append('file', file);

    fetch('/api/file/upload', {
        method: 'POST',
        body: formData
    })
    .then(response => response.json())
    .then(data => {
        if (data.success) {
            console.log('上传成功:', data.fileUrl);
        } else {
            console.error('上传失败:', data.message);
        }
    })
    .catch(error => console.error('上传错误:', error));
}

// 多文件上传
function uploadFiles(files) {
    const formData = new FormData();
    for (let i = 0; i < files.length; i++) {
        formData.append('files', files[i]);
    }

    fetch('/api/file/uploads', {
        method: 'POST',
        body: formData
    })
    .then(response => response.json())
    .then(data => {
        if (data.success) {
            console.log('上传成功:', data.fileUrls);
        } else {
            console.error('上传失败:', data.message);
        }
    });
}
```

**Vue.js 上传示例**

```vue
<template>
  <div>
    <h2>文件上传</h2>
    
    <!-- 单文件上传 -->
    <input type="file" @change="handleSingleUpload">
    <p v-if="singleFileUrl">上传成功：{{ singleFileUrl }}</p>
    
    <!-- 多文件上传 -->
    <input type="file" multiple @change="handleMultipleUpload">
    <div v-for="(url, index) in multipleFileUrls" :key="index">
      {{ url }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      singleFileUrl: '',
      multipleFileUrls: []
    }
  },
  methods: {
    // 单文件上传
    async handleSingleUpload(event) {
      const file = event.target.files[0];
      const formData = new FormData();
      formData.append('file', file);

      try {
        const response = await this.$axios.post('/api/file/upload', formData);
        if (response.data.success) {
          this.singleFileUrl = response.data.fileUrl;
          alert('上传成功');
        } else {
          alert('上传失败：' + response.data.message);
        }
      } catch (error) {
        alert('上传错误：' + error.message);
      }
    },

    // 多文件上传
    async handleMultipleUpload(event) {
      const files = event.target.files;
      const formData = new FormData();
      for (let i = 0; i < files.length; i++) {
        formData.append('files', files[i]);
      }

      try {
        const response = await this.$axios.post('/api/file/uploads', formData);
        if (response.data.success) {
          this.multipleFileUrls = response.data.fileUrls;
          alert('上传成功，共 ' + response.data.count + ' 个文件');
        } else {
          alert('上传失败：' + response.data.message);
        }
      } catch (error) {
        alert('上传错误：' + error.message);
      }
    }
  }
}
</script>
```

**文件上传最佳实践**

1. **文件命名**：使用UUID或时间戳生成唯一文件名，避免中文文件名乱码
2. **目录结构**：按日期分目录存储（YYYY/MM/DD），便于管理和查找
3. **文件大小限制**：在前端和后端都进行大小限制
4. **文件类型验证**：检查文件扩展名和MIME类型
5. **病毒扫描**：上传完成后进行病毒扫描（生产环境）
6. **访问控制**：通过URL重写或权限控制文件访问
7. **CDN分发**：大文件使用CDN分发，减轻服务器压力
8. **定期清理**：定期清理过期文件，释放存储空间

---



## 四、SpringBoot 运维实战

### 4.1 打包与部署

#### 4.1.1 Maven 打包

**配置打包插件**

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

**执行打包命令**

```bash
mvn clean package
```

**生成的文件结构**

```
target/
├── springboot-demo-1.0.0.jar          # 可执行 JAR
└── springboot-demo-1.0.0.jar.original  # 普通 JAR
```

---

#### 4.1.2 运行应用

**方式一：直接运行 JAR**

```bash
java -jar springboot-demo-1.0.0.jar
```

**方式二：指定配置文件**

```bash
java -jar springboot-demo-1.0.0.jar --spring.config.location=/path/to/application.yml
```

**方式三：临时修改配置**

```bash
java -jar springboot-demo-1.0.0.jar --server.port=9090 --spring.profiles.active=prod
```

---

#### 4.1.3 配置优先级

SpringBoot 支持多种配置方式，优先级从高到低：

1. **命令行参数**：`--server.port=8080`
2. **SPRING_APPLICATION_JSON**：环境变量中的 JSON 配置
3. **JNDI**：`java:comp/env/`
4. **Java 系统属性**：`System.getProperties()`
5. **操作系统环境变量**
6. **外部配置文件**：`./config/application.yml`
7. **外部配置文件**：`./application.yml`
8. **类路径配置文件**：`classpath:/config/application.yml`
9. **类路径配置文件**：`classpath:/application.yml`
10. **默认属性**：`SpringApplication.setDefaultProperties()`

---

### 4.2 日志管理

#### 4.2.1 日志框架切换

SpringBoot 默认使用 Logback，可切换至 Log4j2。

**排除 Logback**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

**引入 Log4j2**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```

---

#### 4.2.2 日志配置

**基础配置**

```yaml
logging:
  # 日志级别
  level:
    root: INFO
    com.example: DEBUG
  
  # 日志文件
  file:
    name: logs/application.log
  
  # 日志格式
  pattern:
    console: "%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n"
    file: "%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n"
```

**日志分组**

```yaml
logging:
  group:
    ebank: com.example.controller, com.example.service
  level:
    root: WARN
    ebank: DEBUG
```

---

#### 4.2.3 结构化日志（SpringBoot 3.4+）

**启用 ECS 格式**

```yaml
logging:
  structured:
    format: ecs  # Elastic Common Schema
```

**输出示例**

```json
{
  "@timestamp": "2026-03-16T09:25:40.123Z",
  "message": "用户注册成功",
  "level": "INFO",
  "service.name": "springboot-demo",
  "process.thread.name": "http-nio-8080-exec-1",
  "log.logger": "com.example.controller.UserController"
}
```

**优势**：直接对接 ELK、Graylog 等日志系统。

---

### 4.3 监控与可观测性

#### 4.3.1 Actuator 端点

**添加依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

**基础配置**

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus
  endpoint:
    health:
      show-details: always
```

**常用端点**

| 端点 | 功能描述 | 示例 |
|-------|---------|-------|
| /actuator/health | 健康检查 | `{"status":"UP"}` |
| /actuator/info | 应用信息 | `{"app":"SpringBoot Demo","version":"1.0.0"}` |
| /actuator/metrics | 指标数据 | JVM、HTTP、数据库等指标 |
| /actuator/prometheus | Prometheus 格式指标 | 时间序列数据 |
| /actuator/env | 环境变量 | 所有配置属性 |
| /actuator/threaddump | 线程堆栈 | 所有线程信息 |
| /actuator/beans | Bean 信息 | 容器中所有 Bean |

---

#### 4.3.2 自定义健康指标

**实现 HealthIndicator**

```java
@Component
public class DatabaseHealthIndicator implements HealthIndicator {
    
    @Autowired
    private DataSource dataSource;
    
    @Override
    public Health health() {
        try (Connection connection = dataSource.getConnection()) {
            if (connection.isValid(1)) {
                return Health.up()
                        .withDetail("database", "MySQL")
                        .withDetail("version", "8.0")
                        .build();
            }
        } catch (SQLException e) {
            return Health.down()
                    .withDetail("error", e.getMessage())
                    .build();
        }
    }
}
```

**访问健康检查**

```bash
curl http://localhost:8080/actuator/health
```

**响应示例**

```json
{
  "status": "UP",
  "components": {
    "database": {
      "status": "UP",
      "details": {
        "database": "MySQL",
        "version": "8.0"
      }
    }
  }
}
```

---

#### 4.3.3 自定义指标

**使用 MeterRegistry**

```java
@Service
public class OrderService {
    
    private final Counter orderCounter;
    private final Timer orderTimer;
    
    public OrderService(MeterRegistry registry) {
        this.orderCounter = registry.counter("orders.total");
        this.orderTimer = registry.timer("orders.processing.time");
    }
    
    public void createOrder(Order order) {
        Timer.Sample sample = Timer.start();
        try {
            // 业务逻辑
            orderService.save(order);
            orderCounter.increment();
        } finally {
            sample.stop(orderTimer);
        }
    }
}
```

**访问指标**

```bash
curl http://localhost:8080/actuator/metrics/orders.total
```

**响应示例**

```json
{
  "name": "orders.total",
  "measurements": [
    {
      "statistic": "COUNT",
      "value": 42.0
    }
  ],
  "availableTags": []
}
```

---

## 五、SpringBoot 原理深度解析

### 5.1 @SpringBootApplication 原理

#### 5.1.1 注解组成

```java
@SpringBootApplication
// 等价于：
@SpringBootConfiguration  // 标识为配置类
@EnableAutoConfiguration  // 启用自动配置
@ComponentScan           // 组件扫描
```

#### 5.1.2 启动流程

```
SpringApplication.run()
    ↓
准备环境（加载配置）
    ↓
创建 ApplicationContext
    ↓
加载自动配置
    ↓
刷新上下文（初始化 Bean）
    ↓
调用 Runner（应用启动完成）
```

---

### 5.2 自动配置加载机制

#### 5.2.1 加载入口

**SpringBoot 3.x 变化**：不再使用 `META-INF/spring.factories`，改用 `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`

**AutoConfiguration.imports 示例**

```
com.example.autoconfigure.DataSourceAutoConfiguration
com.example.autoconfigure.RedisAutoConfiguration
com.example.autoconfigure.WebMvcAutoConfiguration
```

#### 5.2.2 条件装配流程

```java
@AutoConfiguration
@ConditionalOnClass(DataSource.class)  // 类路径存在 DataSource
@EnableConfigurationProperties(DataSourceProperties.class)
public class DataSourceAutoConfiguration {
    
    @Bean
    @ConditionalOnMissingBean  // 容器中不存在 DataSource Bean
    public DataSource dataSource(DataSourceProperties properties) {
        // 创建数据源
        return createDataSource(properties);
    }
}
```

---

### 5.3 Starter 工作原理

#### 5.3.1 Starter 结构

```
mybatis-spring-boot-starter/
├── pom.xml
└── src/
    └── main/
        └── resources/
            └── META-INF/
                └── spring/
                    └── org.springframework.boot.autoconfigure.AutoConfiguration.imports
```

#### 5.3.2 Starter 核心组件

1. **自动配置类**：定义 Bean 的创建逻辑
2. **配置属性类**：定义配置前缀和默认值
3. **条件注解**：控制 Bean 的加载条件

---

### 5.4 启动过程详解

#### 5.4.1 SpringApplication.run() 执行步骤

1. **创建 SpringApplication**：保存引导类和参数
2. **准备环境**：加载配置文件、环境变量
3. **打印 Banner**：显示 SpringBoot Logo
4. **创建 ApplicationContext**：根据类型创建容器
5. **准备上下文**：注册 Bean、加载自动配置
6. **刷新上下文**：完成 Bean 初始化
7. **调用 Runner**：执行 ApplicationRunner 和 CommandLineRunner
8. **启动应用**：开始处理请求

#### 5.4.2 启动监听器

**自定义监听器**

```java
@Component
public class ApplicationStartListener implements ApplicationListener<ApplicationReadyEvent> {
    
    @Override
    public void onApplicationEvent(ApplicationReadyEvent event) {
        System.out.println("应用启动完成！");
    }
}
```

---

## 六、开发工具附录

### 6.1 Lombok 插件

#### 6.1.1 安装配置

**IDEA 安装步骤**

1. 打开 `File` → `Settings` → `Plugins`
2. 搜索 `Lombok`
3. 点击 `Install` 重启 IDEA
4. 启用注解处理：`File` → `Settings` → `Build` → `Compiler` → `Annotation Processors` → 勾选 `Enable annotation processing`

#### 6.1.2 常用注解

| 注解 | 作用 | 示例 |
|------|------|------|
| @Data | 生成 getter/setter/toString/equals/hashCode | `@Data class User { ... }` |
| @Getter/@Setter | 生成 getter/setter | `@Getter private String name;` |
| @NoArgsConstructor | 生成无参构造器 | `@NoArgsConstructor class User { ... }` |
| @AllArgsConstructor | 生成全参构造器 | `@AllArgsConstructor class User { ... }` |
| @Slf4j | 生成日志对象 | `@Slf4j class Service { log.info("..."); }` |
| @Builder | 生成构建器模式 | `@Builder class User { User u = User.builder().name("张三").build(); }` |

---

### 6.2 开源 IDEA 插件推荐

| 插件名称 | 功能描述 |
|-----------|---------|
| Rainbow Brackets | 彩虹括号高亮 |
| Translation | 翻译插件 |
| String Manipulation | 字符串操作工具 |
| Maven Helper | Maven 依赖管理助手 |
| MyBatisX | MyBatis 增强插件 |

---

### 6.3 调试工具

#### 6.3.1 IDEA Debug 快捷键

| 快捷键 | 功能 |
|--------|------|
| F8 | 单步跳过 |
| F7 | 单步进入 |
| F9 | 继续执行 |
| Ctrl+F8 | 表达式求值 |
| Ctrl+F9 | 运行到光标处 |

#### 6.3.2 远程调试

**启动参数**

```bash
java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar app.jar
```

**IDEA 远程配置**

1. `Run` → `Edit Configurations`
2. 添加 `Remote JVM Debug`
3. Host: `localhost`
4. Port: `5005`

---

## 📝 总结与最佳实践

### 核心要点回顾

1. **SpringBoot 本质**：通过自动配置简化 Spring 开发
2. **技术基线**：Java 17+、Jakarta EE 9+、Spring Framework 6.x+
3. **配置优先级**：命令行 > 环境变量 > 配置文件
4. **开发模式**：Starter 简化依赖、YAML 层次化配置、注解驱动开发
5. **运维要点**：Actuator 监控、结构化日志、多环境配置

### 升级建议（2.x → 3.x）

**升级前准备**

```
评估项目 → 依赖兼容性检查 → javax → jakarta 迁移 → 配置属性更新 → 单元测试修复 → 集成测试验证 → 性能基准测试 → 生产环境发布
```

**关键迁移点**

| 项目 | SpringBoot 2.x | SpringBoot 3.x |
|------|---------------|---------------|
| Java 版本 | 8+ | 17+ |
| 包名 | `javax.*` | `jakarta.*` |
| 配置属性 | `spring.jpa.properties.javax.*` | `spring.jpa.properties.jakarta.*` |
| Hibernate | 5.x | 6.x |
| Spring Security | 5.x | 6.x |
| 监控 | Spring Cloud Sleuth | Micrometer Tracing |

---

### 常见问题 FAQ

**Q1: SpringBoot 3.x 最低 JDK 版本是多少？**

A: **Java 17**，推荐使用 Java 21 以支持虚拟线程。

**Q2: javax 包如何迁移到 jakarta？**

A: 使用 IDE 的全局替换功能，正则表达式：`javax\.(persistence|servlet|validation)\..*` → `jakarta.$1.*`。

**Q3: 如何禁用自动配置？**

A: 使用 `@EnableAutoConfiguration(exclude = {DataSourceAutoConfiguration.class})` 或配置文件设置 `spring.autoconfigure.exclude=...`。

**Q4: SpringBoot 3.x 如何生成原生镜像？**

A: 使用 GraalVM Native Build Tools：

```bash
mvn -Pnative native:compile
```

**Q5: 如何切换日志框架？**

A: 排除 `spring-boot-starter-logging`，引入 `spring-boot-starter-log4j2`。

---

## 🔗 参考资源

- **SpringBoot 官方文档**：https://spring.io/projects/spring-boot
- **SpringBoot 参考文档**：https://docs.spring.io/spring-boot/docs/current/reference/html/
- **Jakarta EE 规范**：https://jakarta.ee/
- **GraalVM 官网**：https://www.graalvm.org/

---

> **文档版本**：v3.0  
> **最后更新**：2026-03-17  
> **作者**：SpringBoot 学习小组
