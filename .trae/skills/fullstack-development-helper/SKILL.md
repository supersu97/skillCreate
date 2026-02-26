---
name: "fullstack-development-helper"
description: "全栈开发助手，支持前端和后端项目开发。需要输入前端项目位置、后端项目位置，然后收集开发需求进行全栈开发。当需要进行全栈功能开发、前后端联调或同时修改前端后端代码时调用。"
---

# 全栈开发助手

*本技能专为Claude Code设计，帮助开发者同时处理前端和后端项目的开发任务。*

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

### 步骤0：收集项目位置信息

**必须使用交互式选择**，执行以下流程：

#### 0.1 选择项目位置输入方式

使用 AskUserQuestion 工具提供选择菜单：
```javascript
{
  "questions": [{
    "question": "请选择项目位置输入方式：",
    "header": "输入方式",
    "multiSelect": false,
    "options": [
      {
        "label": "手动输入路径",
        "description": "直接输入前端和后端项目的文件夹路径"
      },
      {
        "label": "自动检测",
        "description": "在当前目录或指定目录下自动查找前端和后端项目"
      },
      {
        "label": "使用脚手架目录",
        "description": "前端在脚手架文件夹内，后端在外部或其他位置"
      }
    ]
  }]
}
```

#### 0.2 根据选择收集项目位置

**情况A：手动输入路径**

1. 询问前端项目位置：
   ```
   请输入前端项目文件夹路径（例如：./my-vue-project 或 D:/projects/frontend）：
   ```

2. 询问后端项目位置：
   ```
   请输入后端项目文件夹路径（例如：./my-spring-boot 或 D:/projects/backend）：
   ```

**情况B：自动检测**

1. 询问要在哪个目录检测：
   ```
   请输入要检测的根目录（直接回车表示当前目录）：
   ```

2. 使用 Glob 工具自动查找项目：
   ```javascript
   // 查找前端项目特征文件
   调用 Glob 工具：
   - path: [用户指定的目录]
   - pattern: "**/package.json"
   
   调用 Glob 工具：
   - path: [用户指定的目录]
   - pattern: "**/vue.config.js"
   
   调用 Glob 工具：
   - path: [用户指定的目录]
   - pattern: "**/react.config.js"
   
   // 查找后端项目特征文件
   调用 Glob 工具：
   - path: [用户指定的目录]
   - pattern: "**/pom.xml"
   
   调用 Glob 工具：
   - path: [用户指定的目录]
   - pattern: "**/build.gradle"
   
   调用 Glob 工具：
   - path: [用户指定的目录]
   - pattern: "**/requirements.txt"
   ```

3. 分析检测结果，确定项目类型和位置

**情况C：使用脚手架目录**

1. 询问脚手架根目录：
   ```
   请输入脚手架项目的根目录（例如：./scaffold 或 D:/projects/scaffold）：
   ```

2. 询问前端项目在脚手架中的位置：
   ```
   请输入前端项目在脚手架中的相对路径（例如：client、frontend、app）：
   ```

3. 询问后端项目位置：
   ```
   请输入后端项目位置：
   1. 在脚手架内的相对路径（例如：server、backend）
   2. 在脚手架外部的绝对路径或相对路径
   ```

4. 使用 Glob/Read 工具验证项目结构：
   ```javascript
   // 验证前端项目
   调用 Glob 工具：
   - path: [脚手架目录]/[前端相对路径]
   - pattern: "package.json"
   
   // 验证后端项目
   调用 Glob 工具：
   - path: [后端目录]
   - pattern: "pom.xml" 或 "build.gradle" 或 "requirements.txt"
   ```

#### 0.3 确认项目位置

总结检测到的项目位置并确认：
```
检测到以下项目：
- 前端项目：[前端路径]
  技术栈：[Vue/React/Angular + 具体框架版本]
  入口文件：[package.json路径]
  
- 后端项目：[后端路径]
  技术栈：[Spring Boot/Django/Express + 具体框架版本]
  入口文件：[pom.xml/build.gradle路径]

请确认项目位置是否正确？
```

### 步骤1：分析项目结构

使用 Glob 和 Read 工具分析前后端项目结构：

```javascript
// 分析前端项目结构
调用 Glob 工具：
- path: [前端项目路径]
- pattern: "src/**/*"

调用 Read 工具：
- file_path: [前端项目路径]/package.json

// 分析后端项目结构
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

使用 Glob 和 Read 工具分析前后端代码风格：

```javascript
// 分析前端代码风格
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
- 状态管理文件（如：src/store 下的文件）

// 分析后端代码风格
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

#### 4. 目录结构风格
```
src/
├── components/    # 组件目录结构
├── views/         # 页面目录结构
├── api/           # 接口目录结构
├── store/         # 状态管理目录结构
└── utils/         # 工具函数目录结构
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

#### 4. 注释风格
- 类注释：[如：/** */ JavaDoc格式]
- 方法注释：[如：/** */ 功能说明]
- 行注释：[如：// 单行注释]

**重要提示**：后续所有生成的代码必须严格遵循上述代码风格，禁止使用与现有代码风格不一致的实现方式。

### 步骤2：收集开发需求

使用 AskUserQuestion 工具选择需求类型和接口文档输入：
```javascript
{
  "questions": [
    {
      "question": "请选择开发需求类型：",
      "header": "需求类型",
      "multiSelect": false,
      "options": [
        {
          "label": "新增功能",
          "description": "开发一个全新的前后端功能"
        },
        {
          "label": "修改功能",
          "description": "修改现有的前后端功能"
        },
        {
          "label": "接口联调",
          "description": "解决前后端接口对接问题"
        },
        {
          "label": "性能优化",
          "description": "优化前后端性能"
        },
        {
          "label": "Bug修复",
          "description": "修复前后端的Bug"
        },
        {
          "label": "代码审查",
          "description": "审查前后端代码质量"
        }
      ]
    },
    {
      "question": "请选择接口文档输入方式（可选）：",
      "header": "接口文档",
      "multiSelect": false,
      "options": [
        {
          "label": "本地文件",
          "description": "提供本地接口文档文件路径（支持YAML/JSON/Markdown/Postman）"
        },
        {
          "label": "在线链接",
          "description": "提供在线接口文档URL（支持Swagger/OpenAPI/Markdown）"
        },
        {
          "label": "暂无接口文档",
          "description": "暂无接口文档，需要根据需求自行设计接口"
        }
      ]
    }
  ]
}
```

**根据用户选择的接口文档方式，收集相应信息：**

**如果选择"本地文件"**：
```
请提供本地接口文档的文件路径：
- 支持格式：YAML (.yaml/.yml)、JSON (.json)、Markdown (.md)、Postman Collection (.json)
- 示例路径：
  • ./docs/api.yaml
  • D:/projects/api/openapi.json
  • ./api-docs/user-api.md
```

**如果选择"在线链接"**：
```
请提供在线接口文档的URL链接：
- 支持格式：
  • Swagger UI: https://api.example.com/swagger-ui.html
  • OpenAPI JSON: https://api.example.com/v3/api-docs
  • ReDoc: https://api.example.com/redoc
  • 普通Markdown文档URL
- 如需认证，请说明：
```

**如果选择"暂无接口文档"**：
```
好的，将根据您描述的需求自行设计接口。
请在需求描述中详细说明：
- 前端需要展示哪些数据
- 前端需要提交哪些数据
- 数据的格式要求
```

根据选择收集详细需求：

**新增功能 / 修改功能**：
```
请详细描述要开发的新功能或修改内容：
1. 功能名称：
2. 功能描述：
3. 前端需求：
   - 页面/组件：
   - 用户交互：
   - 数据展示：
4. 后端需求：
   - API接口：
   - 业务逻辑：
   - 数据存储：
5. 接口文档（可选）：
   - 如有接口文档，请选择输入方式：
     ① 本地文件路径（例如：./docs/api.yaml 或 D:/api-docs/openapi.json）
     ② 在线文档链接（例如：https://api.example.com/docs 或 Swagger URL）
     ③ 暂无接口文档，需要根据需求自行设计
6. 接口设计（如无接口文档）：
   - 请求格式：
   - 响应格式：
```

**接口文档处理流程**：
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

**接口联调**：
```
请描述联调问题：
1. 问题描述：
2. 前端调用方式：
3. 期望的后端响应：
4. 实际的后端响应：
5. 错误信息（如有）：
```

### 步骤3：生成开发方案

根据收集的需求，生成全栈开发方案：

**重要约束**：所有生成的代码必须严格遵循步骤1.5中分析的代码风格指南

#### 3.1 分析前后端修改范围

**如果需要同时修改前后端，按照以下流程处理：**

**第一步：后端提供接口信息**
```
请提供需要修改的后端接口信息：
- 接口路径（如：/api/users、/api/orders/{id}）
- 接口方法（如：GET、POST、PUT、DELETE）
- 或后端方法名（如：UserController.getUserById）
```

**第二步：前端自动查找对应接口调用**
```
后端接口信息已收到，正在查找前端对应代码...
```

使用 Grep 工具在前端项目中查找接口调用：
```javascript
// 1. 根据接口路径查找
调用 Grep 工具：
- path: [前端项目路径]
- pattern: "/api/users"
- output_mode: "content"

调用 Grep 工具：
- path: [前端项目路径]
- pattern: "axios\\.get|axios\\.post|fetch\\("
- output_mode: "content"

// 2. 查找API服务定义文件
调用 Glob 工具：
- path: [前端项目路径]
- pattern: "**/api/**/*.js"
- pattern: "**/api/**/*.ts"
- pattern: "**/services/**/*.js"
- pattern: "**/services/**/*.ts"
```

**第三步：分析前后端对应关系**
```
## 前后端接口对应关系

### 后端接口
- 接口：[后端提供的接口路径]
- 方法：[HTTP方法]
- 文件位置：[后端Controller文件路径]
- 代码位置：[具体方法行号]

### 前端对应调用
- API服务文件：[找到的API服务文件路径]
- 调用位置：[具体调用行号]
- 调用方式：[axios/fetch的具体调用代码]
- 请求参数：[前端传递的参数]
- 响应处理：[前端如何处理响应]
```

#### 3.2 生成详细开发方案

```markdown
## 全栈开发方案

### 功能：[功能名称]

**代码风格约束（必须遵循）**：
- 前端代码风格：严格遵循步骤1.5中分析的前端代码风格
- 后端代码风格：严格遵循步骤1.5中分析的后端代码风格

#### 前端开发
- 涉及文件：
  - [文件1路径]
  - [文件2路径]
- 开发内容：
  - [具体前端开发任务1]
  - [具体前端开发任务2]
- 代码风格要求：[对应步骤1.5中的前端代码风格]

#### 后端开发
- 涉及文件：
  - [文件1路径]
  - [文件2路径]
- 开发内容：
  - [具体后端开发任务1]
  - [具体后端开发任务2]
- 代码风格要求：[对应步骤1.5中的后端代码风格]

#### 接口定义
| 接口 | 方法 | 路径 | 请求参数 | 响应格式 |
|------|------|------|----------|----------|
| xxx | GET | /api/xxx | {id: number} | {data: {...}} |

#### 前后端联调
- 联调要点：[具体的联调注意事项]
- 测试方法：[测试步骤]
```

#### 3.3 前后端联动修改流程（当同时修改前后端时）

如果用户需要同时修改前后端的同一个功能接口：

```
## 前后端联动修改流程

### 第一阶段：后端修改
1. 后端先进行修改
2. 确定新的接口规范（请求/响应格式）
3. 更新接口定义

### 第二阶段：前端同步
根据后端新的接口规范：
1. 修改API服务调用代码
2. 更新请求参数处理
3. 更新响应数据处理
4. 更新相关页面组件

### 前后端对应修改示例

**后端修改**：
- 文件：src/main/java/com/example/controller/UserController.java
- 修改：getUserById 方法返回值增加 age 字段

**前端同步修改**：
- 文件：src/api/user.js
- 修改：updateUserType 定义，增加 age 字段
- 文件：src/views/user/UserDetail.vue
- 修改：展示新增的 age 字段
```

### 步骤4：执行开发任务

#### 4.1 使用 AskUserQuestion 工具选择执行方式
```javascript
{
  "questions": [{
    "question": "请选择执行操作（可多选）：",
    "header": "执行操作",
    "multiSelect": true,
    "options": [
      {
        "label": "查看完整开发方案",
        "description": "查看前后端详细的开发计划和代码示例"
      },
      {
        "label": "只生成前端代码",
        "description": "只生成前端相关代码"
      },
      {
        "label": "只生成后端代码",
        "description": "只生成后端相关代码"
      },
      {
        "label": "执行前端代码修改",
        "description": "直接修改前端代码文件"
      },
      {
        "label": "执行后端代码修改",
        "description": "直接修改后端代码文件"
      },
      {
        "label": "生成接口文档",
        "description": "生成或更新API接口文档"
      },
      {
        "label": "生成测试代码",
        "description": "生成前后端测试代码"
      }
    ]
  }]
}
```

#### 4.2 根据选择执行相应操作

**生成前端代码**：
- 使用 Write 工具创建或修改前端文件
- 使用 Read 工具参考现有代码风格

**生成后端代码**：
- 使用 Write 工具创建或修改后端文件
- 使用 Read 工具参考现有代码风格

**生成接口文档**：
```
## API接口文档

### 接口列表
1. [接口名称]
   - 请求方式：GET/POST/PUT/DELETE
   - 请求路径：/api/xxx
   - 请求头：[需要认证等信息]
   - 请求参数：[参数说明]
   - 响应示例：[JSON示例]
   - 错误码：[错误码说明]
```

### 步骤4.5：生成修改记录文档

**重要**：每次代码修改完成后，必须生成或更新修改记录文档

#### 4.5.1 检查是否存在现有修改记录文档

```javascript
// 在前端项目中查找修改记录文档
调用 Glob 工具：
- path: [前端项目路径]
- pattern: "**/CHANGELOG.md"
- pattern: "**/HISTORY.md"
- pattern: "**/CHANGES.md"
- pattern: "**/docs/changelog*.md"
- pattern: "**/docs/history*.md"

调用 Glob 工具：
- path: [前端项目路径]
- pattern: "**/MODIFY_RECORD.md"
- pattern: "**/修改记录.md"

// 在后端项目中查找修改记录文档
调用 Glob 工具：
- path: [后端项目路径]
- pattern: "**/CHANGELOG.md"
- pattern: "**/CHANGES.md"
- pattern: "**/docs/changelog*.md"

调用 Glob 工具：
- path: [后端项目路径]
- pattern: "**/MODIFY_RECORD.md"
- pattern: "**/修改记录.md"
```

#### 4.5.2 根据情况生成或更新修改记录

**情况A：存在现有修改记录文档**
```
1. 读取现有修改记录文档
   调用 Read 工具：
   - file_path: [找到的修改记录文件路径]

2. 在文档末尾添加新的修改记录
   格式：
   ```
   ## [日期 时间]
   
   ### [修改类型：新增功能/修改功能/Bug修复/性能优化]
   
   **功能名称**：[功能名称]
   
   #### 前端修改
   - 修改文件：[前端文件路径]
   - 修改内容：[具体修改内容]
   
   #### 后端修改
   - 修改文件：[后端文件路径]
   - 修改内容：[具体修改内容]
   
   #### 接口变更（如有）
   - 新增接口：[接口路径]
   - 修改接口：[接口路径]
   - 删除接口：[接口路径]
   
   ---
   ```

3. 使用 Write 工具更新文档
   ```javascript
   调用 Write 工具：
   - file_path: [修改记录文件路径]
   - content: [原文档内容 + 新增修改记录]
   ```

**情况B：不存在修改记录文档，需要新建**

**前端修改记录**：
```
调用 Write 工具：
- file_path: [前端项目路径]/docs/MODIFY_RECORD.md 或 [前端项目路径]/CHANGELOG.md
- content: 
# 修改记录

本文档记录前端项目的所有代码修改历史。

---

## [当前日期 时间]

### [修改类型：新增功能/修改功能/Bug修复/性能优化]

**功能名称**：[功能名称]

#### 修改内容
- 修改文件：[文件路径]
- 修改内容：[具体修改内容]
- 代码风格：严格遵循项目现有代码风格（见步骤1.5）

#### 关联后端（如有）
- 对应后端修改：[后端文件路径]
- 接口变更：[接口变化说明]

---

*文档创建时间：[当前日期 时间]*
```

**后端修改记录**：
```
调用 Write 工具：
- file_path: [后端项目路径]/docs/MODIFY_RECORD.md 或 [后端项目路径]/CHANGELOG.md
- content:
# 修改记录

本文档记录后端项目的所有代码修改历史。

---

## [当前日期 时间]

### [修改类型：新增功能/修改功能/Bug修复/性能优化]

**功能名称**：[功能名称]

#### 修改内容
- 修改文件：[文件路径]
- 修改内容：[具体修改内容]
- 代码风格：严格遵循项目现有代码风格（见步骤1.5）

#### 关联前端（如有）
- 对应前端修改：[前端文件路径]

#### 接口变更（如有）
- 新增接口：[接口路径] - [接口说明]
- 修改接口：[接口路径] - [修改说明]
- 删除接口：[接口路径] - [删除原因]

---

*文档创建时间：[当前日期 时间]*
```

#### 4.5.3 修改记录模板汇总

```markdown
## [日期 时间]

### [修改类型]

**功能名称**：[功能名称]

#### 前端修改
| 文件路径 | 修改内容 | 修改类型 |
|----------|----------|----------|
| src/xxx/xxx.vue | 具体修改内容 | 新增/修改/删除 |

#### 后端修改
| 文件路径 | 修改内容 | 修改类型 |
|----------|----------|----------|
| src/main/java/.../XXX.java | 具体修改内容 | 新增/修改/删除 |

#### 接口变更
| 接口路径 | 方法 | 变更类型 | 说明 |
|----------|------|----------|------|
| /api/xxx | GET | 新增/修改/删除 | 接口说明 |

#### 代码风格确认
- 前端代码：严格遵循步骤1.5中的前端代码风格
- 后端代码：严格遵循步骤1.5中的后端代码风格

---
```

### 步骤5：验证和测试

修改完成后，询问用户：
```
开发任务已完成。请选择后续操作：
1. 查看生成的前端代码
2. 查看生成的后端代码
3. 运行前端项目验证
4. 运行后端项目验证
5. 继续开发其他功能
6. 调整开发方案
```

## 常用场景示例

### 场景1：新增用户管理功能

**输入**：
- 前端项目位置：`./scaffold/client`
- 后端项目位置：`./scaffold/server`
- 需求：新增用户管理功能，包括用户列表、用户详情、用户编辑

**输出**：
```markdown
## 用户管理功能开发方案

### 前端开发
1. 用户列表页面
   - 文件：src/views/user/UserList.vue
   - 功能：展示用户列表、分页、搜索
   
2. 用户详情页面
   - 文件：src/views/user/UserDetail.vue
   - 功能：查看用户详细信息
   
3. 用户编辑对话框
   - 文件：src/components/user/UserForm.vue
   - 功能：新增/编辑用户表单

### 后端开发
1. 用户Controller
   - 文件：src/main/java/com/example/controller/UserController.java
   - 接口：GET/POST/PUT/DELETE /api/users
   
2. 用户Service
   - 文件：src/main/java/com/example/service/UserService.java
   - 业务逻辑：用户 CRUD 操作
   
3. 用户Mapper
   - 文件：src/main/resources/mapper/UserMapper.xml
   - 数据库操作：用户表增删改查

### 接口定义
| 接口 | 方法 | 路径 | 说明 |
|------|------|------|------|
| 获取用户列表 | GET | /api/users | 支持分页、搜索 |
| 获取用户详情 | GET | /api/users/{id} | 获取单个用户 |
| 新增用户 | POST | /api/users | 创建用户 |
| 更新用户 | PUT | /api/users/{id} | 更新用户 |
| 删除用户 | DELETE | /api/users/{id} | 删除用户 |
```

### 场景2：前后端联调问题

**输入**：
- 前端项目位置：`./frontend`
- 后端项目位置：`./backend`
- 问题：用户登录接口联调失败

**分析流程**：
1. 读取前端登录代码，找到API调用
2. 读取后端登录接口代码
3. 对比请求/响应格式
4. 找出差异并提供解决方案

**输出**：
```markdown
## 联调问题分析

### 问题描述
前端调用登录接口返回500错误

### 前端调用
```javascript
axios.post('/api/login', {
  username: 'admin',
  password: '123456'
})
```

### 后端接口
```java
@PostMapping("/login")
public ResponseEntity<?> login(@RequestParam String username, @RequestParam String password) {
  // 使用 @RequestParam 接收参数
}
```

### 问题原因
- 前端发送JSON body
- 后端使用 @RequestParam 接收表单参数

### 解决方案
1. 修改前端，使用表单提交：
```javascript
axios.post('/api/login', 
  new URLSearchParams({
    username: 'admin',
    password: '123456'
  })
)
```
或
2. 修改后端，使用 @RequestBody：
```java
@PostMapping("/login")
public ResponseEntity<?> login(@RequestBody LoginRequest request) {
  // 使用 @RequestBody 接收JSON
}
```

### 建议
推荐修改后端使用 @RequestBody，前端发送JSON更规范。
```

## 项目位置识别特征

### 前端项目特征文件

| 框架 | 特征文件 | 入口配置 |
|------|----------|----------|
| Vue 2 | vue.config.js | package.json |
| Vue 3 | vite.config.ts | package.json |
| React | react.config.js / tsconfig.json | package.json |
| Angular | angular.json | package.json |

### 后端项目特征文件

| 框架 | 特征文件 | 入口配置 |
|------|----------|----------|
| Spring Boot | pom.xml / build.gradle | Application.java |
| Django | manage.py / settings.py | requirements.txt |
| Express | package.json | app.js / index.js |
| Flask | app.py / requirements.txt | requirements.txt |
| NestJS | nest-cli.json | package.json |

## 注意事项

1. **项目路径**：确保前端和后端项目路径正确，避免文件操作错误
2. **代码风格**：生成代码时参考项目现有代码风格
3. **版本兼容**：注意前后端框架版本的兼容性
4. **接口约定**：前后端接口保持一致的数据格式
5. **错误处理**：生成代码时包含完善的错误处理
6. **安全性**：生成的代码包含基本的安全考虑
7. **测试**：建议生成相应的测试代码

---

*使用本技能可以高效地进行全栈开发，同时处理前端和后端项目，确保前后端代码的协调一致。*