---
name: "code-refactor-assistant"
description: "帮助前端开发者重构后台代码，提高代码质量和可维护性。当需要优化代码结构、消除重复代码、提高可读性或遵循最佳实践时调用。"
---

# 代码重构助手

## 功能介绍

这个技能专注于帮助前端开发者进行后台代码重构，提供以下功能：

1. **代码结构优化**：分析并优化代码的整体结构和组织方式
2. **重复代码消除**：识别并提取重复代码，实现代码复用
3. **可读性提升**：改进代码风格、命名规范和注释质量
4. **性能优化**：识别并修复性能瓶颈
5. **架构改进**：提供代码架构的改进建议
6. **最佳实践应用**：确保代码遵循行业最佳实践

## 何时调用

当你遇到以下情况时，应该调用这个技能：

- 后台代码结构混乱，难以理解和维护
- 存在大量重复代码，需要提取和重构
- 代码性能不佳，需要优化
- 代码风格不一致，需要统一
- 想要提升代码质量和可维护性
- 准备进行大型代码重构但不确定从何开始

## 使用方法

### 基本用法

1. **指定重构范围**：明确指出需要重构的代码文件或目录
2. **描述重构目标**：详细说明你想要达到的重构目标
3. **提供约束条件**：如果有任何技术或业务约束，一并提供
4. **等待重构建议**：AI会分析代码并提供详细的重构方案
5. **应用重构方案**：可以直接使用AI生成的重构代码

### 高级用法

#### 1. 整体重构

```
请对以下目录中的代码进行整体重构，提高代码质量和可维护性：
src/controllers/
```

#### 2. 具体问题重构

```
请重构以下代码，消除重复逻辑并提高可读性：
src/services/userService.js
```

#### 3. 性能优化重构

```
请重构以下代码，优化其性能：
src/utils/dataProcessor.js
```

#### 4. 架构改进

```
请分析并改进以下代码的架构设计：
src/api/
```

#### 5. 代码风格统一

```
请重构以下代码，使其符合项目的代码风格规范：
src/models/
```

## 重构原则

这个技能遵循以下重构原则：

1. **保持行为不变**：重构后的代码应保持原有功能不变
2. **小步前进**：将大型重构分解为小的、可管理的步骤
3. **测试覆盖**：提供测试建议，确保重构不会破坏现有功能
4. **渐进式改进**：优先解决最关键的问题，然后逐步改进
5. **代码质量**：注重代码的可读性、可维护性和可扩展性

## 支持的重构类型

### 1. 代码结构重构
- 提取函数和方法
- 合并相似函数
- 重新组织代码文件和目录结构
- 优化模块依赖关系

### 2. 性能重构
- 优化算法和数据结构
- 减少不必要的计算和IO操作
- 实现缓存策略
- 优化数据库查询

### 3. 可读性重构
- 改进命名规范
- 添加清晰的注释和文档
- 优化代码缩进和格式
- 简化复杂的条件语句

### 4. 架构重构
- 应用设计模式
- 实现依赖注入
- 优化模块间通信
- 改进错误处理机制

## 输出格式

使用此技能时，你将获得：

1. **重构分析**：详细的代码分析和问题识别
2. **重构方案**：具体的重构步骤和方法
3. **重构前后对比**：展示重构前后的代码差异
4. **重构理由**：解释为什么需要进行这样的重构
5. **测试建议**：确保重构正确性的测试策略

## 示例输出

### 代码重构示例

```
# 代码重构分析：src/services/userService.js

## 重构前代码
```javascript
function getUserById(id) {
  return db.query('SELECT * FROM users WHERE id = ?', [id]);
}

function getUserByEmail(email) {
  return db.query('SELECT * FROM users WHERE email = ?', [email]);
}

function getUserByUsername(username) {
  return db.query('SELECT * FROM users WHERE username = ?', [username]);
}

function createUser(userData) {
  return db.query('INSERT INTO users SET ?', [userData]);
}

function updateUser(id, userData) {
  return db.query('UPDATE users SET ? WHERE id = ?', [userData, id]);
}

function deleteUser(id) {
  return db.query('DELETE FROM users WHERE id = ?', [id]);
}
```

## 重构问题分析
1. **重复代码**：多个查询函数使用相似的模式
2. **缺少抽象**：直接使用原始SQL查询，缺少抽象层
3. **错误处理**：缺少错误处理机制
4. **类型安全**：缺少参数验证和类型检查

## 重构后代码
```javascript
class UserService {
  constructor(db) {
    this.db = db;
  }

  async getUserByField(field, value) {
    try {
      const [rows] = await this.db.query(
        `SELECT * FROM users WHERE ${field} = ?`, 
        [value]
      );
      return rows[0];
    } catch (error) {
      console.error(`Error getting user by ${field}:`, error);
      throw new Error(`Failed to get user by ${field}`);
    }
  }

  async getUserById(id) {
    return this.getUserByField('id', id);
  }

  async getUserByEmail(email) {
    return this.getUserByField('email', email);
  }

  async getUserByUsername(username) {
    return this.getUserByField('username', username);
  }

  async createUser(userData) {
    try {
      const [result] = await this.db.query('INSERT INTO users SET ?', [userData]);
      return { id: result.insertId, ...userData };
    } catch (error) {
      console.error('Error creating user:', error);
      throw new Error('Failed to create user');
    }
  }

  async updateUser(id, userData) {
    try {
      await this.db.query('UPDATE users SET ? WHERE id = ?', [userData, id]);
      return await this.getUserById(id);
    } catch (error) {
      console.error('Error updating user:', error);
      throw new Error('Failed to update user');
    }
  }

  async deleteUser(id) {
    try {
      const [result] = await this.db.query('DELETE FROM users WHERE id = ?', [id]);
      return result.affectedRows > 0;
    } catch (error) {
      console.error('Error deleting user:', error);
      throw new Error('Failed to delete user');
    }
  }
}

module.exports = UserService;
```

## 重构说明
1. **使用类结构**：创建UserService类，提供更好的封装和组织
2. **提取通用方法**：创建getUserByField方法，消除重复的查询逻辑
3. **添加错误处理**：为所有方法添加try-catch块，提供更好的错误处理
4. **使用异步/ await**：采用现代的异步编程模式
5. **改进返回值**：为create和update方法提供更有用的返回值
6. **依赖注入**：通过构造函数注入数据库连接，提高可测试性

## 测试建议
1. **单元测试**：为每个方法编写单元测试，确保功能正确性
2. **集成测试**：测试与数据库的交互
3. **错误处理测试**：测试错误情况下的行为
4. **边界情况测试**：测试边界输入和异常情况
```