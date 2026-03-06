# 项目位置收集器（优化版）

本文件是 fullstack-development-helper-v2 的项目位置收集模块，包含如何收集前端和后端项目位置的详细流程。

## v2 优化说明

相比 v1 版本：
- ✅ 优先使用当前工作目录，减少询问
- ✅ **智能检测相邻目录**：自动查找 ../backend、../frontend 等常见路径
- ✅ **提供快速路径选项**：根据当前目录智能推荐可能的路径
- ✅ 移除"脚手架目录"选项（过于复杂）
- ✅ 简化自动检测逻辑

## 步骤 2：收集项目位置信息

**根据步骤 1 的推断结果，只收集需要的项目位置**

### 2.1 如果只修改前端（只需收集前端项目路径）

**优先检查当前目录**：

```javascript
// 第一步：检查是否存在 package.json
调用 Glob 工具：
- path: .
- pattern: "package.json"

// 第二步：如果找到 package.json，判断是否是前端项目
如果找到 package.json：
  调用 Read 工具：
  - file_path: ./package.json

  // 判断规则（按优先级）：

  // 1. 检查是否是微信小程序项目
  if (存在 project.config.json 或 project.private.config.json):
    → 前端项目（微信小程序）

  // 2. 检查是否包含前端框架依赖
  if (dependencies 或 devDependencies 包含以下任一):
    - "vue" / "vue3" / "@vue/cli"
    - "react" / "react-dom"
    - "angular" / "@angular/core"
    - "next" / "nuxt" / "vite"
    - "taro" / "@tarojs/taro"  // Taro 多端框架
    - "wepy" / "wepy-cli"      // WePY 微信小程序框架
    - "dva" / "umi"            // Dva/Umi 框架
    - "@ant-design/pro"        // Ant Design Pro
    → 前端项目

  // 3. 检查是否包含前端构建工具
  if (devDependencies 包含以下任一):
    - "webpack" / "vite" / "rollup" / "parcel"
    - "babel-loader" / "@babel/core"
    - "sass-loader" / "less-loader" / "stylus-loader"
    - "roadhog"  // Ant Design 构建工具
    → 前端项目

  // 4. 检查是否包含后端框架（排除前端项目）
  if (dependencies 包含以下任一):
    - "express" / "koa" / "fastify"
    - "nest" / "@nestjs/core"
    - "egg" / "midway"
    → 后端项目（Node.js）

  // 5. 如果无法判断，检查目录结构
  调用 Glob 工具：
  - path: .
  - pattern: "src/app.{tsx,ts,jsx,js,wpy,vue}"

  if (找到 src/app.* 文件):
    → 前端项目

  // 6. 检查是否有 components 或 pages 目录
  调用 Glob 工具：
  - path: .
  - pattern: "{src/,}components/**"

  调用 Glob 工具：
  - path: .
  - pattern: "{src/,}pages/**"

  if (找到 components 或 pages 目录):
    → 前端项目

  如果判断为前端项目：
    询问用户："检测到当前目录是前端项目，是否使用？"
    如果确认，使用当前目录，跳过后续步骤
```

**如果当前目录不是前端项目**，使用交互式选择：

```javascript
{
  "questions": [{
    "question": "请选择前端项目位置输入方式：",
    "header": "输入方式",
    "multiSelect": false,
    "options": [
      {
        "label": "手动输入路径",
        "description": "直接输入前端项目的文件夹路径"
      },
      {
        "label": "自动检测",
        "description": "在指定目录下自动查找前端项目"
      }
    ]
  }]
}
```

**情况A：手动输入路径**

```
请输入前端项目文件夹路径（例如：./my-vue-project 或 D:/projects/frontend）：
```

**情况B：自动检测**

```
请输入要检测的根目录（直接回车表示当前目录的父目录）：
```

然后使用 Glob 工具自动查找项目：

```javascript
调用 Glob 工具：
- path: [用户指定的目录]
- pattern: "**/package.json"

// 过滤出前端项目（排除 node_modules）
for each package.json:
  if (路径不包含 "node_modules"):
    读取 package.json
    检查是否包含前端框架依赖
    如果是前端项目，添加到候选列表
```

### 2.2 如果只修改后端（只需收集后端项目路径）

**优先检查当前目录**：

```javascript
// 第一步：检查是否存在后端项目特征文件
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

// 第二步：如果找到任一文件，进一步确认
如果找到 pom.xml：
  调用 Read 工具：
  - file_path: ./pom.xml

  // 检查是否是 Java 后端项目
  if (包含 <packaging>war</packaging> 或 <packaging>jar</packaging>):
    → 后端项目（Java Maven）

  if (包含 Spring 相关依赖):
    - spring-boot-starter
    - spring-webmvc
    - spring-context
    → 后端项目（Spring）

  if (包含 MyBatis 相关依赖):
    - mybatis
    - mybatis-spring
    → 后端项目（MyBatis）

如果找到 build.gradle：
  → 后端项目（Java Gradle）

如果找到 requirements.txt：
  → 后端项目（Python）

如果找到 go.mod：
  → 后端项目（Go）

如果判断为后端项目：
  询问用户："检测到当前目录是后端项目，是否使用？"
  如果确认，使用当前目录，跳过后续步骤
```

**如果当前目录不是后端项目**，使用交互式选择：

```javascript
{
  "questions": [{
    "question": "请选择后端项目位置输入方式：",
    "header": "输入方式",
    "multiSelect": false,
    "options": [
      {
        "label": "手动输入路径",
        "description": "直接输入后端项目的文件夹路径"
      },
      {
        "label": "自动检测",
        "description": "在指定目录下自动查找后端项目"
      }
    ]
  }]
}
```

**情况A：手动输入路径**

```
请输入后端项目文件夹路径（例如：./my-spring-boot 或 D:/projects/backend）：
```

**情况B：自动检测**

```
请输入要检测的根目录（直接回车表示当前目录的父目录）：
```

然后使用 Glob 工具自动查找项目：

```javascript
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

### 2.3 如果同时修改前后端（需要收集两个项目路径）

**智能检测策略：当前目录 + 相邻目录**

#### 第一步：检测当前目录

```javascript
// 检查当前目录是前端还是后端项目
调用 Glob 工具：
- path: .
- pattern: "package.json"

调用 Glob 工具：
- path: .
- pattern: "pom.xml"

调用 Glob 工具：
- path: .
- pattern: "build.gradle"

// 判断当前目录类型
if (找到 package.json 且包含前端框架依赖):
  currentType = "frontend"
  currentPath = "."
else if (找到 pom.xml 或 build.gradle):
  currentType = "backend"
  currentPath = "."
else:
  currentType = "unknown"
```

#### 第二步：智能检测另一个项目

**情况A：当前目录是前端项目**

```javascript
// 1. 检查常见的相对路径
const commonBackendPaths = [
  "../backend",
  "../server",
  "../api",
  "./backend",
  "./server",
  "../" + path.basename(currentPath).replace("frontend", "backend")
]

for each path in commonBackendPaths:
  检查该路径是否存在后端项目特征文件
  if (找到):
    candidateBackendPath = path
    break

// 2. 检查父目录的所有子目录
调用 Glob 工具：
- path: ..
- pattern: "*/pom.xml"

调用 Glob 工具：
- path: ..
- pattern: "*/build.gradle"

// 3. 显示检测结果
if (找到候选后端项目):
  询问用户：
  ```
  检测到以下项目：
  - 前端项目：[当前目录]
  - 后端项目：[候选路径]（自动检测）

  是否使用这些项目？
  ```

  如果确认，跳过后续步骤
  如果不确认，继续询问后端项目路径
else:
  直接询问后端项目路径（提供快速选项）
```

**情况B：当前目录是后端项目**

```javascript
// 1. 检查常见的相对路径
const commonFrontendPaths = [
  "../frontend",
  "../client",
  "../web",
  "./frontend",
  "./client",
  "../" + path.basename(currentPath).replace("backend", "frontend")
]

for each path in commonFrontendPaths:
  检查该路径是否存在前端项目特征文件
  if (找到):
    candidateFrontendPath = path
    break

// 2. 检查父目录的所有子目录
调用 Glob 工具：
- path: ..
- pattern: "*/package.json"

// 过滤出前端项目
for each package.json:
  if (路径不包含 "node_modules"):
    读取 package.json
    检查是否包含前端框架依赖
    if (是前端项目):
      candidateFrontendPath = 该路径
      break

// 3. 显示检测结果
if (找到候选前端项目):
  询问用户：
  ```
  检测到以下项目：
  - 前端项目：[候选路径]（自动检测）
  - 后端项目：[当前目录]

  是否使用这些项目？
  ```

  如果确认，跳过后续步骤
  如果不确认，继续询问前端项目路径
else:
  直接询问前端项目路径（提供快速选项）
```

**情况C：当前目录不是项目**

```javascript
// 检查父目录和子目录
调用 Glob 工具：
- path: ..
- pattern: "*/package.json"

调用 Glob 工具：
- path: ..
- pattern: "*/pom.xml"

// 如果在父目录找到前端和后端项目
if (找到前端项目 && 找到后端项目):
  显示检测结果：
  ```
  检测到以下项目：
  - 前端项目：[路径]
  - 后端项目：[路径]

  是否使用这些项目？
  ```

  如果确认，跳过后续步骤
```

#### 第三步：如果自动检测失败，提供快速输入选项

**如果自动检测失败，提供快速输入选项**

根据当前目录类型，提供智能的快速选项：

**当前目录是前端项目时**：

使用 AskUserQuestion 工具：

```javascript
{
  "questions": [{
    "question": "未能自动检测到后端项目，请选择后端项目位置：",
    "header": "后端项目",
    "multiSelect": false,
    "options": [
      {
        "label": "../backend",
        "description": "后端项目在父目录的 backend 文件夹"
      },
      {
        "label": "../server",
        "description": "后端项目在父目录的 server 文件夹"
      },
      {
        "label": "./backend",
        "description": "后端项目在当前目录的 backend 子文件夹"
      },
      {
        "label": "手动输入路径",
        "description": "输入自定义路径"
      },
      {
        "label": "自动检测",
        "description": "在指定目录下自动查找后端项目"
      }
    ]
  }]
}
```

如果用户选择"手动输入路径"：
```
请输入后端项目文件夹路径（例如：../my-spring-boot 或 D:/projects/backend）：
```

如果用户选择"自动检测"：
```
请输入要检测的根目录（直接回车表示父目录）：
```

**当前目录是后端项目时**：

使用 AskUserQuestion 工具：

```javascript
{
  "questions": [{
    "question": "未能自动检测到前端项目，请选择前端项目位置：",
    "header": "前端项目",
    "multiSelect": false,
    "options": [
      {
        "label": "../frontend",
        "description": "前端项目在父目录的 frontend 文件夹"
      },
      {
        "label": "../client",
        "description": "前端项目在父目录的 client 文件夹"
      },
      {
        "label": "../web",
        "description": "前端项目在父目录的 web 文件夹"
      },
      {
        "label": "手动输入路径",
        "description": "输入自定义路径"
      },
      {
        "label": "自动检测",
        "description": "在指定目录下自动查找前端项目"
      }
    ]
  }]
}
```

如果用户选择"手动输入路径"：
```
请输入前端项目文件夹路径（例如：../my-vue-project 或 D:/projects/frontend）：
```

如果用户选择"自动检测"：
```
请输入要检测的根目录（直接回车表示父目录）：
```

**当前目录不是项目时**：

使用 AskUserQuestion 工具：

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
        "description": "在指定目录下自动查找前端和后端项目"
      }
    ]
  }]
}
```

如果选择"手动输入路径"：
1. 询问前端项目位置
2. 询问后端项目位置

如果选择"自动检测"：
1. 询问要检测的根目录
2. 使用 Glob 工具自动查找项目

### 2.4 确认项目位置

根据推断的修改范围显示检测到的项目：

**只修改前端时**：

```
检测到以下前端项目：
- 前端项目：[前端路径]
  技术栈：[Vue/React/Angular + 具体框架版本]
  入口文件：[package.json路径]

请确认项目位置是否正确？
```

**只修改后端时**：

```
检测到以下后端项目：
- 后端项目：[后端路径]
  技术栈：[Spring Boot/Django/Express + 具体框架版本]
  入口文件：[pom.xml/build.gradle路径]

请确认项目位置是否正确？
```

**同时修改前后端时**：

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

---

**相关引用**

- 修改范围推断：参考核心 SKILL.md 步骤 1
- 项目结构分析：参考核心 SKILL.md 步骤 3.2
