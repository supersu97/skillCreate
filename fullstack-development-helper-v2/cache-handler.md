# 缓存处理模块

本模块负责项目信息的缓存管理，包括缓存检查、读取和保存。

## 缓存位置

**统一使用默认位置**：`~/.claude/cache/fullstack-projects/[项目路径 hash]/`

**注意**：`~/.claude/` 在 Windows (Git Bash)、Linux 和 Mac 上均可用，会自动指向用户主目录。

## 缓存目录获取流程

**获取默认缓存目录**
```javascript
调用 Bash 工具获取用户主目录：
echo $HOME

// 构建默认缓存基础路径
const cacheBasePath = $HOME/.claude/cache/fullstack-projects/[项目路径 hash]
```

## 缓存检查流程

1. **生成项目标识**
   ```javascript
   // 前端项目 hash（如果需要修改前端）
   const frontendHash = hashPath([前端项目路径]);
   // 后端项目 hash（如果需要修改后端）
   const backendHash = hashPath([后端项目路径]);
   ```

2. **检查缓存是否存在且有效**
   ```javascript
   // 1. 获取用户主目录
   调用 Bash 工具：
   echo $HOME

   // 2. 构建缓存路径
   const cacheBasePath = $HOME + '/.claude/cache/fullstack-projects/[hash]'

   // 3. 检查缓存是否存在
   调用 Read 工具：
   - file_path: [cacheBasePath]/project-cache.json

   // 4. 检查缓存有效性
   if (缓存存在 && 创建时间 < 7 天) {
     // 使用缓存数据，跳过项目分析步骤
     跳转到询问用户是否使用缓存
   } else {
     // 缓存不存在或已过期，继续执行项目分析
   }
   ```

3. **读取缓存数据并询问用户**
   ```javascript
   // 读取缓存的项目信息和代码风格
   调用 Read 工具：
   - file_path: [cacheBasePath]/project-cache.json
   ```

   然后询问用户是否使用缓存：
   ```javascript
   {
     "questions": [{
       "question": "检测到该项目的缓存信息（创建于 [时间]），是否使用缓存？",
       "header": "使用缓存",
       "multiSelect": false,
       "options": [
         {
           "label": "使用缓存",
           "description": "使用缓存的项目信息和代码风格，节省时间"
         },
         {
           "label": "重新分析",
           "description": "重新分析项目结构和代码风格（项目有重大变更时选择）"
         }
       ]
     }]
   }
   ```

## 缓存数据结构

**project-cache.json** (合并了项目信息、代码风格和元数据):
```json
{
  "metadata": {
    "createdAt": "2026-02-27T08:00:00.000Z",
    "expiresAt": "2026-03-06T08:00:00.000Z",
    "version": "1.0",
    "skillVersion": "1.0"
  },
  "projectInfo": {
    "projectPath": "D:\\HcWebProject\\mix\\fe-his-mixapp-dev\\project\\p279",
    "projectType": "frontend",
    "framework": "Taro 3.0.17",
    "language": "TypeScript",
    "uiLibrary": "taro-ui 3.0.0-alpha.9",
    "stateManagement": "dva-core + Redux",
    "cssFramework": "SCSS",
    "packageManager": "npm",
    "entryFile": "package.json",
    "srcDirectory": "src",
    "commonPaths": {
      "components": "src/components",
      "pages": "src/pages",
      "models": "src/models",
      "utils": "src/utils"
    }
  },
  "codeStyle": {
    "componentStyle": "函数组件 + Hooks",
    "namingConvention": "PascalCase for components, camelCase for files",
    "stateManagement": "dva-core (namespace/reducers/effects)",
    "apiCalling": "request.post('/api/xxx', params, options)",
    "styling": "SCSS Modules (import styles from './index.scss')",
    "pathAlias": "@utils, @com, @config"
  },
  "sampleFiles": [
    "src/pages/extra/home/index.tsx",
    "src/models/global.ts"
  ]
}
```

## 缓存保存流程

**在完成项目分析和代码风格分析后，保存缓存以供下次使用**

1. **获取用户主目录并生成项目 hash**
   ```javascript
   调用 Bash 工具：
   echo $HOME

   // 使用项目路径生成唯一标识
   // 简单实现：将路径中的特殊字符替换为下划线
   const projectHash = projectPath.replace(/[:\\\\/]/g, '_');
   ```

2. **确定缓存目录**
   ```javascript
   // 使用默认缓存目录
   const cacheBasePath = $HOME + '/.claude/cache/fullstack-projects/' + projectHash;
   ```

3. **创建缓存目录**
   ```javascript
   调用 Bash 工具：
   mkdir -p "$HOME/.claude/cache/fullstack-projects/[projectHash]"
   ```

   **注意**：如果缓存目录创建失败，不影响后续流程，仅记录日志。

4. **保存合并的缓存文件**（包含项目信息、代码风格和元数据）
   ```javascript
   调用 Write 工具：
   - file_path: [cacheBasePath]/project-cache.json
   - content: {
       "metadata": {
         "createdAt": "[当前时间 ISO 格式]",
         "expiresAt": "[当前时间 +7 天 ISO 格式]",
         "version": "1.0",
         "skillVersion": "1.0"
       },
       "projectInfo": {
         "projectPath": "[项目路径]",
         "projectType": "frontend" 或 "backend",
         "framework": "[框架名称和版本]",
         "language": "[编程语言]",
         "uiLibrary": "[UI 组件库]",  // 仅前端
         "stateManagement": "[状态管理方案]",  // 仅前端
         "cssFramework": "[CSS 方案]",  // 仅前端
         "orm": "[ORM 框架]",  // 仅后端
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
       },
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

5. **告知用户缓存已保存**
   ```
   项目信息已缓存至：[cacheBasePath]
   下次使用该项目时将自动加载缓存，节省分析时间。
   缓存有效期：7 天
   ```

## 注意事项

- 如果缓存目录创建失败，不影响后续流程，只是下次无法使用缓存
- 缓存保存失败时，记录日志但不中断流程
- 前端和后端项目分别保存各自的缓存
- 缓存有效期为 7 天
- 路径使用 `~/.claude/` 表示，会根据操作系统自动解析为用户主目录
