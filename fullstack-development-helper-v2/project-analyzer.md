# 项目分析模块

本模块负责分析项目结构和代码风格，为后续开发提供技术约束和规范。

## 项目结构分析

**如果使用了缓存，跳过本步骤**

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

### 后端项目
- 框架：[Spring Boot 3.x / Django 4.x / Express]
- 数据库：[MySQL / PostgreSQL / MongoDB]
- ORM：[JPA / Hibernate / Sequelize / Prisma]
- 认证：[JWT / Session / OAuth]
- 目录结构：
  - src/main/java/...：Java 源码
  - src/main/resources：配置文件
  - src/test：测试代码
```

## 代码风格分析

**必须分析现有代码风格，生成代码风格指南，后续所有代码生成必须严格遵循**

根据选择的修改范围分析代码风格：

### 前端代码风格分析

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

### 后端代码风格分析

```javascript
调用 Glob 工具：
- path: [后端项目路径]/src
- pattern: "**/*.java" 或 "**/*.py" 或 "**/*.js"

调用 Read 工具读取 3-5 个代表性文件：
- Controller/Handler 文件
- Service 业务逻辑文件
- Model/Entity 实体文件
- 数据库操作文件（如：Mapper/DAO/Repository）
```

## 代码风格指南生成

生成代码风格指南：
```
## 代码风格指南（必须严格遵循）

### 前端代码风格

#### 1. 组件风格
- 文件命名：[如：UserList.vue、UserDetail.jsx]
- 组件命名：[如：UserList、UserDetailComponent]
- Props 定义：[如：defineProps、PropTypes]
- 状态管理：[如：ref/reactive、useState]

#### 2. API 调用风格
- 请求方式：[如：axios.get/post、fetch]
- 参数传递：[如：params/、data/、body]
- 错误处理：[如：.catch、try-catch]
- 类型定义：[如：TypeScript 接口、Flow 类型]

#### 3. 样式风格
- 样式方案：[如：CSS Modules、SCSS、Styled Components]
- 类名命名：[如：BEM 命名、语义化命名]
- 响应式：[如：媒体查询、弹性盒、栅格]
- 组件样式：[如：scoped 样式、CSS-in-JS]

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
- 查询方式：[如：JPQL、@Query、XML 映射]
- 命名规范：[如：下划线命名、驼峰命名]
- 关联关系：[如：@OneToMany、@ManyToOne]

**重要提示**：后续所有生成的代码必须严格遵循上述代码风格，禁止使用与现有代码风格不一致的实现方式。同时，必须严格遵循用户指定的修改范围，没有要求改动的地方必须保持原样，不得进行额外修改。
