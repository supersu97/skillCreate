---
name: "backend-development-helper"
description: "后端开发助手。支持 Spring Boot、Django、Express、Go 等主流后端框架的开发任务。当需要修改后端代码、新增 API 接口、修改业务逻辑、数据库变更或修复后端 Bug 时调用。"
---

# 后端开发助手

*本技能专为 Claude Code 设计，帮助开发者处理后端项目的开发任务。*

## 模块文件说明

| 模块文件 | 说明 |
|---------|------|
| [SKILL.md](SKILL.md) | 核心流程主文件（步骤 1-7） |
| [project-collector.md](project-collector.md) | 后端项目位置收集模块 |
| [cache-handler.md](cache-handler.md) | 缓存处理模块 |
| [project-analyzer.md](project-analyzer.md) | 后端项目分析模块 |
| [requirement-collector.md](requirement-collector.md) | 后端需求收集模块 |
| [planning.md](planning.md) | 规划模块（轻量 + 深度） |
| [backend-executor.md](backend-executor.md) | 后端代码执行模块 |
| [error-handler.md](error-handler.md) | 错误处理模块 |

## ⚠️ 模块加载规则（CRITICAL）

以下模块需要使用 Read 工具读取：
- 项目位置收集（project-collector.md）
- 缓存处理（cache-handler.md）
- 项目分析（project-analyzer.md）
- 需求收集（requirement-collector.md）
- 规划模块（planning.md）
- 执行模块（backend-executor.md）
- 错误处理（error-handler.md）

模块文件的基础路径为本 SKILL.md 所在目录（通过技能加载时的 "Base directory for this skill" 获取）。

## 🚫 技能维护规范（CRITICAL）

- ❌ 禁止创建 CHANGELOG.md 等辅助文档
- ✅ 只保留上述"模块文件说明"表格中列出的功能模块文件

## 进度跟踪

本技能使用 TaskCreate/TaskUpdate 跟踪执行进度。

## 执行指令（CRITICAL）

### 步骤 0：创建主任务并初始化

```javascript
调用 TaskCreate 工具：
{
  "subject": "后端开发任务",
  "description": "执行后端开发助手"
}
```

### 步骤 1：智能需求分析

**目标**：通过一次询问获取核心信息，智能推断需求类型和修改方式

#### 1.1 询问用户需求

使用 AskUserQuestion 工具：

```javascript
{
  "questions": [{
    "question": "请描述你要做什么？（一句话即可）",
    "header": "需求描述"
  }]
}
```

#### 1.2 智能推断

根据用户描述，智能推断以下信息：

**需求类型推断规则**：
- 包含"新增"/"添加"/"创建"/"开发" → 新增功能
- 包含"修改"/"更新"/"调整"/"改" → 修改现有功能
- 包含"bug"/"错误"/"问题"/"修复" → Bug修复
- 包含"优化"/"性能"/"重构" → 性能优化
- 无法判断 → 默认为"修改现有功能"

**修改方式推断规则**：
- 包含接口路径（如"/api/xxx"） → 根据接口信息修改
- 包含类名或文件名 → 指定文件修改
- 包含"新增"/"创建" → 新增API/功能
- 其他 → 直接描述修改

#### 1.3 确认推断结果

显示推断结果并询问用户确认：

```
## 需求分析结果

根据你的描述，我推断：
- 需求类型：[新增功能/修改现有功能/Bug修复/性能优化]
- 修改方式：[直接描述/根据接口/指定文件/新增]

是否正确？如果不正确，请说明需要调整的地方。
```

**更新任务进度**：
```javascript
调用 TaskUpdate 工具：
{
  "taskId": [主任务ID],
  "status": "in_progress",
  "description": "步骤1完成：需求分析 - [需求类型]"
}
```

### 步骤 2：收集后端项目位置

**优先检查当前工作目录**

#### 2.1 智能检测当前目录

```javascript
调用 Glob 工具：
- path: .
- pattern: "pom.xml"

调用 Glob 工具：
- path: .
- pattern: "build.gradle"

调用 Glob 工具：
- path: .
- pattern: "requirements.txt"

调用 Glob 工具：
- path: .
- pattern: "go.mod"
```

#### 2.2 根据检测结果处理

**情况A：当前目录是后端项目**

```
检测到当前目录是后端项目：
- 项目路径：[当前目录]
- 项目类型：[Java/Python/Go 等]

是否使用当前目录作为项目路径？
```

如果用户确认，跳过项目位置收集，直接进入步骤 3。

**情况B：当前目录不是后端项目**

必须使用 Read 工具读取 `[技能基础路径]/project-collector.md` 文件，然后严格按照文件中的流程执行。

**更新任务进度**：
```javascript
调用 TaskUpdate 工具：
{
  "taskId": [主任务ID],
  "description": "步骤2完成：项目位置 - [项目路径]"
}
```

### 步骤 3：检查和分析项目

#### 3.1 检查项目缓存

**必须先使用 Read 工具读取 `[技能基础路径]/cache-handler.md` 文件**，按照"缓存检查流程"执行。

如果缓存存在且用户选择使用缓存，跳过步骤 3.2 和 3.3，直接进入步骤 4。

#### 3.2 分析项目结构

**如果使用了缓存，跳过本步骤**

**必须先使用 Read 工具读取 `[技能基础路径]/project-analyzer.md` 文件**，按照"后端项目结构分析"部分执行。

#### 3.3 分析代码风格

**如果使用了缓存，跳过本步骤**

**必须先使用 Read 工具读取 `[技能基础路径]/project-analyzer.md` 文件**，按照"后端代码风格分析"部分执行。

生成后端代码风格指南，后续所有代码生成必须严格遵循。

#### 3.4 保存项目缓存

**如果完成了步骤 3.2 和 3.3（未使用缓存），必须保存缓存**

**必须先使用 Read 工具读取 `[技能基础路径]/cache-handler.md` 文件**，按照"缓存保存流程"执行。

**更新任务进度**：
```javascript
调用 TaskUpdate 工具：
{
  "taskId": [主任务ID],
  "description": "步骤3完成：项目分析 - [框架] - [代码风格]"
}
```

### 步骤 4：收集详细后端需求

**必须先使用 Read 工具读取 `[技能基础路径]/requirement-collector.md` 文件**，然后严格按照文件中的流程执行。

包含数据库变更需求和第三方接口对接需求的收集。

**更新任务进度**：
```javascript
调用 TaskUpdate 工具：
{
  "taskId": [主任务ID],
  "description": "步骤4完成：需求收集 - [详细需求摘要]"
}
```

### 步骤 5：规划阶段

**在需求收集后、代码执行前，必须执行规划阶段**

#### 5.1 判断任务复杂度（自动判断）

**简单任务（跳过规划）**：
- 修改 1-2 个文件
- Bug 修复
- 简单字段调整

**中等任务（轻量规划）**：
- 修改 3-5 个文件
- 修改现有 API 逻辑
- 小范围优化

**复杂任务（深度规划）**：
- 新增功能模块
- 修改 6+ 个文件
- 涉及数据库变更
- 新增完整的 API 接口链路

#### 5.2 执行对应的规划流程

**简单任务**：跳过规划，直接进入步骤 6

**中等任务或复杂任务**：
- 必须使用 Read 工具读取 `[技能基础路径]/planning.md`
- 执行对应规划流程
- 用户确认后继续

**更新任务进度**：
```javascript
调用 TaskUpdate 工具：
{
  "taskId": [主任务ID],
  "description": "步骤5完成：规划阶段 - [规划类型] - 已确认"
}
```

### 步骤 6：执行后端代码修改

**必须先使用 Read 工具读取 `[技能基础路径]/backend-executor.md` 文件**，然后严格按照文件中的流程执行。

**错误处理**：
如果执行过程中出现错误，必须使用 Read 工具读取 `[技能基础路径]/error-handler.md` 文件，按照错误处理流程执行。

**更新任务进度**：
```javascript
调用 TaskUpdate 工具：
{
  "taskId": [主任务ID],
  "description": "步骤6完成：后端代码修改 - [修改文件列表]"
}
```

### 步骤 7：完成并总结

**标记任务完成**：
```javascript
调用 TaskUpdate 工具：
{
  "taskId": [主任务ID],
  "status": "completed",
  "description": "后端开发任务完成"
}
```

**生成完成摘要**：
```
## 后端开发任务完成

### 修改内容
- [具体修改内容摘要]

### 涉及文件
1. [文件路径] - [修改说明]
2. [文件路径] - [修改说明]

### 数据库变更
- [如有，列出SQL语句或迁移脚本]

### 技术约束
- 已严格遵循项目现有代码风格
- 已遵循用户指定的修改范围

### 后续建议
- [如有需要，提供测试建议]
- [如有需要，提供部署建议]
```

## 接口文档处理流程（如有需要）

如果用户提供了接口文档，按照以下流程处理：

**情况 A：本地文件**
```
1. 读取本地接口文档文件
   调用 Read 工具：
   - file_path: [用户提供的本地文件路径]

2. 解析文档格式
   - YAML 文件：使用 YAML 解析器
   - JSON 文件（OpenAPI/Swagger）：解析 JSON 结构
   - Markdown 文件：提取接口定义

3. 提取接口信息
   - 接口路径和方法
   - 请求参数和类型
   - 响应结构和状态码
   - 认证要求
```

**情况 B：暂无接口文档**
```
1. 根据需求设计接口
   - 分析业务数据模型
   - 设计 RESTful API 结构
   - 确定请求/响应格式

2. 生成接口设计文档
   - 接口列表
   - 参数说明
   - 响应示例
   - 错误码定义
```
