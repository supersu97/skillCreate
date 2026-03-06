# 前端项目分析模块

本模块负责分析前端项目结构和代码风格。

## 前端项目结构分析

**如果使用了缓存，跳过本步骤**

根据收集到的项目路径，使用 Glob 和 Read 工具分析项目结构：

```javascript
// 分析前端项目结构
调用 Glob 工具：
- path: [前端项目路径]
- pattern: "src/**/*"

调用 Read 工具：
- file_path: [前端项目路径]/package.json
```

生成项目结构报告：
```
## 前端项目结构分析

- 框架：[Vue 3 / React 18 / Angular 16]
- UI 组件库：[Element Plus / Ant Design / Material UI]
- 状态管理：[Pinia / Redux / NgRx]
- 路由：[Vue Router / React Router]
- API 调用方式：[Axios / Fetch / 自定义]
- 目录结构：
  - src/api：接口定义
  - src/components：组件
  - src/views：页面
  - src/store：状态管理
  - src/router：路由配置
```

## 前端代码风格分析

**必须分析现有代码风格，生成代码风格指南，后续所有代码生成必须严格遵循**

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

调用 Read 工具读取 3-5 个代表性文件：
- 组件文件（如：src/components 下的.vue/.tsx 文件）
- API 服务文件（如：src/api 下的服务文件）
- 样式文件（如：src/styles 或组件内的样式）

// 额外收集：设计规范和样式变量
调用 Glob 工具查找样式变量文件：
- path: [前端项目路径]/src
- pattern: "**/variables.{scss,less,css}" 或 "**/theme.{scss,less,css}" 或 "**/config.{scss,less,css}"

调用 Read 工具读取样式变量文件（如果存在）：
- 提取颜色变量（如：$primary-color, --main-color）
- 提取尺寸变量（如：$spacing-base, --font-size-base）
- 提取断点变量（如：$breakpoint-md）
- 提取其他设计 token（如：圆角、阴影、过渡动画）
```

## 代码风格指南生成

生成代码风格指南：
```
## 前端代码风格指南（必须严格遵循）

### 1. 组件风格
- 文件命名：[如：UserList.vue、UserDetail.jsx]
- 组件命名：[如：UserList、UserDetailComponent]
- Props 定义：[如：defineProps、PropTypes]
- 状态管理：[如：ref/reactive、useState]

### 2. API 调用风格
- 请求方式：[如：axios.get/post、fetch]
- 参数传递：[如：params、data、body]
- 错误处理：[如：.catch、try-catch]
- 类型定义：[如：TypeScript 接口、Flow 类型]

### 3. 样式风格
- 样式方案：[如：CSS Modules、SCSS、Styled Components]
- 类名命名：[如：BEM 命名、语义化命名]
- 响应式：[如：媒体查询、弹性盒、栅格]
- 组件样式：[如：scoped 样式、CSS-in-JS]

### 4. 设计规范（从样式变量文件中提取）
- 主题色：[如：$primary-color: #1890ff, $success-color: #52c41a]
- 文字颜色：[如：$text-color: #333, $text-secondary: #666]
- 背景色：[如：$bg-color: #fff, $bg-gray: #f5f5f5]
- 字体大小：[如：$font-size-base: 14px, $font-size-lg: 16px]
- 间距规范：[如：$spacing-xs: 8px, $spacing-md: 16px, $spacing-lg: 24px]
- 圆角：[如：$border-radius: 4px]
- 阴影：[如：$box-shadow: 0 2px 8px rgba(0,0,0,0.1)]
- 断点：[如：$breakpoint-md: 768px, $breakpoint-lg: 1024px]

**注意**：如果项目没有样式变量文件，则从现有组件样式中总结常用的颜色、尺寸等规范。

**重要提示**：后续所有生成的代码必须严格遵循上述代码风格，禁止使用与现有代码风格不一致的实现方式。
```
