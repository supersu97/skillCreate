---
name: "fullstack-development-helper"
description: "全栈开发助手，支持前端和后端项目开发。需要输入前端项目位置、后端项目位置，然后收集开发需求进行全栈开发。当需要进行全栈功能开发、前后端联调或同时修改前端后端代码时调用。"
---

# 全栈开发助手

*本技能专为Claude Code设计，帮助开发者同时处理前端和后端项目的开发任务。*

## 模块文件说明

本技能由多个模块文件组成：

| 模块文件 | 说明 |
|---------|------|
| [SKILL.md](SKILL.md) | 核心流程主文件（含 Agent Team 真正团队协作模式） |
| [project-collector.md](project-collector.md) | 项目位置收集模块 |
| [requirement-collector.md](requirement-collector.md) | 需求收集模块 |
| [ui-design-handler.md](ui-design-handler.md) | UI设计图处理模块（前端UI修改时使用） |
| [confirmation.md](confirmation.md) | 确认修改内容模块 |
| [frontend-executor.md](frontend-executor.md) | 前端代码执行模块 |
| [backend-executor.md](backend-executor.md) | 后端代码执行模块 |

## ⚠️ 模块加载规则（CRITICAL - 最高优先级）

本技能的模块文件不会自动加载。当流程指示"读取模块文件"时，你**必须立即使用 Read 工具**读取对应的 .md 文件，获取完整流程后再执行。**绝对禁止跳过读取、自行替代或凭记忆执行模块中的流程。**

模块文件的基础路径为本 SKILL.md 所在目录（通过技能加载时的 "Base directory for this skill" 获取）。

## 功能介绍

这个技能让开发者能够：

1. **前端代码开发**：支持Vue、React、Angular等主流前端框架
2. **后端代码开发**：支持Spring Boot、Django、Express等主流后端框架
3. **前后端联调**：协调前后端接口，确保数据交互正常
4. **需求分析**：分析全栈功能需求，生成开发方案
5. **代码生成**：根据需求同时生成前端和后端代码
6. **项目结构管理**：管理前端脚手架项目和后端项目的关系

## 何时调用

当你遇到以下情况时，应该调用这个技能：

- 需要开发一个涉及前端和后端的完整功能
- 需要同时修改前端和后端代码
- 前后端接口联调出现问题
- 不清楚前端项目或后端项目的文件位置
- 需要新建全栈功能模块
- 需要了解现有项目的技术栈和结构

## 执行指令（CRITICAL - 必须遵循）

### 步骤0：（新流程第一步）选择修改范围

**必须使用交互式选择**，首先让用户选择要修改的范围：

```javascript
{
  "questions": [{
    "question": "请选择要修改的范围：",
    "header": "修改范围",
    "multiSelect": false,
    "options": [
      {
        "label": "只修改前端代码",
        "description": "仅修改前端代码，不涉及后端修改"
      },
      {
        "label": "只修改后端代码",
        "description": "仅修改后端代码，不涉及前端修改"
      },
      {
        "label": "同时修改前后端代码",
        "description": "需要同时修改前端和后端代码"
      }
    ]
  }]
}
```

**记录用户的选择**，根据选择执行不同流程：
- 如果选择"只修改前端代码" → 跳过步骤0.2的后端项目收集，只收集前端项目路径
- 如果选择"只修改后端代码" → 跳过步骤0.2的前端项目收集，只收集后端项目路径
- 如果选择"同时修改前后端代码" → 收集前端和后端项目路径，后续进入协同开发模式选择

### 步骤0.1：收集项目位置信息

**必须先使用 Read 工具读取 `[技能基础路径]/project-collector.md` 文件**，然后严格按照文件中的流程执行，不得跳过或自行替代。

### 步骤0.5：检查项目缓存

**必须先使用 Read 工具读取 `[技能基础路径]/cache-handler.md` 文件中的"步骤0.5"部分**，然后严格按照文件中的流程执行，不得跳过或自行替代。

### 步骤1：分析项目结构

**如果步骤0.5使用了缓存，跳过本步骤**

根据收集到的项目路径，使用 Glob 和 Read 工具分析项目结构：

```javascript
// 分析前端项目结构（如果需要修改前端）
调用 Glob 工具：
- path: [前端项目路径]
- pattern: "src/**/*"

调用 Read 工具：
- file_path: [前端项目路径]/package.json

// 分析后端项目结构（如果需要修改后端）
调用 Glob 工具：
- path: [后端项目路径]
- pattern: "src/**/*"

调用 Read 工具：
- file_path: [后端路径]/pom.xml 或 build.gradle 或 requirements.txt
```

生成项目结构报告：
```
## 项目结构分析

### 前端项目
- 框架：[Vue 3 / React 18 / Angular 16]
- UI组件库：[Element Plus / Ant Design / Material UI]
- 状态管理：[Pinia / Redux / NgRx]
- 路由：[Vue Router / React Router]
- API调用方式：[Axios / Fetch / 自定义]
- 目录结构：
  - src/api：接口定义
  - src/components：组件
  - src/views：页面
  - src/store：状态管理
  - src/router：路由配置

### 后端项目
- 框架：[Spring Boot 3.x / Django 4.x / Express]
- 数据库：[MySQL / PostgreSQL / MongoDB]
- ORM：[JPA / Hibernate / Sequelize / Prisma]
- 认证：[JWT / Session / OAuth]
- 目录结构：
  - src/main/java/...：Java源码
  - src/main/resources：配置文件
  - src/test：测试代码
```

### 步骤1.5：（关键）分析代码风格

**必须分析现有代码风格，生成代码风格指南，后续所有代码生成必须严格遵循**

根据选择的修改范围分析代码风格：

**如果修改前端**：
```javascript
调用 Glob 工具：
- path: [前端项目路径]/src
- pattern: "**/*.vue" 或 "**/*.tsx" 或 "**/*.jsx"

调用 Glob 工具：
- path: [前端项目路径]/src
- pattern: "**/*.css" 或 "**/*.scss" 或 "**/*.less"

调用 Glob 工具：
- path: [前端项目路径]/src
- pattern: "**/api/**/*.js" 或 "**/api/**/*.ts"

调用 Read 工具读取3-5个代表性文件：
- 组件文件（如：src/components 下的 .vue/.tsx 文件）
- API服务文件（如：src/api 下的服务文件）
- 样式文件（如：src/styles 或组件内的样式）

// 额外收集：设计规范和样式变量
调用 Glob 工具查找样式变量文件：
- path: [前端项目路径]/src
- pattern: "**/variables.{scss,less,css}" 或 "**/theme.{scss,less,css}" 或 "**/config.{scss,less,css}"

调用 Read 工具读取样式变量文件（如果存在）：
- 提取颜色变量（如：$primary-color, --main-color）
- 提取尺寸变量（如：$spacing-base, --font-size-base）
- 提取断点变量（如：$breakpoint-md）
- 提取其他设计token（如：圆角、阴影、过渡动画）
```

**如果修改后端**：
```javascript
调用 Glob 工具：
- path: [后端项目路径]/src
- pattern: "**/*.java" 或 "**/*.py" 或 "**/*.js"

调用 Read 工具读取3-5个代表性文件：
- Controller/Handler文件
- Service业务逻辑文件
- Model/Entity实体文件
- 数据库操作文件（如：Mapper/DAO/Repository）
```

生成代码风格指南：
```
## 代码风格指南（必须严格遵循）

### 前端代码风格

#### 1. 组件风格
- 文件命名：[如：UserList.vue、UserDetail.jsx]
- 组件命名：[如：UserList、UserDetailComponent]
- Props定义：[如：defineProps、PropTypes]
- 状态管理：[如：ref/reactive、useState]

#### 2. API调用风格
- 请求方式：[如：axios.get/post、fetch]
- 参数传递：[如：params/、data/、body]
- 错误处理：[如：.catch、try-catch]
- 类型定义：[如：TypeScript接口、Flow类型]

#### 3. 样式风格
- 样式方案：[如：CSS Modules、SCSS、Styled Components]
- 类名命名：[如：BEM命名、语义化命名]
- 响应式：[如：媒体查询、弹性盒、栅格]
- 组件样式：[如： scoped样式、 CSS-in-JS]

#### 4. 设计规范（从样式变量文件中提取）
- 主题色：[如：$primary-color: #1890ff, $success-color: #52c41a]
- 文字颜色：[如：$text-color: #333, $text-secondary: #666]
- 背景色：[如：$bg-color: #fff, $bg-gray: #f5f5f5]
- 字体大小：[如：$font-size-base: 14px, $font-size-lg: 16px]
- 间距规范：[如：$spacing-xs: 8px, $spacing-md: 16px, $spacing-lg: 24px]
- 圆角：[如：$border-radius: 4px]
- 阴影：[如：$box-shadow: 0 2px 8px rgba(0,0,0,0.1)]
- 断点：[如：$breakpoint-md: 768px, $breakpoint-lg: 1024px]

**注意**：如果项目没有样式变量文件，则从现有组件样式中总结常用的颜色、尺寸等规范。
```

### 后端代码风格

#### 1. 命名风格
- 类命名：[如：UserController、UserService]
- 方法命名：[如：getUserById、createUser]
- 变量命名：[如：userId、userList]
- 常量命名：[如：USER_STATUS、MAX_COUNT]

#### 2. 代码结构风格
- 分层结构：[如：Controller → Service → Mapper]
- 参数校验：[如：@Valid、@NotNull]
- 事务管理：[如：@Transactional]
- 异常处理：[如：@ExceptionHandler、try-catch]

#### 3. 数据库操作风格
- 查询方式：[如：JPQL、@Query、XML映射]
- 命名规范：[如：下划线命名、驼峰命名]
- 关联关系：[如：@OneToMany、@ManyToOne]

**重要提示**：后续所有生成的代码必须严格遵循上述代码风格，禁止使用与现有代码风格不一致的实现方式。同时，必须严格遵循用户指定的修改范围，没有要求改动的地方必须保持原样，不得进行额外修改。

### 步骤1.6：保存项目缓存（新增 - 优化性能）

**在完成项目分析和代码风格分析后，保存缓存以供下次使用**

#### 缓存保存流程

1. **获取用户主目录并生成项目hash**
   ```javascript
   调用 Bash 工具：
   # Linux/Mac
   echo $HOME
   # Windows (如果上面失败)
   echo %USERPROFILE%

   // 使用项目路径生成唯一标识
   // 简单实现：将路径中的特殊字符替换为下划线
   const projectHash = projectPath.replace(/[:\\\/]/g, '_');
   ```

2. **创建缓存目录**
   ```javascript
   调用 Bash 工具：
   # Linux/Mac
   mkdir -p "$HOME/.claude/cache/fullstack-projects/[projectHash]"
   # Windows
   mkdir "%USERPROFILE%\.claude\cache\fullstack-projects\[projectHash]"
   ```

3. **保存项目信息**（根据步骤1的分析结果）
   ```javascript
   调用 Write 工具：
   - file_path: [用户主目录]/.claude/cache/fullstack-projects/[projectHash]/project-info.json
   - content: {
       "projectPath": "[项目路径]",
       "projectType": "frontend" 或 "backend",
       "framework": "[框架名称和版本]",
       "language": "[编程语言]",
       "uiLibrary": "[UI组件库]",  // 仅前端
       "stateManagement": "[状态管理方案]",  // 仅前端
       "cssFramework": "[CSS方案]",  // 仅前端
       "orm": "[ORM框架]",  // 仅后端
       "database": "[数据库]",  // 仅后端
       "packageManager": "npm/yarn/maven/gradle",
       "entryFile": "package.json/pom.xml",
       "srcDirectory": "src",
       "commonPaths": {
         "components": "src/components",  // 前端
         "pages": "src/pages",  // 前端
         "controllers": "src/main/java/.../controller",  // 后端
         "services": "src/main/java/.../service"  // 后端
       }
     }
   ```

4. **保存代码风格**（根据步骤1.5的分析结果）
   ```javascript
   调用 Write 工具：
   - file_path: [用户主目录]/.claude/cache/fullstack-projects/[projectHash]/code-style.json
   - content: {
       "projectPath": "[项目路径]",
       "projectType": "frontend" 或 "backend",
       "codeStyle": {
         // 前端代码风格
         "componentStyle": "函数组件 + Hooks / 类组件",
         "namingConvention": "PascalCase / camelCase",
         "stateManagement": "dva-core / Redux / Vuex",
         "apiCalling": "request.post / axios.get",
         "styling": "SCSS Modules / CSS-in-JS",
         "pathAlias": "@utils, @com, @config",

         // 后端代码风格
         "layerStructure": "Controller → Service → Mapper",
         "controllerPattern": "extends BaseController",
         "responsePattern": "sendSuccessData / sendFailureMessage",
         "paramPattern": "FrontUtils.getParams(request)",
         "loggingPattern": "LoggerUtil.getLogger(context, logger)",
         "jsonLibrary": "FastJSON / Jackson"
       },
       "sampleFiles": [
         "src/pages/xxx/index.tsx",
         "src/models/global.ts"
       ]
     }
   ```

5. **保存缓存元数据**
   ```javascript
   调用 Write 工具：
   - file_path: [用户主目录]/.claude/cache/fullstack-projects/[projectHash]/cache-meta.json
   - content: {
       "createdAt": "[当前时间 ISO格式]",
       "expiresAt": "[当前时间+24小时 ISO格式]",
       "version": "1.0",
       "skillVersion": "1.0"
     }
   ```

6. **告知用户缓存已保存**
   ```
   项目信息已缓存至：~/.claude/cache/fullstack-projects/[projectHash]/
   下次使用该项目时将自动加载缓存，节省分析时间。
   ```

#### 注意事项

- 如果缓存目录创建失败，不影响后续流程，只是下次无法使用缓存
- 缓存保存失败时，记录日志但不中断流程
- 前端和后端项目分别保存各自的缓存
- 路径使用 `~/.claude/` 表示，会根据操作系统自动解析为用户主目录

### 步骤2：收集开发需求

**必须先使用 Read 工具读取 `[技能基础路径]/requirement-collector.md` 文件**，然后严格按照文件中的流程执行，不得跳过或自行替代。

### 步骤3：确认修改内容

**必须先使用 Read 工具读取 `[技能基础路径]/confirmation.md` 文件**，然后严格按照文件中的流程执行，不得跳过或自行替代。

### 步骤4：执行开发任务

**根据步骤0的选择执行不同的流程**

#### 4.1 只修改前端 → 直接执行

基于步骤2已收集的修改方式和需求，直接进入"执行前端代码修改"流程（步骤5）

#### 4.2 只修改后端 → 直接执行

基于步骤2已收集的修改方式和需求，直接进入"执行后端代码修改"流程（步骤6）

#### 4.3 同时修改前后端 → 选择协同开发模式

**必须使用交互式选择**，让用户选择协同开发模式：

```javascript
{
  "questions": [{
    "question": "请选择协同开发模式：",
    "header": "协同模式",
    "multiSelect": false,
    "options": [
      {
        "label": "轻量顺序模式（省Token）",
        "description": "顺序执行：后端先改 → 确定接口 → 前端再改。不启动子Agent，Token消耗最少，适合简单的前后端联动修改"
      },
      {
        "label": "Agent Team并行模式",
        "description": "创建Agent团队，前后端可并行修改。适合前后端改动独立、复杂度高的场景"
      },
      {
        "label": "Agent Team顺序模式",
        "description": "创建Agent团队，但通过任务依赖让后端先完成接口设计，前端等待后再修改。适合接口未确定、需要后端先定义的复杂场景"
      }
    ]
  }]
}
```

**根据选择的模式执行相应流程：**

**模式A：轻量顺序模式（省Token）**

**适用场景**：简单的前后端联动修改，不需要并行处理，优先节省Token消耗。
**特点**：不启动子Agent，所有操作在主Agent上下文中顺序执行。

**注意：需求和修改方式已在步骤2中收集完毕，本模式直接使用已收集的信息执行。**

按照以下流程执行：

## 轻量顺序模式流程

### 第一步：检查第三方接口需求

使用 AskUserQuestion 工具询问：
```javascript
{
  "questions": [{
    "question": "后端是否需要对接第三方接口？",
    "header": "第三方接口",
    "multiSelect": false,
    "options": [
      {
        "label": "需要对接第三方接口",
        "description": "需要调用第三方API，请提供接口文档路径或链接"
      },
      {
        "label": "不需要对接第三方接口",
        "description": "仅修改内部逻辑，根据需求自行设计接口"
      }
    ]
  }]
}
```

如果需要对接第三方接口，收集接口文档路径后解析。

### 第二步：执行后端代码修改
基于步骤2收集的后端需求和修改方式，执行"执行后端代码修改"流程（步骤6）。

### 第三步：确定接口规范
后端修改完成后，整理新的接口规范：
- 接口路径和方法
- 请求/响应格式
- 参数和返回值

### 第四步：执行前端代码修改
基于步骤2收集的前端需求和后端确定的接口规范，执行"执行前端代码修改"流程（步骤5）。

### 第五步：完成总结

```
## 轻量顺序模式完成

1. 后端修改：已完成
2. 前端修改：已基于后端接口规范完成
3. 接口一致性：前后端基于同一接口规范实现
```

**模式B：Agent Team并行模式（真正的团队协作）**

**注意：需求、修改方式和详细信息已在步骤2中收集完毕，本模式直接使用已收集的信息。**

**本模式使用 TeamCreate 创建真正的 Agent 团队，团队成员之间可以通过 SendMessage 互相通信、通过 TaskList 共享任务进度，实现实时协作。**

按照以下流程执行：

**第一步：生成修改方案文档并保存到项目**

基于步骤2和步骤3已确认的需求，生成完整的修改方案文档。

**必须将方案文档保存到项目目录**，使用 Write 工具写入文件：
- 保存路径：`[后端项目路径]/docs/dev-plan-[YYYYMMDD-HHmmss].md`（优先后端项目，如果只有前端则保存到前端项目）
- 如果 `docs/` 目录不存在，先创建该目录

方案文档内容模板：

```markdown
# 全栈修改方案文档

> 生成时间：[当前时间]
> 修改范围：前端 + 后端

## 功能概述
[基于步骤2收集的功能描述]

## 接口定义
| 接口名称 | 方法 | 路径 | 请求参数 | 响应格式 | 说明 |
|----------|------|------|----------|----------|------|
| [接口名] | [GET/POST/PUT/DELETE] | [/api/xxx] | [参数说明] | [响应结构] | [备注] |

## 前端修改清单
- 修改方式：[步骤2选择的前端修改方式]

| 序号 | 文件路径 | 修改内容 | 状态 |
|------|----------|----------|------|
| 1 | [文件路径] | [修改内容] | 待修改 |
| 2 | [文件路径] | [修改内容] | 待修改 |

## 后端修改清单
- 修改方式：[步骤2选择的后端修改方式]

| 序号 | 文件路径 | 修改内容 | 状态 |
|------|----------|----------|------|
| 1 | [文件路径] | [修改内容] | 待修改 |
| 2 | [文件路径] | [修改内容] | 待修改 |

## 数据库变更（如需要）
[SQL语句或变更描述]

## 代码风格要求
- 前端：严格遵循步骤1.5中分析的前端代码风格
- 后端：严格遵循步骤1.5中分析的后端代码风格
```

保存后告知用户方案文档路径：
```
修改方案文档已保存至：[文件路径]
```

**第二步：创建 Agent Team**

使用 TeamCreate 工具创建真正的团队：

```javascript
调用 TeamCreate 工具：
{
  "team_name": "fullstack-dev",
  "description": "全栈开发团队 - [功能描述简要]"
}
```

**第三步：创建任务并分配**

使用 TaskCreate 创建前端和后端任务：

```javascript
// 创建后端开发任务
调用 TaskCreate 工具：
{
  "subject": "后端代码修改",
  "description": "基于修改方案文档执行后端代码修改。\n\n方案文档路径：[方案文档路径]\n\n后端项目路径：[后端项目路径]\n\n修改清单：[后端修改清单内容]\n\n代码风格要求：严格遵循现有后端代码风格",
  "activeForm": "执行后端代码修改"
}

// 创建前端开发任务
调用 TaskCreate 工具：
{
  "subject": "前端代码修改",
  "description": "基于修改方案文档执行前端代码修改。\n\n方案文档路径：[方案文档路径]\n\n前端项目路径：[前端项目路径]\n\n修改清单：[前端修改清单内容]\n\n接口定义：[接口定义内容]\n\n代码风格要求：严格遵循现有前端代码风格",
  "activeForm": "执行前端代码修改"
}
```

**第四步：启动 Agent 团队成员并行工作**

使用 Task 工具启动两个真正的团队成员（注意：使用 team_name 参数加入团队）：

```javascript
// 启动后端开发 Agent（并行）
调用 Task 工具：
{
  "name": "backend-dev",
  "subagent_type": "general-purpose",
  "team_name": "fullstack-dev",
  "prompt": "你是后端开发Agent，属于 fullstack-dev 团队。\n\n请执行以下工作：\n1. 先用 TaskList 查看你的任务\n2. 用 TaskUpdate 认领'后端代码修改'任务（设置 owner 为 backend-dev，status 为 in_progress）\n3. 读取方案文档：[方案文档路径]\n4. 按照方案文档中的后端修改清单，逐一执行代码修改\n5. 严格遵循现有后端代码风格\n6. 修改完成后，用 SendMessage 通知 frontend-dev 你完成的接口信息，格式：接口路径、方法、请求参数、响应格式\n7. 用 TaskUpdate 将任务标记为 completed\n8. 用 SendMessage 通知 team-lead（主Agent）后端修改已完成\n\n后端项目路径：[后端项目路径]\n方案文档路径：[方案文档路径]",
  "description": "后端开发Agent"
}

// 启动前端开发 Agent（并行）
调用 Task 工具：
{
  "name": "frontend-dev",
  "subagent_type": "general-purpose",
  "team_name": "fullstack-dev",
  "prompt": "你是前端开发Agent，属于 fullstack-dev 团队。\n\n请执行以下工作：\n1. 先用 TaskList 查看你的任务\n2. 用 TaskUpdate 认领'前端代码修改'任务（设置 owner 为 frontend-dev，status 为 in_progress）\n3. 读取方案文档：[方案文档路径]\n4. 按照方案文档中的前端修改清单，逐一执行代码修改\n5. 严格遵循现有前端代码风格\n6. 如果收到 backend-dev 的消息（包含接口信息），根据实际接口信息校验和调整前端代码\n7. 用 TaskUpdate 将任务标记为 completed\n8. 用 SendMessage 通知 team-lead（主Agent）前端修改已完成\n\n前端项目路径：[前端项目路径]\n方案文档路径：[方案文档路径]",
  "description": "前端开发Agent"
}
```

**关键特性**：
- 后端 Agent 完成接口后，会通过 SendMessage 主动通知前端 Agent 实际的接口信息
- 前端 Agent 收到后端消息后，可以校验和调整自己的代码，确保接口对接正确
- 两个 Agent 共享同一个 TaskList，可以看到彼此的任务进度
- 主 Agent 作为 team-lead 接收两个 Agent 的完成通知

**第五步：监控团队进度**

主 Agent 等待两个团队成员的完成消息：
- 收到 backend-dev 的完成消息 → 记录后端完成
- 收到 frontend-dev 的完成消息 → 记录前端完成
- 两个都完成后 → 进入第六步

如果某个 Agent 遇到问题发来消息，主 Agent 应协助解决或协调。

**第六步：关闭团队并总结**

1. 使用 SendMessage 向两个 Agent 发送 shutdown_request
2. 等待 Agent 确认关闭
3. 使用 TeamDelete 清理团队资源
4. 使用 TaskList 确认所有任务已完成

```
## Agent Team协作完成

1. 后端开发：已完成 ✓
2. 前端开发：已完成 ✓
3. 接口一致性：后端Agent已将接口信息同步给前端Agent，前端已校验
4. 方案文档：已保存至 [方案文档路径]
5. 团队资源：已清理
```

**模式C：Agent Team顺序模式（任务依赖协作）**

**适用场景**：接口未确定，需要后端先完成接口设计和实现，前端再基于实际接口修改。复杂度较高，需要Agent独立工作。
**特点**：创建Agent团队，但通过 TaskUpdate 的 blockedBy 机制让前端任务等待后端任务完成后再开始。后端Agent完成后通过 SendMessage 将接口信息同步给前端Agent。

**注意：需求、修改方式和详细信息已在步骤2中收集完毕，本模式直接使用已收集的信息。**

按照以下流程执行：

**第一步：生成修改方案文档并保存到项目**

与模式B的第一步完全相同，生成修改方案文档并保存到项目目录。

**第二步：创建 Agent Team**

```javascript
调用 TeamCreate 工具：
{
  "team_name": "fullstack-dev",
  "description": "全栈开发团队（顺序模式） - [功能描述简要]"
}
```

**第三步：创建任务并设置依赖关系**

```javascript
// 创建后端开发任务
调用 TaskCreate 工具：
{
  "subject": "后端代码修改",
  "description": "基于修改方案文档执行后端代码修改。\n\n方案文档路径：[方案文档路径]\n\n后端项目路径：[后端项目路径]\n\n修改清单：[后端修改清单内容]\n\n代码风格要求：严格遵循现有后端代码风格",
  "activeForm": "执行后端代码修改"
}

// 创建前端开发任务（设置 blockedBy 依赖后端任务）
调用 TaskCreate 工具：
{
  "subject": "前端代码修改",
  "description": "基于修改方案文档和后端实际接口信息执行前端代码修改。\n\n方案文档路径：[方案文档路径]\n\n前端项目路径：[前端项目路径]\n\n修改清单：[前端修改清单内容]\n\n代码风格要求：严格遵循现有前端代码风格\n\n注意：需等待后端任务完成并获取实际接口信息后再开始",
  "activeForm": "等待后端完成后执行前端代码修改"
}

// 设置前端任务依赖后端任务
调用 TaskUpdate 工具：
{
  "taskId": "[前端任务ID]",
  "addBlockedBy": ["[后端任务ID]"]
}
```

**第四步：启动后端 Agent 先行工作**

先只启动后端Agent：

```javascript
// 启动后端开发 Agent
调用 Task 工具：
{
  "name": "backend-dev",
  "subagent_type": "general-purpose",
  "team_name": "fullstack-dev",
  "prompt": "你是后端开发Agent，属于 fullstack-dev 团队。\n\n请执行以下工作：\n1. 先用 TaskList 查看你的任务\n2. 用 TaskUpdate 认领'后端代码修改'任务（设置 owner 为 backend-dev，status 为 in_progress）\n3. 读取方案文档：[方案文档路径]\n4. 按照方案文档中的后端修改清单，逐一执行代码修改\n5. 严格遵循现有后端代码风格\n6. 修改完成后，用 TaskUpdate 将任务标记为 completed\n7. 用 SendMessage 通知 team-lead（主Agent）后端修改已完成，并附上实际的接口信息（接口路径、方法、请求参数、响应格式）\n\n后端项目路径：[后端项目路径]\n方案文档路径：[方案文档路径]",
  "description": "后端开发Agent"
}
```

**第五步：等待后端完成，启动前端 Agent**

主 Agent 等待后端 Agent 的完成消息。收到后端完成消息（含接口信息）后：

```javascript
// 启动前端开发 Agent
调用 Task 工具：
{
  "name": "frontend-dev",
  "subagent_type": "general-purpose",
  "team_name": "fullstack-dev",
  "prompt": "你是前端开发Agent，属于 fullstack-dev 团队。\n\n后端已完成修改，以下是实际的接口信息：\n[从后端Agent消息中提取的接口信息]\n\n请执行以下工作：\n1. 先用 TaskList 查看你的任务\n2. 用 TaskUpdate 认领'前端代码修改'任务（设置 owner 为 frontend-dev，status 为 in_progress）\n3. 读取方案文档：[方案文档路径]\n4. 基于后端实际接口信息和方案文档中的前端修改清单，逐一执行代码修改\n5. 严格遵循现有前端代码风格\n6. 用 TaskUpdate 将任务标记为 completed\n7. 用 SendMessage 通知 team-lead（主Agent）前端修改已完成\n\n前端项目路径：[前端项目路径]\n方案文档路径：[方案文档路径]",
  "description": "前端开发Agent"
}
```

**第六步：等待前端完成，关闭团队并总结**

1. 等待前端 Agent 的完成消息
2. 使用 SendMessage 向两个 Agent 发送 shutdown_request
3. 等待 Agent 确认关闭
4. 使用 TeamDelete 清理团队资源
5. 使用 TaskList 确认所有任务已完成

```
## Agent Team顺序模式完成

1. 后端开发：已完成（先行）
2. 前端开发：已完成（基于后端实际接口）
3. 接口一致性：前端基于后端实际完成的接口信息开发，确保一致
4. 方案文档：已保存至 [方案文档路径]
5. 团队资源：已清理
```

### 步骤5：执行前端代码修改

**必须先使用 Read 工具读取 `[技能基础路径]/frontend-executor.md` 文件**，然后严格按照文件中的流程执行，不得跳过或自行替代。

### 步骤6：执行后端代码修改

**必须先使用 Read 工具读取 `[技能基础路径]/backend-executor.md` 文件**，然后严格按照文件中的流程执行，不得跳过或自行替代。

### 接口文档处理流程（如有需要）

如果用户提供了接口文档，按照以下流程处理：

**情况A：本地文件**
```
1. 读取本地接口文档文件
   调用 Read 工具：
   - file_path: [用户提供的本地文件路径]
   
2. 解析文档格式
   - YAML文件：使用YAML解析器
   - JSON文件（OpenAPI/Swagger）：解析JSON结构
   - Markdown文件：提取接口定义
   - Postman集合：导入Collection格式
   
3. 提取接口信息
   - 接口路径和方法
   - 请求参数和类型
   - 响应结构和状态码
   - 认证要求
```

**情况B：在线文档链接**
```
1. 获取在线接口文档
   - 如果是公开URL：直接获取
   - 如果需要认证：提示用户提供认证信息
   
2. 解析文档格式
   - Swagger/OpenAPI 2.0/3.0：解析规范
   - Markdown文档：提取接口说明
   - HTML页面：解析接口表格

3. 验证接口可用性
   - 测试接口连通性
   - 检查接口响应格式
```

**情况C：暂无接口文档**
```
1. 根据需求设计接口
   - 分析前端需要的数据
   - 设计RESTful API结构
   - 确定请求/响应格式
   
2. 生成接口设计文档
   - 接口列表
   - 参数说明
   - 响应示例
   - 错误码定义
```
