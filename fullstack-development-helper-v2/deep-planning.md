# 深度规划模块

本模块用于新增功能模块等复杂任务的深度规划，使用 Plan Mode 进行详细的需求分析和架构设计。

## 适用场景

- 新增功能模块
- 修改 6+ 个文件
- 涉及数据库变更
- 前后端联动的复杂功能

## 规划流程

### 1. 进入 Plan Mode

使用 EnterPlanMode 工具进入规划模式：

```javascript
调用 EnterPlanMode 工具
```

### 2. 需求分析与模块划分

在 Plan Mode 中执行详细的需求分析，参考 v1 版本 planning-phase.md 的第 3.2 节内容。

### 3. 技术方案设计

为每个模块确定实现思路和技术选型，参考 v1 版本 planning-phase.md 的第 3.3 节内容。

### 4. 任务优先级排序

根据依赖关系和重要性确定实现顺序，参考 v1 版本 planning-phase.md 的第 3.5 节内容。

### 5. 生成实施计划文档

将所有内容整合为一份完整的实施计划文档，保存到项目目录：

```javascript
调用 Write 工具：
- file_path: [后端项目路径]/docs/implementation-plan-[YYYYMMDD-HHmmss].md
- content: [整合所有规划内容]
```

### 6. 退出 Plan Mode 并用户确认

使用 ExitPlanMode 工具退出规划模式，并将实施计划提交给用户审核。

---

**相关引用**

- 完整的深度规划流程：参考 v1 版本 planning-phase.md 第 3.2-3.8 节
- 需求收集：参考核心 SKILL.md 步骤 4
- 代码风格：参考核心 SKILL.md 步骤 3.3
