# 后端项目分析模块

本模块负责分析后端项目结构和代码风格。

## 后端项目结构分析

**如果使用了缓存，跳过本步骤**

根据收集到的项目路径，使用 Glob 和 Read 工具分析项目结构：

```javascript
// 分析后端项目结构
调用 Glob 工具：
- path: [后端项目路径]
- pattern: "src/**/*"

调用 Read 工具：
- file_path: [后端路径]/pom.xml 或 build.gradle 或 requirements.txt 或 go.mod
```

生成项目结构报告：
```
## 后端项目结构分析

- 框架：[Spring Boot 3.x / Django 4.x / Express / Go]
- 数据库：[MySQL / PostgreSQL / MongoDB]
- ORM：[JPA / Hibernate / MyBatis / Sequelize / Prisma]
- 认证：[JWT / Session / OAuth]
- 目录结构：
  - src/main/java/...：Java 源码
  - src/main/resources：配置文件
  - src/test：测试代码
```

## 后端代码风格分析

**必须分析现有代码风格，生成代码风格指南，后续所有代码生成必须严格遵循**

```javascript
调用 Glob 工具：
- path: [后端项目路径]/src
- pattern: "**/*.java" 或 "**/*.py" 或 "**/*.js" 或 "**/*.go"

调用 Read 工具读取 3-5 个代表性文件：
- Controller/Handler 文件
- Service 业务逻辑文件
- Model/Entity 实体文件
- 数据库操作文件（如：Mapper/DAO/Repository）
```

## 代码风格指南生成

生成代码风格指南：
```
## 后端代码风格指南（必须严格遵循）

### 1. 命名风格
- 类命名：[如：UserController、UserService]
- 方法命名：[如：getUserById、createUser]
- 变量命名：[如：userId、userList]
- 常量命名：[如：USER_STATUS、MAX_COUNT]

### 2. 代码结构风格
- 分层结构：[如：Controller → Service → Mapper]
- 参数校验：[如：@Valid、@NotNull]
- 事务管理：[如：@Transactional]
- 异常处理：[如：@ExceptionHandler、try-catch]

### 3. 数据库操作风格
- 查询方式：[如：JPQL、@Query、XML 映射]
- 命名规范：[如：下划线命名、驼峰命名]
- 关联关系：[如：@OneToMany、@ManyToOne]

**重要提示**：后续所有生成的代码必须严格遵循上述代码风格，禁止使用与现有代码风格不一致的实现方式。
```
