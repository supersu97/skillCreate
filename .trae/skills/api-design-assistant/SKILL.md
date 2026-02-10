---
name: "api-design-assistant"
description: "帮助前端开发者设计和优化后台API接口，确保API的一致性、安全性和性能。当需要设计新API、优化现有API或解决API相关问题时调用。"
---

# API设计助手

## 功能介绍

这个技能专注于帮助前端开发者设计和优化后台API接口，提供以下功能：

1. **API设计规范**：提供RESTful API设计的最佳实践和规范
2. **API结构设计**：帮助设计合理的API路由结构和资源命名
3. **请求/响应设计**：优化API的请求参数和响应格式
4. **错误处理设计**：设计统一的错误处理机制
5. **安全性设计**：提供API安全最佳实践
6. **性能优化**：优化API性能和响应时间
7. **API文档生成**：生成清晰的API文档
8. **版本控制策略**：提供API版本控制的最佳实践

## 何时调用

当你遇到以下情况时，应该调用这个技能：

- 需要设计新的后台API接口
- 现有API设计不合理，需要优化
- API响应格式不一致，需要统一
- API安全性存在问题，需要改进
- API性能不佳，需要优化
- 缺少清晰的API文档
- 准备进行API版本升级

## 使用方法

### 基本用法

1. **描述API需求**：详细说明你需要设计的API功能和业务逻辑
2. **提供技术约束**：如果有任何技术栈或架构约束，一并提供
3. **指定设计目标**：明确你想要达到的API设计目标
4. **等待设计方案**：AI会分析需求并提供详细的API设计方案
5. **应用设计方案**：可以直接使用AI生成的API设计和实现代码

### 高级用法

#### 1. 新API设计

```
请设计一个用户管理系统的API，包含用户注册、登录、获取用户信息、更新用户信息和删除用户等功能。
```

#### 2. API优化

```
请优化以下API，提高其性能和安全性：
GET /api/users?page=1&limit=10
```

#### 3. 响应格式统一

```
请设计一个统一的API响应格式，适用于所有API接口。
```

#### 4. 错误处理设计

```
请设计一个统一的API错误处理机制。
```

#### 5. API文档生成

```
请为以下API生成详细的文档：
src/routes/userRoutes.js
```

## API设计原则

这个技能遵循以下API设计原则：

1. **RESTful设计**：遵循REST架构风格，使用合适的HTTP方法和状态码
2. **一致性**：保持API设计的一致性，包括命名规范、响应格式等
3. **安全性**：优先考虑API的安全性，包括认证、授权、输入验证等
4. **性能**：优化API性能，减少响应时间和资源消耗
5. **可维护性**：设计易于理解和维护的API
6. **可扩展性**：考虑未来的功能扩展和版本控制
7. **清晰性**：提供清晰的API文档和错误信息

## 支持的API类型

### 1. RESTful API
- 基于HTTP方法的资源操作
- 标准的状态码使用
- 资源导向的设计

### 2. GraphQL API
- 灵活的数据查询
- 减少过度获取和不足获取
- 类型系统和模式定义

### 3. WebSocket API
- 实时通信
- 双向数据传输
- 长连接管理

## 输出格式

使用此技能时，你将获得：

1. **API设计方案**：详细的API设计文档
2. **路由结构**：API的路由定义和结构
3. **请求/响应示例**：API的请求和响应示例
4. **实现代码**：API的具体实现代码
5. **安全建议**：API的安全最佳实践
6. **性能优化**：API的性能优化建议
7. **文档模板**：API文档的模板和示例

## 示例输出

### RESTful API设计示例

```
# 用户管理系统API设计

## 1. 设计概述

### 资源定义
- **用户**：系统中的用户实体
- **认证**：用户登录和认证相关操作

### 技术栈
- **后端框架**：Express.js
- **数据库**：MongoDB
- **认证**：JWT

## 2. API路由设计

### 认证相关
| HTTP方法 | 路径 | 描述 | 权限 |
|---------|------|------|------|
| POST | /api/auth/register | 用户注册 | 公开 |
| POST | /api/auth/login | 用户登录 | 公开 |
| GET | /api/auth/me | 获取当前用户信息 | 需要认证 |

### 用户管理
| HTTP方法 | 路径 | 描述 | 权限 |
|---------|------|------|------|
| GET | /api/users | 获取用户列表 | 需要认证 |
| GET | /api/users/:id | 获取单个用户信息 | 需要认证 |
| PUT | /api/users/:id | 更新用户信息 | 需要认证 |
| DELETE | /api/users/:id | 删除用户 | 需要管理员权限 |

## 3. 请求/响应设计

### 用户注册

**请求**：
```json
POST /api/auth/register
Content-Type: application/json

{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "securePassword123",
  "name": "John Doe"
}
```

**成功响应**：
```json
HTTP/1.1 201 Created
Content-Type: application/json

{
  "success": true,
  "data": {
    "_id": "60d5ec49f1a2c8001c8e4b7c",
    "username": "john_doe",
    "email": "john@example.com",
    "name": "John Doe",
    "createdAt": "2021-06-25T12:34:56.789Z"
  },
  "message": "User registered successfully"
}
```

**错误响应**：
```json
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": {
      "email": "Email is already registered",
      "password": "Password must be at least 6 characters"
    }
  }
}
```

## 4. 实现代码

### 认证路由 (src/routes/auth.js)

```javascript
const express = require('express');
const router = express.Router();
const authController = require('../controllers/authController');
const authMiddleware = require('../middleware/auth');

// 注册路由
router.post('/register', authController.register);

// 登录路由
router.post('/login', authController.login);

// 获取当前用户信息
router.get('/me', authMiddleware, authController.getMe);

module.exports = router;
```

### 认证控制器 (src/controllers/authController.js)

```javascript
const User = require('../models/User');
const jwt = require('jsonwebtoken');
const bcrypt = require('bcryptjs');

// 用户注册
exports.register = async (req, res) => {
  try {
    const { username, email, password, name } = req.body;

    // 验证输入
    if (!username || !email || !password || !name) {
      return res.status(400).json({
        success: false,
        error: {
          code: 'VALIDATION_ERROR',
          message: 'All fields are required'
        }
      });
    }

    // 检查邮箱是否已注册
    const existingUser = await User.findOne({ email });
    if (existingUser) {
      return res.status(400).json({
        success: false,
        error: {
          code: 'VALIDATION_ERROR',
          message: 'Email is already registered'
        }
      });
    }

    // 哈希密码
    const salt = await bcrypt.genSalt(10);
    const hashedPassword = await bcrypt.hash(password, salt);

    // 创建用户
    const user = await User.create({
      username,
      email,
      password: hashedPassword,
      name
    });

    // 生成token
    const token = jwt.sign(
      { id: user._id, role: user.role },
      process.env.JWT_SECRET,
      { expiresIn: '7d' }
    );

    // 隐藏密码
    user.password = undefined;

    res.status(201).json({
      success: true,
      data: user,
      token,
      message: 'User registered successfully'
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: {
        code: 'SERVER_ERROR',
        message: 'Server error'
      }
    });
  }
};

// 用户登录
exports.login = async (req, res) => {
  try {
    const { email, password } = req.body;

    // 验证输入
    if (!email || !password) {
      return res.status(400).json({
        success: false,
        error: {
          code: 'VALIDATION_ERROR',
          message: 'Email and password are required'
        }
      });
    }

    // 查找用户
    const user = await User.findOne({ email });
    if (!user) {
      return res.status(401).json({
        success: false,
        error: {
          code: 'AUTH_ERROR',
          message: 'Invalid email or password'
        }
      });
    }

    // 验证密码
    const isMatch = await bcrypt.compare(password, user.password);
    if (!isMatch) {
      return res.status(401).json({
        success: false,
        error: {
          code: 'AUTH_ERROR',
          message: 'Invalid email or password'
        }
      });
    }

    // 生成token
    const token = jwt.sign(
      { id: user._id, role: user.role },
      process.env.JWT_SECRET,
      { expiresIn: '7d' }
    );

    // 隐藏密码
    user.password = undefined;

    res.status(200).json({
      success: true,
      data: user,
      token,
      message: 'Login successful'
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: {
        code: 'SERVER_ERROR',
        message: 'Server error'
      }
    });
  }
};

// 获取当前用户信息
exports.getMe = async (req, res) => {
  try {
    const user = await User.findById(req.user.id);
    if (!user) {
      return res.status(404).json({
        success: false,
        error: {
          code: 'NOT_FOUND',
          message: 'User not found'
        }
      });
    }

    // 隐藏密码
    user.password = undefined;

    res.status(200).json({
      success: true,
      data: user
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: {
        code: 'SERVER_ERROR',
        message: 'Server error'
      }
    });
  }
};
```

## 5. 安全建议

1. **使用HTTPS**：确保所有API通信使用HTTPS
2. **密码加密**：使用bcrypt等安全算法对密码进行哈希处理
3. **JWT认证**：使用JSON Web Token进行身份验证
4. **输入验证**：对所有API输入进行严格验证
5. **速率限制**：对登录等敏感API实施速率限制
6. **CORS设置**：正确配置CORS策略
7. **敏感信息保护**：不在响应中包含敏感信息
8. **定期更新依赖**：保持依赖包的安全更新

## 6. 性能优化建议

1. **数据库索引**：为常用查询字段添加索引
2. **缓存策略**：对频繁访问的数据使用缓存
3. **请求限制**：实施合理的请求限制
4. **响应压缩**：启用HTTP响应压缩
5. **延迟加载**：对大型资源使用延迟加载
6. **批量操作**：支持批量API操作
7. **监控和分析**：实施API性能监控

## 7. API文档示例

### 使用Swagger文档

```yaml
swagger: "2.0"
info:
  version: "1.0.0"
  title: "User Management API"
  description: "API for user management system"

paths:
  /api/auth/register:
    post:
      summary: "Register a new user"
      tags: ["Auth"]
      parameters:
        - in: "body"
          name: "user"
          description: "User registration information"
          required: true
          schema:
            $ref: "#/definitions/UserRegister"
      responses:
        201:
          description: "User registered successfully"
          schema:
            $ref: "#/definitions/UserResponse"
        400:
          description: "Validation error"
          schema:
            $ref: "#/definitions/ErrorResponse"

  /api/auth/login:
    post:
      summary: "User login"
      tags: ["Auth"]
      parameters:
        - in: "body"
          name: "credentials"
          description: "User login credentials"
          required: true
          schema:
            $ref: "#/definitions/UserLogin"
      responses:
        200:
          description: "Login successful"
          schema:
            $ref: "#/definitions/UserResponse"
        401:
          description: "Invalid credentials"
          schema:
            $ref: "#/definitions/ErrorResponse"

definitions:
  UserRegister:
    type: "object"
    required:
      - "username"
      - "email"
      - "password"
      - "name"
    properties:
      username:
        type: "string"
      email:
        type: "string"
        format: "email"
      password:
        type: "string"
        minLength: 6
      name:
        type: "string"

  UserLogin:
    type: "object"
    required:
      - "email"
      - "password"
    properties:
      email:
        type: "string"
        format: "email"
      password:
        type: "string"

  UserResponse:
    type: "object"
    properties:
      success:
        type: "boolean"
      data:
        $ref: "#/definitions/User"
      token:
        type: "string"
      message:
        type: "string"

  User:
    type: "object"
    properties:
      _id:
        type: "string"
      username:
        type: "string"
      email:
        type: "string"
      name:
        type: "string"
      createdAt:
        type: "string"
        format: "date-time"

  ErrorResponse:
    type: "object"
    properties:
      success:
        type: "boolean"
        default: false
      error:
        $ref: "#/definitions/Error"

  Error:
    type: "object"
    properties:
      code:
        type: "string"
      message:
        type: "string"
      details:
        type: "object"
```
