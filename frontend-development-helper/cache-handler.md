# 缓存处理模块（前端）

本模块负责前端项目信息的缓存管理，包括缓存检查、读取和保存。

## 缓存位置

**统一使用默认位置**：`~/.claude/cache/frontend-projects/[项目路径 hash]/`

**注意**：`~/.claude/` 在 Windows (Git Bash)、Linux 和 Mac 上均可用，会自动指向用户主目录。

## 缓存目录获取流程

```javascript
调用 Bash 工具获取用户主目录：
echo $HOME

// 构建默认缓存基础路径
const cacheBasePath = $HOME/.claude/cache/frontend-projects/[项目路径 hash]
```

## 缓存检查流程

1. **生成项目标识**
   ```javascript
   const projectHash = projectPath.replace(/[:\\\\/]/g, '_');
   ```

2. **检查缓存是否存在且有效**
   ```javascript
   // 1. 获取用户主目录
   调用 Bash 工具：
   echo $HOME

   // 2. 构建缓存路径
   const cacheBasePath = $HOME + '/.claude/cache/frontend-projects/' + projectHash

   // 3. 检查缓存是否存在
   调用 Read 工具：
   - file_path: [cacheBasePath]/project-cache.json

   // 4. 检查缓存有效性
   if (缓存存在 && 创建时间 < 7 天) {
     跳转到询问用户是否使用缓存
   } else {
     // 缓存不存在或已过期，继续执行项目分析
   }
   ```

3. **读取缓存数据并询问用户**
   ```javascript
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

**project-cache.json**:
```json
{
  "metadata": {
    "createdAt": "2026-02-27T08:00:00.000Z",
    "expiresAt": "2026-03-06T08:00:00.000Z",
    "version": "1.0",
    "skillVersion": "1.0"
  },
  "projectInfo": {
    "projectPath": "[项目路径]",
    "projectType": "frontend",
    "framework": "[框架名称和版本]",
    "language": "TypeScript/JavaScript",
    "uiLibrary": "[UI 组件库]",
    "stateManagement": "[状态管理方案]",
    "cssFramework": "[CSS 方案]",
    "packageManager": "npm/yarn/pnpm",
    "entryFile": "package.json",
    "srcDirectory": "src",
    "commonPaths": {
      "components": "src/components",
      "pages": "src/pages",
      "utils": "src/utils",
      "api": "src/api"
    }
  },
  "codeStyle": {
    "componentStyle": "函数组件 + Hooks / 类组件",
    "namingConvention": "PascalCase for components, camelCase for files",
    "stateManagement": "Pinia / Redux / Vuex / dva-core",
    "apiCalling": "axios.get / request.post",
    "styling": "SCSS Modules / CSS-in-JS / scoped",
    "pathAlias": "@utils, @components, @api"
  },
  "designSpec": {
    "primaryColor": "[主色]",
    "fontSizeBase": "[基础字体大小]",
    "spacingBase": "[基础间距]",
    "borderRadius": "[圆角]"
  },
  "sampleFiles": [
    "src/components/xxx/index.tsx",
    "src/pages/xxx/index.vue"
  ]
}
```

## 缓存保存流程

1. **获取用户主目录并生成项目 hash**
   ```javascript
   调用 Bash 工具：
   echo $HOME

   const projectHash = projectPath.replace(/[:\\\\/]/g, '_');
   ```

2. **确定缓存目录**
   ```javascript
   const cacheBasePath = $HOME + '/.claude/cache/frontend-projects/' + projectHash;
   ```

3. **创建缓存目录**
   ```javascript
   调用 Bash 工具：
   mkdir -p "$HOME/.claude/cache/frontend-projects/[projectHash]"
   ```

4. **保存缓存文件**
   ```javascript
   调用 Write 工具：
   - file_path: [cacheBasePath]/project-cache.json
   - content: [按上述数据结构填充]
   ```

5. **告知用户缓存已保存**
   ```
   项目信息已缓存至：[cacheBasePath]
   下次使用该项目时将自动加载缓存，节省分析时间。
   缓存有效期：7 天
   ```

## 注意事项

- 如果缓存目录创建失败，不影响后续流程
- 缓存有效期为 7 天
- 路径使用 `~/.claude/` 表示，会根据操作系统自动解析为用户主目录
