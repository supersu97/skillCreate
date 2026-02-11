---
name: "java-backend-code-modifier"
description: "专为Java后台项目设计的代码修改技能，支持Spring框架、MyBatis、JUnit测试。当需要修改现有Java代码、根据接口文档更新Spring Controller、修改MyBatis映射文件或自动生成JUnit测试时调用。"
---

# 执行指令（CRITICAL - 必须遵循）

当用户调用此技能时，你必须按照以下步骤执行：

## 步骤1：理解修改需求
- 识别要修改的目标（类名、方法名、文件路径等）
- 理解修改的具体要求和目的
- 确定是否需要生成JUnit测试
- 确定用户是否指定了修改方式（直接修改、交互式选择等）

## 步骤2：分析现有代码
- 使用 Glob 工具查找目标文件
- 使用 Read 工具读取相关代码
- 使用 Grep 工具查找依赖和引用
- 分析代码结构、依赖关系、业务逻辑
- 识别潜在的修改风险点

## 步骤3：生成修改方案
- 根据需求设计修改方案
- 考虑代码风格一致性
- 考虑向后兼容性
- 考虑性能和安全影响
- 准备修改前后的代码对比

## 步骤4：确定执行方式

### 情况A：用户明确指定了执行方式
如果用户在请求中包含以下关键词，直接按指定方式执行：
- "直接修改" → 直接执行修改并展示结果
- "只查看修改方案" → 只展示修改方案，不执行
- "生成测试" → 重点关注测试生成

### 情况B：用户未指定执行方式（默认情况）
**必须使用交互式选择**，执行以下流程：

1. **简要总结修改方案**（2-3句话）
   ```
   已完成对 [目标] 的修改方案设计。计划修改 [X] 处代码，主要涉及 [简述]。
   ```

2. **展示关键修改点**（简要列出3-5个主要修改）
   ```
   主要修改点：
   1. [修改点1]
   2. [修改点2]
   3. [修改点3]
   ```

3. **使用 AskUserQuestion 工具提供选择菜单**
   ```javascript
   {
     "questions": [{
       "question": "请选择要执行的操作（可多选）：",
       "header": "修改操作",
       "multiSelect": true,
       "options": [
         {
           "label": "查看完整修改方案",
           "description": "查看所有修改的详细说明和代码对比"
         },
         {
           "label": "查看修改前后对比",
           "description": "并排对比修改前后的代码差异"
         },
         {
           "label": "查看风险评估",
           "description": "查看修改可能带来的风险和影响"
         },
         {
           "label": "执行代码修改",
           "description": "执行所有计划的代码修改"
         },
         {
           "label": "生成JUnit测试",
           "description": "为修改的代码生成单元测试"
         },
         {
           "label": "只修改指定方法",
           "description": "只修改特定的方法，不修改其他部分"
         },
         {
           "label": "导出修改方案",
           "description": "将修改方案导出到文件"
         }
       ]
     }]
   }
   ```

4. **根据用户选择执行相应操作**
   - 如果选择"查看完整修改方案"，展示详细的修改说明
   - 如果选择"执行代码修改"，使用 Edit/Write 工具执行修改
   - 如果选择"生成JUnit测试"，生成测试代码
   - 支持多选，按顺序执行

## 步骤5：执行修改（如果用户确认）
- 使用 Edit 工具修改现有文件
- 使用 Write 工具创建新文件（如测试文件）
- 记录每个修改操作的结果
- 如果修改失败，提供详细的错误信息

## 步骤6：后续确认
修改完成后，询问用户：
```
修改已完成。是否需要：
1. 查看修改后的完整代码
2. 运行测试验证修改
3. 继续修改其他部分
4. 回滚某些修改
```

---

# Java后台代码修改助手

*本技能专为Java后台项目设计，提供智能化的Spring框架代码修改和JUnit测试生成功能。*

## 功能介绍

这个技能让开发者能够：

1. **智能Java代码修改**：根据需求修改现有Spring Controller、Service、DAO层代码
2. **多种输入支持**：支持类名、方法名、文件名、接口路径、MyBatis映射文件等多种指定方式
3. **文档驱动修改**：根据本地或在线接口文档自动更新Controller接口
4. **精准定位修改**：精确指定要修改的Java类、方法或XML映射文件
5. **自动JUnit测试生成**：在test目录下自动查找相关测试并生成JUnit 4测试用例
6. **安全可靠的修改**：确保修改后的代码保持原有功能和兼容性
7. **XML配置文件修改**：支持Spring XML配置文件和MyBatis映射文件的修改

## 何时调用

当你遇到以下情况时，应该调用这个技能：

- 需要根据新的接口文档更新Spring Controller
- 需要修改某个Service中的业务逻辑
- 需要更新MyBatis的XML映射文件
- 需要为现有的Java方法添加新功能
- 需要修复Java代码中的bug或安全问题
- 需要根据规范更新代码结构
- 需要为修改的代码自动生成JUnit单元测试
- 需要批量修改相关Controller或Service
- 需要更新Spring XML配置文件

## 使用方法

### 基本用法

1. **指定修改目标**：支持以下格式指定要修改的代码：
   - **类名**：如 `UserController`、`OrderServiceImpl`
   - **方法名**：如 `UserController.getUserById`、`OrderService.createOrder`
   - **文件名**：如 `src/main/java/com/example/controller/UserController.java`
   - **接口路径**：如 `@RequestMapping("/api/users/:id")`
   - **MyBatis映射文件**：如 `src/main/resources/mapper/UserMapper.xml`
   - **Spring配置文件**：如 `src/main/resources/spring/application-context.xml`

2. **指定修改位置**：精确说明要修改的具体位置
   - 修改整个Controller接口
   - 修改Service中的某个业务方法
   - 修改DAO层的数据访问逻辑
   - 修改MyBatis映射文件中的SQL语句
   - 修改Spring配置文件中的Bean定义
   - 修改特定参数或返回值
   - 修改异常处理逻辑

3. **提供修改说明**：详细说明要如何修改
   - 直接提供修改要求
   - 提供接口文档链接或本地文件
   - 说明要添加、删除或修改的具体内容
   - 提供示例代码或规范
   - **注意**：请说明是否需要保持原有代码风格，或指定特定的Java代码规范要求

4. **指定测试生成**：是否需要自动生成JUnit测试
   - 自动在test目录下查找相关测试
   - 生成新的JUnit测试类
   - 更新现有测试用例
   - 支持Spring测试框架集成

5. **确认修改方案**：查看修改建议并确认执行

### 交互式修改流程（默认方式）

当你没有明确指定执行方式时，系统会：

1. **分析代码并生成修改方案**
2. **简要总结修改内容**（2-3句话）
3. **列出主要修改点**（3-5个关键修改）
4. **提供交互式选择菜单**：
   ```
   请选择要执行的操作（可多选）：
   1. 查看完整修改方案
   2. 查看修改前后对比
   3. 查看风险评估
   4. 执行代码修改
   5. 生成JUnit测试
   6. 只修改指定方法
   7. 导出修改方案
   ```
5. **根据你的选择执行相应操作**

### 高级用法

#### 1. 基于接口文档的Spring Controller修改

```
请根据以下接口文档修改对应的Spring Controller：
接口文档链接：https://api.example.com/docs/user-api
需要修改的Controller：UserController
需要修改的方法：getUserById
修改要求：根据文档更新参数验证和响应格式
自动生成JUnit测试：是
```

```
请根据本地文档文件修改代码：
文档文件：./docs/user-api.yaml
需要修改的Service：UserServiceImpl
修改要求：按照文档规范更新业务逻辑实现
自动生成JUnit测试：是，使用Spring测试框架
```

#### 2. 修改MyBatis映射文件

```
请修改以下MyBatis映射文件中的SQL查询：
映射文件：src/main/resources/mapper/UserMapper.xml
需要修改的SQL ID：selectUserById
修改要求：
1. 添加新的查询字段
2. 优化SQL查询性能
3. 添加分页支持
4. 更新结果映射配置
```

#### 3. 批量修改相关Controller

```
请批量修改所有用户相关的Controller：
Controller模式：所有包含 "User" 的Controller类
修改要求：
1. 统一添加请求参数验证
2. 统一响应格式为BaseResult
3. 统一异常处理机制
4. 统一添加操作日志
生成JUnit测试：是，为每个Controller生成集成测试
```

#### 4. 自动生成JUnit测试

```
请为以下Service方法修改代码并自动生成JUnit测试：
Service：OrderServiceImpl
方法：calculateTotal
修改要求：添加优惠券计算逻辑
测试要求：在test目录下查找相关测试并生成新的测试用例，使用SpringJUnit4ClassRunner
```

#### 5. 直接修改（跳过交互）

```
请直接修改以下方法，不需要交互确认：
文件：UserServiceImpl.java
方法：getUserById
修改要求：添加缓存注解 @Cacheable
```

## 支持的技术栈

### Java相关
- **Java版本**：Java 8+（兼容当前项目使用的Java版本）
- **框架**：Spring Framework 4.x/5.x、Spring Boot、Spring MVC
- **ORM**：MyBatis、MyBatis-Plus、Hibernate（根据项目实际使用）
- **数据库**：Oracle、MySQL、SQL Server、PostgreSQL
- **构建工具**：Maven、Gradle
- **测试框架**：JUnit 4/5、Spring Test、Mockito、PowerMock

### 项目架构
- **分层架构**：Controller层、Service层、DAO层、Entity层
- **配置文件**：Spring XML配置、properties文件、YAML配置
- **映射文件**：MyBatis XML映射文件
- **依赖注入**：Spring注解驱动、XML配置驱动

### 文档格式
- **接口文档**：OpenAPI/Swagger、Markdown、YAML、JSON
- **数据库文档**：SQL脚本、数据字典
- **配置文档**：properties、YAML、XML

## 修改原则

1. **保持功能不变**：确保修改后的Java代码保持原有核心功能
2. **向后兼容**：尽量保持向后兼容性，避免破坏现有调用
3. **代码风格一致**：保持与原有Java代码一致的风格、命名规范和格式
4. **代码质量**：遵循Java代码规范和最佳实践，如阿里巴巴Java开发规约
5. **测试覆盖**：确保修改后的代码有充分的JUnit测试覆盖
6. **安全性**：避免引入安全漏洞和风险，特别是SQL注入、XSS等
7. **性能考虑**：考虑修改对性能的影响，特别是数据库查询性能
8. **事务管理**：确保事务边界正确，避免事务相关问题

## JUnit测试生成策略

### 测试发现
1. 自动在项目的src/test/java目录下查找相关测试文件
2. 根据命名约定匹配测试文件（如UserService → UserServiceTest）
3. 分析现有测试结构和模式（如使用SpringJUnit4ClassRunner）
4. 确定测试框架和断言库（JUnit 4、AssertJ等）

### 测试生成
1. **新建测试类**：如果不存在对应的测试类
2. **更新现有测试**：如果已存在测试类
3. **测试用例设计**：
   - 正常情况测试
   - 边界情况测试
   - 异常情况测试
   - 数据库集成测试（如适用）
   - 事务测试（如适用）
4. **测试数据准备**：生成合适的测试数据，支持@Before、@BeforeClass
5. **Mock对象配置**：自动配置Mock对象（如Mockito）
6. **Spring上下文配置**：自动配置Spring测试上下文

### 测试覆盖
1. 方法级别覆盖（所有public方法）
2. 参数组合覆盖
3. 异常情况覆盖
4. 集成测试覆盖（Controller层、Service层、DAO层）
5. 事务测试覆盖

## 示例

### 示例1：根据接口文档修改Spring Controller

```
请根据以下OpenAPI文档修改用户注册接口：
文档内容：
```
openapi: 3.0.0
paths:
  /api/auth/register:
    post:
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [username, email, password]
              properties:
                username:
                  type: string
                  minLength: 3
                email:
                  type: string
                  format: email
                password:
                  type: string
                  minLength: 8
      responses:
        '201':
          description: User created successfully
```

需要修改的文件：src/main/java/com/example/controller/UserController.java
修改的方法：register
额外要求：添加密码强度验证，密码必须包含字母和数字
自动生成JUnit测试：是，生成Controller层集成测试
```

### 示例2：修改Service业务逻辑

```
请修改订单服务中的价格计算逻辑：
文件：src/main/java/com/example/service/impl/OrderServiceImpl.java
方法：calculateTotal
修改要求：
1. 添加优惠券折扣计算
2. 添加会员等级折扣
3. 添加增值税计算
4. 更新价格舍入规则（四舍五入保留2位小数）
生成JUnit测试：是，在现有测试基础上添加新的测试用例，覆盖所有价格计算场景
```

### 示例3：修改MyBatis映射文件

```
请修改用户查询的MyBatis映射文件：
文件：src/main/resources/mapper/UserMapper.xml
需要修改的SQL：selectUserList
修改要求：
1. 添加新的查询条件：status、createTime范围
2. 优化查询性能，添加索引提示
3. 添加分页支持（使用limit offset）
4. 更新结果映射，添加新字段
```

### 示例4：批量修改和测试生成

```
请批量修改所有产品相关接口：
Controller模式：所有包含 "Product" 的Controller类
修改要求：
1. 统一添加分页参数支持（pageNum, pageSize）
2. 统一添加查询缓存，缓存时间5分钟
3. 统一响应中添加分页信息（total, pageNum, pageSize, pages）
生成JUnit测试：是，为每个修改的Controller生成集成测试，测试分页功能
```

## 输出内容

### 修改报告
1. **修改前代码**：显示原始Java代码或XML配置
2. **修改后代码**：显示修改后的代码
3. **修改说明**：详细说明每个修改的原因和目的
4. **代码对比**：展示修改前后的差异
5. **影响分析**：分析修改对现有功能的影响
6. **数据库影响**：分析对数据库的影响（如SQL变更）

### 测试报告
1. **测试文件位置**：生成的测试文件路径
2. **测试用例列表**：生成的测试用例说明
3. **测试覆盖率**：估计的测试覆盖率
4. **测试运行建议**：如何运行测试的建议（如Maven命令）
5. **Spring上下文配置**：生成的测试上下文配置

### 风险提示
1. **兼容性风险**：可能的兼容性问题
2. **性能影响**：对性能的潜在影响，特别是数据库查询性能
3. **安全考虑**：需要注意的安全问题（SQL注入、XSS等）
4. **事务风险**：可能的事务相关问题
5. **测试建议**：额外的测试建议

## 注意事项

1. **备份建议**：建议在执行修改前备份重要代码，特别是生产环境代码
2. **代码审查**：建议进行代码审查后再合并到主分支
3. **代码风格检查**：修改后检查代码风格是否与原有Java代码保持一致
4. **逐步部署**：建议逐步部署修改，监控系统稳定性
5. **监控配置**：修改后建议配置相应的监控和告警
6. **文档更新**：修改后需要更新相关文档（JavaDoc、接口文档等）
7. **数据库变更**：如果涉及数据库变更，需要同步更新数据库脚本
8. **依赖检查**：检查修改是否引入新的依赖或版本冲突

---

*使用本技能可以快速、安全地修改Java后台代码，并自动生成高质量的JUnit测试，提高开发效率和代码质量。特别适用于Spring框架和MyBatis项目。*
