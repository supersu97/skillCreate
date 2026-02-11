---
name: "java-backend-code-analyzer"
description: "让开发者使用AI分析和理解Java后台代码，包括Spring框架、MyBatis、接口详细分析。当需要理解Java后台代码逻辑、分析接口流程、查找bug、优化性能或进行代码修改时调用。"
---

# Java后台代码AI分析工具

*本技能专为Claude Code设计，提供强大的交互式Java代码分析体验，特别针对Spring框架和MyBatis项目。*

## 功能介绍

这个技能让开发者能够：

1. **Java代码分析**：自动分析Java后台代码结构、逻辑流程和依赖关系
2. **Spring接口分析**：详细分析Spring Controller接口的出入参、调用流程、第三方接口依赖和数据库操作
3. **MyBatis分析**：分析MyBatis映射文件中的SQL语句、结果映射、性能优化
4. **问题定位**：快速识别Java代码中的bug、性能瓶颈和安全隐患
5. **代码修改**：基于需求智能生成和修改Java后台代码
6. **文档生成**：为复杂Java代码生成清晰的文档和注释
7. **技术咨询**：解答关于Java后台代码的技术问题

## 何时调用

当你遇到以下情况时，应该调用这个技能：

- 需要理解不熟悉的Java后台代码逻辑
- Java代码出现bug需要定位和修复
- 想要优化Java后台代码性能
- 需要为前端功能添加或修改后台API
- 需要分析Spring Controller接口的详细调用流程、出入参、第三方接口依赖和数据库操作
- 对Java后台技术实现有疑问需要解答
- 需要快速了解代码库的整体结构
- 需要分析MyBatis映射文件中的SQL性能问题
- 需要理解Spring事务管理和Bean生命周期

## 使用方法

### 基本用法

1. **指定分析目标**：支持以下格式指定要分析的Java代码：
   - **类名**：如 `UserController`、`OrderServiceImpl`
   - **方法名**：如 `UserController.getUserById`、`OrderService.createOrder`
   - **文件路径**：如 `src/main/java/com/example/controller/UserController.java`
   - **接口路径**：如 `@RequestMapping("/api/users/:id")`
   - **MyBatis映射文件**：如 `src/main/resources/mapper/UserMapper.xml`
   - **Spring配置文件**：如 `src/main/resources/spring/application-context.xml`

2. **描述具体需求**：详细说明你想要分析的问题或想要实现的功能
3. **提供上下文信息**：如果有相关的前端代码或业务逻辑，一并提供
4. **等待分析结果**：AI会分析代码并提供详细的分析报告
5. **交互式选择结果**：分析完成后，可以在命令行中选择要查看的具体内容
6. **根据建议行动**：可以直接使用AI生成的代码修改建议

### 交互式选择结果
在分析完成后，你可以通过命令行交互式选择要查看的分析内容：

1. **完整查看**：查看所有分析结果
2. **部分查看**：选择查看特定类型的分析内容
3. **重点查看**：聚焦于某个关键问题或功能
4. **导出结果**：将分析结果保存到文件

**交互式选择示例：**
当分析完成后，Claude Code会提供类似以下的交互选项：

```
请选择要查看的分析内容：
1. Spring接口基本信息
2. 输入输出参数分析
3. Java代码调用顺序
4. 业务逻辑流程
5. MyBatis数据库操作详情
6. Spring事务管理分析
7. 性能考虑
8. 安全考虑
9. 查看所有内容
10. 导出分析报告到文件
```

你可以输入对应的数字或选择多个选项来查看特定内容。

### 高级用法

#### 1. Java代码分析

```
请分析以下Java后台代码文件，说明其功能和潜在问题：
src/main/java/com/example/service/impl/UserServiceImpl.java
```

#### 2. Spring接口分析

支持多种格式指定分析目标：

**格式一：类名+方法名**
```
请分析以下Spring Controller方法的详细调用流程：出入参、代码调用顺序、第三方接口调用、数据库操作等：
UserController.getUserById
```

**格式二：文件路径**
```
请分析以下Java文件中所有Spring接口的详细调用流程：出入参、代码调用顺序、第三方接口调用、数据库操作等：
src/main/java/com/example/controller/UserController.java
```

**格式三：接口路径**
```
请分析以下Spring接口的详细调用流程：出入参、代码调用顺序、第三方接口调用、数据库操作等：
@RequestMapping("/api/users/:id")
```

**输出内容选择方式**

### 交互式选择（推荐）
分析完成后，你可以在命令行中交互式选择要查看的内容：

```
请分析以下Spring Controller方法，完成分析后提供交互式选择：
UserController.getUserById
```

分析完成后，会显示类似以下的交互选项：

```
分析完成！请选择要查看的内容：
1. Spring接口基本信息
2. 输入输出参数分析
3. Java代码调用顺序
4. 业务逻辑流程
5. MyBatis数据库操作详情
6. Spring事务管理分析
7. 性能考虑
8. 安全考虑
9. 查看完整报告
10. 导出报告到文件
```

### 预设模式选择
```
请使用【基础模式】分析以下Spring接口：
UserController.getUserById
```

```
请使用【数据库模式】分析以下Service方法：
OrderService.processPayment
```

```
请使用【安全模式】分析以下MyBatis映射文件：
src/main/resources/mapper/UserMapper.xml
```

### 自定义详细选择
```
请分析以下Spring接口，只需要输出【接口基本信息】、【输入输出参数】和【数据库操作】：
UserController.getUserById
```

```
请分析以下Service方法，只需要输出【代码调用顺序】和【业务逻辑流程】：
OrderService.processPayment
```

```
请分析以下Java文件，只需要输出【代码结构】和【问题识别】：
src/main/java/com/example/service/impl/UserServiceImpl.java
```

#### 3. 问题定位

```
我的前端请求后台API时出现500错误，请分析以下Spring Controller找出问题：
src/main/java/com/example/controller/UserController.java
```

#### 4. 代码修改

```
请为以下Spring Controller添加一个新的API端点，用于获取用户统计数据：
src/main/java/com/example/controller/UserController.java
```

#### 5. 性能优化

```
请分析并优化以下Service方法的性能：
src/main/java/com/example/service/impl/OrderServiceImpl.java
```

#### 6. 技术咨询

```
这个Java后台代码使用了什么设计模式？有什么优缺点？
src/main/java/com/example/utils/AuthUtils.java
```

#### 7. MyBatis映射文件分析

```
请分析以下MyBatis映射文件中的SQL语句，评估性能和安全性：
src/main/resources/mapper/OrderMapper.xml
```

## 支持的Java技术栈

- **Java版本**：Java 8+（兼容当前项目使用的Java版本）
- **框架**：Spring Framework 4.x/5.x、Spring Boot、Spring MVC、Spring Data JPA
- **ORM**：MyBatis、MyBatis-Plus、Hibernate（根据项目实际使用）
- **数据库**：Oracle、MySQL、SQL Server、PostgreSQL
- **构建工具**：Maven、Gradle
- **测试框架**：JUnit 4/5、Spring Test、Mockito、PowerMock

## 注意事项

1. **代码安全性**：分析时会考虑Java代码的安全性，避免生成有安全隐患的代码
2. **代码风格**：会保持与原有Java代码一致的风格和命名规范
3. **向后兼容**：生成的代码修改会尽量保持向后兼容性
4. **性能考虑**：会考虑代码的性能影响，避免生成低效代码
5. **测试建议**：会提供相应的JUnit测试建议，确保代码修改的正确性
6. **事务管理**：会考虑Spring事务管理的影响

## 输出格式

你可以选择性地指定需要哪些分析内容。如果未指定，默认返回所有分析内容。

### 可选择的输出项目
1. **Java代码分析报告**：详细的Java代码结构和逻辑分析
2. **Spring接口分析报告**：详细的Spring接口调用流程、出入参、第三方接口依赖和数据库操作分析
3. **MyBatis分析报告**：MyBatis映射文件、SQL语句、结果映射分析
4. **问题识别**：明确指出Java代码中的问题和潜在风险
5. **修改建议**：具体的Java代码修改方案和理由
6. **技术解释**：对复杂Java技术概念的清晰解释
7. **最佳实践**：相关的Java代码最佳实践建议

### 输出内容选择方式

#### 方式一：交互式选择（最推荐）
分析完成后，在命令行中提供交互式选项，让你选择要查看的内容：

**交互式选择流程：**
1. 分析完成后，显示交互选项菜单
2. 你可以选择查看完整报告或特定部分
3. 支持多选和分步查看
4. 可以随时切换查看不同内容

**交互式选择优势：**
- 无需提前指定输出内容
- 可以动态选择感兴趣的部份
- 支持探索式分析
- 避免信息过载

**交互式选择示例：**
分析完成后会显示类似以下的选项：

```
分析完成！请选择要查看的内容：
1. Spring接口基本信息
2. 输入输出参数分析
3. Java代码调用顺序
4. 业务逻辑流程
5. MyBatis数据库操作详情
6. Spring事务管理分析
7. 性能考虑
8. 安全考虑
9. 查看完整报告
10. 导出报告到文件
```

**触发交互式选择：**
```
请分析以下Spring Controller方法，完成分析后提供交互式选择：
UserController.getUserById
```

#### 方式二：预设分析模式
提供以下预设分析模式，只需选择模式名称即可：

1. **基础模式**：仅输出核心信息
   - 包含：Spring接口基本信息、输入输出参数
   - 适合：快速了解接口基本功能

2. **详细模式**：输出完整分析内容（默认）
   - 包含：所有分析内容
   - 适合：全面深入的Java代码分析

3. **数据库模式**：重点关注数据库操作
   - 包含：MyBatis数据库操作、性能考虑、输入输出参数、SQL分析
   - 适合：分析数据库相关接口

4. **安全模式**：重点关注安全问题
   - 包含：安全考虑、问题识别、输入输出参数、SQL注入检查
   - 适合：安全审计和漏洞分析

5. **性能模式**：重点关注性能问题
   - 包含：性能考虑、代码调用顺序、业务逻辑流程、SQL性能优化
   - 适合：性能优化和分析

6. **事务模式**：重点关注事务管理
   - 包含：Spring事务管理、事务边界、事务传播机制
   - 适合：分析事务相关代码

**使用示例：**
```
请使用【基础模式】分析以下Spring接口：
UserController.getUserById
```

```
请使用【数据库模式】分析以下Service方法：
OrderService.createOrder
```

```
请使用【安全模式】分析以下MyBatis映射文件：
src/main/resources/mapper/UserMapper.xml
```

#### 方式三：自定义详细选择
如果你需要更精细的控制，可以指定具体的分析内容：

```
请分析以下Spring接口，只需要输出【接口基本信息】和【输入输出参数】：
UserController.getUserById
```

或者使用逗号分隔多个项目：

```
请分析以下Java代码，只需要输出【代码结构】和【问题识别】：
src/main/java/com/example/service/impl/UserServiceImpl.java
```

### 完整的分析内容分类
以下是可按类别选择的分析内容：

#### Spring接口分析相关
- **Spring接口基本信息**：Controller类、方法、@RequestMapping注解、HTTP方法、路径、描述
- **输入输出参数**：@RequestParam、@PathVariable、@RequestBody参数、响应DTO、返回类型
- **Java代码调用顺序**：Controller层 → Service层 → DAO层 → MyBatis映射层 → 数据库
- **业务逻辑流程**：从请求到响应的完整处理步骤，包含业务规则
- **第三方接口调用**：内部服务调用和外部API依赖
- **数据库操作**：MyBatis Mapper接口、SQL语句、查询表、关联关系、返回字段
- **性能考虑**：SQL索引、缓存策略、查询优化、连接池配置
- **安全考虑**：身份验证、权限验证、数据过滤、SQL注入防护
- **Spring事务管理**：@Transactional注解、事务传播机制、事务隔离级别

#### Java代码分析相关
- **Java代码结构**：类、方法、属性、接口、枚举的组织结构
- **功能概述**：Java代码的主要功能和用途
- **依赖关系**：Spring Bean依赖、类之间的调用关系
- **代码风格**：Java命名规范、注释质量、代码格式
- **性能瓶颈**：识别可能的性能问题，如循环查询、内存泄漏等
- **安全问题**：识别安全漏洞和风险，如SQL注入、XSS等
- **测试覆盖**：JUnit测试代码的质量和覆盖率

#### MyBatis分析相关
- **映射文件结构**：namespace、resultMap、sql片段、语句定义
- **SQL语句分析**：SELECT/INSERT/UPDATE/DELETE语句、参数映射、结果映射
- **性能优化**：SQL执行计划、索引使用、查询优化建议
- **安全性检查**：SQL注入风险、参数绑定检查

## 示例输出

### Java代码分析示例

*以下是完整的Java代码分析报告（默认输出所有内容，用户可选择只输出部分内容）：*

```
# Java代码分析报告：src/main/java/com/example/service/impl/UserServiceImpl.java

## 功能概述
- 提供用户管理相关的业务逻辑实现
- 包含用户CRUD操作、身份验证、权限管理等功能
- 使用Spring框架进行依赖注入和事务管理

## 代码结构
- `UserServiceImpl`类：实现UserService接口
- `@Service`注解：Spring服务层组件
- `@Autowired`依赖：注入UserMapper、RoleService等
- `@Transactional`方法：需要事务管理的方法

## 主要方法
- `getUserById(Long id)`: 根据ID获取用户信息
- `createUser(UserDTO userDTO)`: 创建新用户
- `updateUser(Long id, UserDTO userDTO)`: 更新用户信息
- `deleteUser(Long id)`: 删除用户（逻辑删除）
- `authenticate(String username, String password)`: 用户认证

## 潜在问题
1. 密码存储使用了MD5哈希，存在安全隐患，建议使用BCrypt
2. 删除操作为逻辑删除，但相关关联数据未处理
3. 缺少输入验证，可能导致业务逻辑错误
4. 事务边界设置不够精确，可能影响性能

## 优化建议
1. 使用BCryptPasswordEncoder对密码进行安全哈希处理
2. 完善删除操作的级联处理
3. 添加参数验证注解（如@Valid）
4. 优化事务管理，为只读操作添加readOnly=true
```

### Spring接口分析示例

*支持多种输入格式：类名+方法名、文件路径、接口路径。以下是以类名+方法名格式为例的完整分析报告（默认输出所有内容，用户可选择只输出部分内容）：*

```
# Spring接口分析报告：UserController.getUserById

## Spring接口基本信息
- **Controller类**：UserController
- **方法名**：getUserById
- **HTTP方法**：GET
- **路径**：/api/users/{id}
- **描述**：根据用户ID获取用户详细信息
- **注解**：@GetMapping("/{id}")、@ResponseBody

## 输入参数
### 路径参数
- `id` (Long): 用户ID，必填，通过@PathVariable绑定

### 查询参数
- `includeProfile` (Boolean, 可选): 是否包含用户资料信息，默认false
- `fields` (String, 可选): 需要返回的字段，逗号分隔

### 请求头
- `Authorization` (String): Bearer token，用于身份验证
- `Content-Type`: application/json

## 输出参数
### 成功响应 (200 OK)
```java
// 返回类型：ResponseEntity<UserResponseDTO>
{
  "success": true,
  "code": 200,
  "message": "success",
  "data": {
    "id": 12345,
    "username": "john_doe",
    "email": "john@example.com",
    "name": "John Doe",
    "createdAt": "2021-06-25T12:34:56.789Z",
    "profile": {
      "avatar": "https://example.com/avatar.jpg",
      "bio": "Software developer"
    }
  }
}
```

### 错误响应
- `401 Unauthorized`: Token无效或过期
- `404 Not Found`: 用户不存在
- `500 Internal Server Error`: 服务器内部错误

## Java代码调用顺序
1. **Controller层**：UserController.getUserById()接收请求
2. **参数验证**：Spring Validation验证@PathVariable参数
3. **Service层**：调用UserService.getUserById(id, includeProfile)
4. **业务逻辑**：UserServiceImpl处理业务规则和权限检查
5. **DAO层**：调用UserMapper.selectUserById(id)
6. **MyBatis映射层**：执行SQL查询，映射结果到User实体
7. **DTO转换**：将User实体转换为UserResponseDTO
8. **响应构建**：构建ResponseEntity返回给客户端

## 业务逻辑流程
1. 从路径参数中提取用户ID
2. 验证当前用户是否有权限访问该用户信息（权限检查）
3. 调用UserService.getUserById获取用户数据
4. 如果查询结果为空，抛出UserNotFoundException
5. 根据includeProfile参数决定是否加载用户资料
6. 转换实体为DTO，隐藏敏感字段（如密码）
7. 构建统一的响应格式返回

## 第三方接口调用
### 内部服务调用
- **用户服务**：调用UserService.getUserById(id, includeProfile)
- **权限服务**：调用PermissionService.checkUserAccess(userId, currentUserId)
- **缓存服务**：调用CacheService.getUserCache(id)

### 外部API调用
- **无**：此接口不依赖外部第三方API

## MyBatis数据库操作
### Mapper接口
- **接口**：UserMapper
- **方法**：selectUserById(@Param("id") Long id)

### SQL语句
```xml
<!-- src/main/resources/mapper/UserMapper.xml -->
<select id="selectUserById" resultMap="UserResultMap">
    SELECT 
        u.id, u.username, u.email, u.name, u.created_at,
        p.avatar, p.bio
    FROM users u
    LEFT JOIN user_profiles p ON u.id = p.user_id
    WHERE u.id = #{id} AND u.deleted = 0
</select>
```

### 查询表
- **主表**：users
- **关联表**：user_profiles (通过user_id关联)

### 返回字段
- **用户表字段**：id, username, email, name, created_at
- **资料表字段**：avatar, bio
- **排除字段**：password, deleted等敏感字段

## 性能考虑
1. **数据库索引**：users.id有主键索引，user_profiles.user_id有外键索引
2. **查询优化**：使用LEFT JOIN避免N+1查询问题
3. **缓存策略**：用户数据可缓存，减少数据库查询
4. **连接池配置**：使用DBCP2连接池管理数据库连接

## 安全考虑
1. **身份验证**：必须提供有效的Token
2. **权限验证**：只能访问自己的用户信息或具有相应权限
3. **数据过滤**：排除敏感字段，如密码、删除标记等
4. **SQL注入防护**：使用#{}参数绑定防止SQL注入

## Spring事务管理
- **事务注解**：在Service层使用@Transactional
- **传播机制**：PROPAGATION_REQUIRED（默认）
- **隔离级别**：READ_COMMITTED（默认）
- **只读事务**：查询操作为只读事务

### MyBatis映射文件分析示例

```
# MyBatis映射文件分析报告：src/main/resources/mapper/OrderMapper.xml

## 文件概述
- **namespace**：com.example.mapper.OrderMapper
- **包含语句**：8个SQL语句（4个SELECT，2个INSERT，2个UPDATE）
- **结果映射**：3个resultMap定义

## SQL语句分析
### selectOrderById
```xml
<select id="selectOrderById" parameterType="Long" resultMap="OrderResultMap">
    SELECT * FROM orders WHERE id = #{id}
</select>
```
**分析**：
- 使用#{}参数绑定，防止SQL注入
- 查询所有字段（SELECT *），建议指定具体字段
- 缺少deleted字段过滤逻辑删除的记录

### selectOrdersByUserId
```xml
<select id="selectOrdersByUserId" parameterType="Long" resultMap="OrderResultMap">
    SELECT o.*, u.name as user_name 
    FROM orders o 
    JOIN users u ON o.user_id = u.id 
    WHERE o.user_id = #{userId} AND o.status != 'DELETED'
    ORDER BY o.created_at DESC
</select>
```
**分析**：
- 使用JOIN关联查询，性能较好
- 添加了状态过滤，避免查询已删除订单
- 按创建时间倒序排序，符合业务需求
- 建议添加分页支持

## 性能优化建议
1. **索引建议**：为orders.user_id、orders.created_at添加复合索引
2. **分页支持**：添加LIMIT子句支持分页查询
3. **字段优化**：避免使用SELECT *，指定具体字段
4. **缓存考虑**：可考虑添加二级缓存配置

## 安全性检查
1. **SQL注入防护**：所有语句都使用#{}参数绑定，安全
2. **权限控制**：需要在业务层进行权限验证
3. **数据过滤**：逻辑删除的记录已过滤，良好
```

### 代码修改示例

```
# Java代码修改建议：src/main/java/com/example/controller/UserController.java

## 原代码
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @Autowired
    private UserService userService;
    
    @GetMapping("/{id}")
    public User getUserById(@PathVariable Long id) {
        return userService.getUserById(id);
    }
}
```

## 修改后代码
```java
@RestController
@RequestMapping("/api/users")
@Validated
public class UserController {
    
    @Autowired
    private UserService userService;
    
    @GetMapping("/{id}")
    public ResponseEntity<BaseResponse<UserResponseDTO>> getUserById(
            @PathVariable @Min(1) Long id,
            @RequestParam(required = false) Boolean includeProfile) {
        
        UserResponseDTO user = userService.getUserById(id, includeProfile != null ? includeProfile : false);
        return ResponseEntity.ok(BaseResponse.success(user));
    }
}
```

## 修改说明
1. 添加@Validated注解支持参数验证
2. 为id参数添加@Min(1)验证，确保ID大于0
3. 添加includeProfile查询参数，支持可选包含用户资料
4. 统一响应格式为BaseResponse，包含success、code、message、data字段
5. 使用ResponseEntity包装响应，支持HTTP状态码
```

---

*使用本技能可以深入分析Java后台代码，理解Spring框架和MyBatis的实现细节，快速定位问题和优化代码。特别适用于复杂的Java企业级应用。*
