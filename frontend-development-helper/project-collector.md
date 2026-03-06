# 前端项目位置收集器

本文件是 frontend-development-helper 的项目位置收集模块。

## 步骤 2：收集前端项目位置

### 2.1 检测当前目录是否是前端项目

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
    - "taro" / "@tarojs/taro"
    - "wepy" / "wepy-cli"
    - "dva" / "umi"
    - "@ant-design/pro"
    → 前端项目

  // 3. 检查是否包含前端构建工具
  if (devDependencies 包含以下任一):
    - "webpack" / "vite" / "rollup" / "parcel"
    - "babel-loader" / "@babel/core"
    - "sass-loader" / "less-loader" / "stylus-loader"
    - "roadhog"
    → 前端项目

  // 4. 检查是否包含后端框架（排除前端项目）
  if (dependencies 包含以下任一):
    - "express" / "koa" / "fastify"
    - "nest" / "@nestjs/core"
    - "egg" / "midway"
    → 后端项目（Node.js），当前技能不适用

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

### 2.2 如果当前目录不是前端项目，使用交互式选择

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

### 2.3 确认项目位置

```
检测到以下前端项目：
- 前端项目：[前端路径]
  技术栈：[Vue/React/Angular + 具体框架版本]
  入口文件：[package.json路径]

请确认项目位置是否正确？
```
