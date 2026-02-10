---
name: "frontend-ai-code-analyzer"
description: "让前端开发者使用AI分析和修改后台代码，包括接口详细分析。当需要理解后台代码逻辑、分析接口流程、查找bug、优化性能或进行代码修改时调用。"
---

# 前端AI代码分析工具

*本技能专为Claude Code设计，提供强大的交互式代码分析体验。*

## 功能介绍

这个技能让前端开发者能够：

1. **代码分析**：自动分析后台代码结构、逻辑流程和依赖关系
2. **接口分析**：详细分析API接口的出入参、调用流程、第三方接口依赖和数据库操作
3. **问题定位**：快速识别代码中的bug、性能瓶颈和安全隐患
4. **代码修改**：基于需求智能生成和修改后台代码
5. **文档生成**：为复杂代码生成清晰的文档和注释
6. **技术咨询**：解答关于后台代码的技术问题

## 何时调用

当你遇到以下情况时，应该调用这个技能：

- 需要理解不熟悉的后台代码逻辑
- 后台代码出现bug需要定位和修复
- 想要优化后台代码性能
- 需要为前端功能添加或修改后台API
- 需要分析API接口的详细调用流程、出入参、第三方接口依赖和数据库操作
- 对后台技术实现有疑问需要解答
- 需要快速了解代码库的整体结构

## 使用方法

### 基本用法

1. **指定分析目标**：支持以下三种格式指定要分析的代码：
   - **接口路径**：如 `GET /api/users/:id`、`POST /api/orders`
   - **类名+方法名**：如 `UserController.getUserById`、`OrderService.createOrder`
   - **文件路径**：如 `src/api/user.js`、`src/controllers/userController.js`
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
1. 接口基本信息
2. 输入输出参数
3. 代码调用顺序
4. 数据库操作详情
5. 性能考虑
6. 安全考虑
7. 查看所有内容
8. 导出分析报告到文件
```

你可以输入对应的数字或选择多个选项来查看特定内容。

### 高级用法

#### 1. 代码分析

```
请分析以下后台代码文件，说明其功能和潜在问题：
src/api/user.js
```

#### 2. 接口分析

支持三种格式指定分析目标：

**格式一：接口路径**
```
请分析以下接口的详细调用流程：出入参、代码调用顺序、第三方接口调用、数据库操作等：
GET /api/users/:id
```

**格式二：类名+方法名**
```
请分析以下类方法的详细调用流程：出入参、代码调用顺序、第三方接口调用、数据库操作等：
UserController.getUserById
```

**格式三：文件路径**
```
请分析以下文件中所有接口的详细调用流程：出入参、代码调用顺序、第三方接口调用、数据库操作等：
src/controllers/userController.js
```

**输出内容选择方式**

### 交互式选择（推荐）
分析完成后，你可以在命令行中交互式选择要查看的内容：

```
请分析以下接口，完成分析后提供交互式选择：
GET /api/orders/:id
```

分析完成后，会显示类似以下的交互选项：

```
分析完成！请选择要查看的内容：
1. 接口基本信息
2. 输入输出参数  
3. 代码调用顺序
4. 逻辑流程
5. 第三方接口调用
6. 数据库操作详情
7. 性能考虑
8. 安全考虑
9. 查看完整报告
10. 导出报告到文件
```

### 预设模式选择
```
请使用【基础模式】分析以下接口：
GET /api/orders/:id
```

```
请使用【数据库模式】分析以下类方法：
OrderService.processPayment
```

```
请使用【安全模式】分析以下文件：
src/services/paymentService.js
```

### 自定义详细选择
```
请分析以下接口，只需要输出【接口基本信息】、【输入输出参数】和【数据库操作】：
GET /api/orders/:id
```

```
请分析以下类方法，只需要输出【代码调用顺序】和【逻辑流程】：
OrderService.processPayment
```

```
请分析以下文件，只需要输出【代码结构】和【问题识别】：
src/services/paymentService.js
```

#### 3. 问题定位

```
我的前端请求后台API时出现500错误，请分析以下代码找出问题：
src/controllers/userController.js
```

#### 4. 代码修改

```
请为以下代码添加一个新的API端点，用于获取用户统计数据：
src/routes/userRoutes.js
```

#### 5. 性能优化

```
请分析并优化以下代码的性能：
src/services/dataProcessor.js
```

#### 6. 技术咨询

```
这个后台代码使用了什么设计模式？有什么优缺点？
src/utils/auth.js
```

## 支持的后台技术栈

- **语言**：JavaScript/TypeScript、Python、Java、C#、PHP等
- **框架**：Express、Koa、Django、Flask、Spring Boot、ASP.NET等
- **数据库**：MySQL、PostgreSQL、MongoDB、Redis等
- **其他**：RESTful API、GraphQL、WebSocket等

## 注意事项

1. **代码安全性**：分析时会考虑代码的安全性，避免生成有安全隐患的代码
2. **代码风格**：会保持与原有代码一致的风格和命名规范
3. **向后兼容**：生成的代码修改会尽量保持向后兼容性
4. **性能考虑**：会考虑代码的性能影响，避免生成低效代码
5. **测试建议**：会提供相应的测试建议，确保代码修改的正确性

## 输出格式

你可以选择性地指定需要哪些分析内容。如果未指定，默认返回所有分析内容。

### 可选择的输出项目
1. **代码分析报告**：详细的代码结构和逻辑分析
2. **接口分析报告**：详细的接口调用流程、出入参、第三方接口依赖和数据库操作分析
3. **问题识别**：明确指出代码中的问题和潜在风险
4. **修改建议**：具体的代码修改方案和理由
5. **技术解释**：对复杂技术概念的清晰解释
6. **最佳实践**：相关的代码最佳实践建议

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
1. 接口基本信息
2. 输入输出参数  
3. 代码调用顺序
4. 逻辑流程
5. 第三方接口调用
6. 数据库操作详情
7. 性能考虑
8. 安全考虑
9. 查看完整报告
10. 导出报告到文件
```

**触发交互式选择：**
```
请分析以下接口，完成分析后提供交互式选择：
GET /api/users/:id
```

#### 方式二：预设分析模式
提供以下预设分析模式，只需选择模式名称即可：

1. **基础模式**：仅输出核心信息
   - 包含：接口基本信息、输入输出参数
   - 适合：快速了解接口基本功能

2. **详细模式**：输出完整分析内容（默认）
   - 包含：所有分析内容
   - 适合：全面深入的代码分析

3. **数据库模式**：重点关注数据库操作
   - 包含：数据库操作、性能考虑、输入输出参数
   - 适合：分析数据库相关接口

4. **安全模式**：重点关注安全问题
   - 包含：安全考虑、问题识别、输入输出参数
   - 适合：安全审计和漏洞分析

5. **性能模式**：重点关注性能问题
   - 包含：性能考虑、代码调用顺序、逻辑流程
   - 适合：性能优化和分析

**使用示例：**
```
请使用【基础模式】分析以下接口：
GET /api/users/:id
```

```
请使用【数据库模式】分析以下类方法：
OrderService.createOrder
```

#### 方式三：自定义详细选择
如果你需要更精细的控制，可以指定具体的分析内容：

```
请分析以下接口，只需要输出【接口基本信息】和【输入输出参数】：
GET /api/users/:id
```

或者使用逗号分隔多个项目：

```
请分析以下代码，只需要输出【代码结构】和【问题识别】：
src/controllers/userController.js
```

### 完整的分析内容分类
以下是可按类别选择的分析内容：

#### 接口分析相关
- **接口基本信息**：HTTP方法、路径、描述
- **输入输出参数**：路径参数、查询参数、请求头、响应格式
- **代码调用顺序**：路由层 → 中间件层 → 控制器层 → 服务层 → 数据访问层
- **逻辑流程**：从请求到响应的完整处理步骤
- **第三方接口调用**：内部服务调用和外部API依赖
- **数据库操作**：查询表、关联关系、SQL条件、返回字段
- **性能考虑**：索引、缓存、查询优化
- **安全考虑**：身份验证、权限验证、数据过滤

#### 代码分析相关
- **代码结构**：函数、类、模块的组织结构
- **功能概述**：代码的主要功能和用途
- **依赖关系**：代码之间的调用和依赖关系
- **代码风格**：命名规范、注释质量、代码格式
- **性能瓶颈**：识别可能的性能问题
- **安全问题**：识别安全漏洞和风险
- **测试覆盖**：测试代码的质量和覆盖率

## 示例输出

### 代码分析示例

*以下是完整的代码分析报告（默认输出所有内容，用户可选择只输出部分内容）：*

```
# 代码分析报告：src/api/user.js

## 功能概述
- 提供用户认证和管理相关的API端点
- 包含登录、注册、获取用户信息等功能
- 使用JWT进行身份验证

## 代码结构
- `login()`: 用户登录函数
- `register()`: 用户注册函数
- `getUserInfo()`: 获取用户信息函数
- `updateUser()`: 更新用户信息函数

## 潜在问题
1. 密码存储使用了明文，存在安全隐患
2. 缺少输入验证，可能导致注入攻击
3. 错误处理不够完善，可能泄露敏感信息

## 优化建议
1. 使用bcrypt等库对密码进行哈希处理
2. 添加输入验证中间件
3. 统一错误处理机制，避免泄露敏感信息
```

### 接口分析示例

*支持三种输入格式：接口路径、类名+方法名、文件路径。以下是以接口路径格式为例的完整分析报告（默认输出所有内容，用户可选择只输出部分内容）：*

```
# 接口分析报告：GET /api/users/:id

## 接口基本信息
- **HTTP方法**：GET
- **路径**：/api/users/:id
- **描述**：获取指定ID的用户详细信息

## 输入参数
### 路径参数
- `id` (string): 用户ID，必填

### 查询参数
- `fields` (string, 可选): 需要返回的字段，逗号分隔
- `expand` (string, 可选): 需要展开的相关数据，如"profile,posts"

### 请求头
- `Authorization` (string): Bearer token，用于身份验证
- `Content-Type`: application/json

## 输出参数
### 成功响应 (200 OK)
```json
{
  "success": true,
  "data": {
    "_id": "60d5ec49f1a2c8001c8e4b7c",
    "username": "john_doe",
    "email": "john@example.com",
    "name": "John Doe",
    "profile": {
      "avatar": "https://example.com/avatar.jpg",
      "bio": "Software developer"
    },
    "createdAt": "2021-06-25T12:34:56.789Z"
  }
}
```

### 错误响应
- `401 Unauthorized`: Token无效或过期
- `404 Not Found`: 用户不存在
- `500 Internal Server Error`: 服务器内部错误

## 代码调用顺序
1. **路由层**：src/routes/userRoutes.js → router.get('/:id', userController.getUserById)
2. **中间件层**：authMiddleware验证Token，解析用户身份
3. **控制器层**：src/controllers/userController.js → getUserById函数
4. **服务层**：src/services/userService.js → getUserById函数
5. **数据访问层**：src/models/User.js → findById查询
6. **响应层**：格式化响应数据，返回给客户端

## 逻辑流程
1. 验证请求头中的Authorization token
2. 从路径参数中提取用户ID
3. 验证当前用户是否有权限访问该用户信息
4. 从数据库查询用户数据
5. 如果查询结果为空，返回404错误
6. 如果查询成功，格式化用户数据（隐藏敏感字段）
7. 返回200响应和用户数据

## 第三方接口调用
### 内部服务调用
- **用户服务**：调用UserService.getUserById(id)
- **权限服务**：调用PermissionService.checkUserAccess(userId, currentUserId)

### 外部API调用
- **无**：此接口不依赖外部第三方API

## 数据库操作
### 查询表
- **主表**：users
- **关联表**：profiles (通过user_id关联)

### 查询条件
```sql
SELECT 
  u._id, u.username, u.email, u.name, u.created_at,
  p.avatar, p.bio
FROM users u
LEFT JOIN profiles p ON u._id = p.user_id
WHERE u._id = :userId AND u.deleted_at IS NULL
```

### 返回字段
- **用户表字段**：_id, username, email, name, created_at
- **资料表字段**：avatar, bio
- **排除字段**：password, reset_token, deleted_at等敏感字段

## 性能考虑
1. **数据库索引**：users._id有主键索引，profiles.user_id有外键索引
2. **查询优化**：使用LEFT JOIN避免N+1查询问题
3. **缓存策略**：用户数据可缓存，减少数据库查询
4. **响应大小**：只返回必要字段，控制响应数据量

## 安全考虑
1. **身份验证**：必须提供有效的Token
2. **权限验证**：只能访问自己的用户信息或具有相应权限
3. **数据过滤**：排除敏感字段，如密码、token等
4. **输入验证**：验证用户ID格式，防止注入攻击
```

### 代码修改示例

```
# 代码修改建议：src/routes/userRoutes.js

## 原代码
```javascript
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');

router.post('/login', userController.login);
router.post('/register', userController.register);
router.get('/info', userController.getUserInfo);

module.exports = router;
```

## 修改后代码
```javascript
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');
const authMiddleware = require('../middleware/auth');

router.post('/login', userController.login);
router.post('/register', userController.register);
router.get('/info', authMiddleware, userController.getUserInfo);
router.get('/stats', authMiddleware, userController.getUserStats);

module.exports = router;
```

## 修改说明
1. 添加了认证中间件，保护需要登录才能访问的路由
2. 新增了`/stats`端点，用于获取用户统计数据
3. 保持了原有代码的风格和结构

## 控制器修改建议
需要在`userController.js`中添加`getUserStats`函数：

```javascript
// 在userController.js中添加
const getUserStats = async (req, res) => {
  try {
    const userId = req.user.id;
    const stats = await UserService.getUserStatistics(userId);
    res.status(200).json({ success: true, data: stats });
  } catch (error) {
    res.status(500).json({ success: false, message: '获取统计数据失败' });
  }
};

module.exports = {
  // 其他函数...
  getUserStats
};
```
```