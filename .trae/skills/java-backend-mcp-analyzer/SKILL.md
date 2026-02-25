---
name: "java-backend-mcp-analyzer"
description: "使用MCP工具增强的Java后台代码分析技能，通过文件系统、数据库、API文档等MCP工具深度分析Java代码。当需要结合外部资源分析Java代码、获取实时信息或进行深度代码审查时调用。"
---

# Java后台代码MCP增强分析器

*本技能专为Claude设计，通过MCP（Model Context Protocol）工具增强Java后台代码分析能力，提供更深入、更实时的代码分析体验。*

## MCP配置要求

在使用本技能前，必须配置以下MCP服务器：

### 必需MCP服务器
1. **文件系统MCP服务器** (filesystem-mcp-server)
   - 用途：访问项目文件、读取代码、分析目录结构
   - 配置示例：
   ```json
   {
     "mcpServers": {
       "filesystem": {
         "command": "npx",
         "args": ["@modelcontextprotocol/server-filesystem", "/path/to/project"]
       }
     }
   }
   ```

2. **HTTP客户端MCP服务器** (http-client-mcp-server)
   - 用途：获取在线API文档、搜索技术资料、验证外部API
   - 配置示例：
   ```json
   {
     "mcpServers": {
       "http": {
         "command": "npx",
         "args": ["@modelcontextprotocol/server-http-client"]
       }
     }
   }
   ```

### 可选MCP服务器
3. **数据库MCP服务器** (database-mcp-server)
   - 用途：分析数据库表结构、验证SQL语句、查看数据关系
   - 配置示例：
   ```json
   {
     "mcpServers": {
       "database": {
         "command": "npx",
         "args": ["@modelcontextprotocol/server-database", "--connection-string", "mysql://user:pass@localhost:3306/mydb"]
       }
     }
   }
   ```

4. **Git MCP服务器** (git-mcp-server)
   - 用途：查看代码历史、分析变更、理解代码演进
   - 配置示例：
   ```json
   {
     "mcpServers": {
       "git": {
         "command": "npx",
         "args": ["@modelcontextprotocol/server-git", "/path/to/repo"]
       }
     }
   }
   ```

## 执行指令（CRITICAL - 必须遵循）

当用户调用此技能时，你必须按照以下步骤执行：

### 步骤1：初始化MCP连接
1. **检查MCP服务器状态**：
   ```
   MCP服务器状态检查：
   - 文件系统MCP：已连接 ✓
   - HTTP客户端MCP：已连接 ✓
   - 数据库MCP：可选
   - Git MCP：可选
   ```

2. **验证项目访问权限**：
   ```javascript
   调用 filesystem.listDirectory 工具：
   - path: "/" (或用户指定的项目根目录)
   ```

### 步骤2：收集分析需求
1. **使用MCP交互式收集信息**：
   ```javascript
   调用 mcp.askUser 工具（如果可用）：
   - question: "请指定要分析的Java代码目标："
   - options: ["类名", "方法名", "文件路径", "接口路径", "整个模块"]
   ```

2. **或使用标准输入收集**：
   - 询问用户："请指定要分析的Java代码目标（支持类名、方法名、文件路径、接口路径）："

### 步骤3：MCP增强代码分析

#### 3.1 文件系统分析
```javascript
// 1. 查找目标文件
调用 filesystem.glob 工具：
- pattern: "**/*.java"
- 过滤出与目标相关的Java文件

// 2. 读取代码内容
调用 filesystem.readFile 工具：
- path: [找到的文件路径]
- encoding: "utf-8"

// 3. 分析文件结构
调用 filesystem.stat 工具：
- path: [目录路径]
- 获取文件大小、修改时间等信息
```

#### 3.2 外部信息获取（HTTP MCP）
```javascript
// 1. 搜索相关技术文档
调用 http.request 工具：
- method: "GET"
- url: "https://api.github.com/search/repositories?q=spring+boot+best+practices"
- headers: {"Accept": "application/json"}

// 2. 获取在线API文档
调用 http.request 工具：
- method: "GET"
- url: [用户提供的API文档URL]
- 解析OpenAPI/Swagger文档

// 3. 验证第三方API
调用 http.request 工具：
- method: "GET" 或 "POST"
- url: [代码中调用的第三方API]
- 验证API可用性和响应格式
```

#### 3.3 数据库分析（如果配置了数据库MCP）
```javascript
// 1. 分析数据库表结构
调用 database.query 工具：
- sql: "DESCRIBE users;"  # MySQL
  或 "SELECT column_name, data_type FROM information_schema.columns WHERE table_name = 'users';"  # PostgreSQL

// 2. 验证SQL语句
调用 database.explain 工具：
- sql: [从MyBatis映射文件中提取的SQL语句]
- 分析查询执行计划

// 3. 查看数据关系
调用 database.query 工具：
- sql: "SELECT CONSTRAINT_NAME, TABLE_NAME, COLUMN_NAME, REFERENCED_TABLE_NAME, REFERENCED_COLUMN_NAME FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE WHERE TABLE_SCHEMA = DATABASE();"
```

#### 3.4 代码历史分析（如果配置了Git MCP）
```javascript
// 1. 查看文件历史
调用 git.log 工具：
- path: [文件路径]
- limit: 10

// 2. 分析代码变更
调用 git.diff 工具：
- commit: [提交哈希]
- path: [文件路径]

// 3. 查看代码作者
调用 git.blame 工具：
- path: [文件路径]
```

### 步骤4：综合分析并生成报告

#### 4.1 代码结构分析
```javascript
// 使用MCP工具分析依赖关系
调用 filesystem.readFile 工具读取pom.xml或build.gradle
调用 http.request 工具搜索Maven中央仓库的依赖信息
```

#### 4.2 安全问题检测
```javascript
// 搜索已知安全漏洞
调用 http.request 工具：
- method: "GET"
- url: "https://ossindex.sonatype.org/api/v3/component-report/maven:org.springframework:spring-core"
- 检查依赖的安全漏洞

// 验证输入验证
调用 http.request 工具搜索相关CVE信息
```

#### 4.3 性能分析
```javascript
// 分析SQL性能
调用 database.explain 工具分析所有SQL语句

// 检查外部API性能
调用 http.request 工具测试API响应时间
```

### 步骤5：生成交互式报告

#### 5.1 使用MCP工具提供交互选择
```javascript
调用 mcp.askUser 工具（如果可用）：
{
  "question": "请选择要查看的分析内容：",
  "options": [
    {"label": "代码结构分析", "description": "类图、依赖关系、包结构"},
    {"label": "安全问题报告", "description": "安全漏洞、输入验证、权限问题"},
    {"label": "性能分析结果", "description": "SQL性能、API性能、内存使用"},
    {"label": "数据库分析", "description": "表结构、索引、数据关系"},
    {"label": "外部依赖分析", "description": "第三方库、API依赖、版本兼容性"},
    {"label": "代码历史分析", "description": "变更历史、作者统计、演进趋势"},
    {"label": "完整分析报告", "description": "所有分析内容的汇总"}
  ],
  "multiSelect": true
}
```

#### 5.2 生成报告文件
```javascript
// 使用文件系统MCP保存报告
调用 filesystem.writeFile 工具：
- path: "./code-analysis-report.md"
- content: [生成的Markdown报告]
- encoding: "utf-8"
```

### 步骤6：提供修复建议

#### 6.1 代码修复建议
```javascript
// 使用MCP工具验证修复方案
调用 http.request 工具搜索最佳实践
调用 database.query 工具验证SQL优化方案
```

#### 6.2 生成修复代码
```javascript
// 使用文件系统MCP创建修复文件
调用 filesystem.writeFile 工具：
- path: "./fix-suggestions.md"
- content: [具体的修复建议和代码示例]
```

## MCP工具调用示例

### 示例1：分析Spring Controller
```javascript
// 1. 查找Controller文件
const controllerFiles = await mcp.callTool('filesystem.glob', {
  pattern: '**/*Controller.java'
});

// 2. 读取Controller内容
const controllerContent = await mcp.callTool('filesystem.readFile', {
  path: controllerFiles[0]
});

// 3. 分析@RequestMapping注解
const mappings = extractRequestMappingAnnotations(controllerContent);

// 4. 验证每个API端点
for (const mapping of mappings) {
  const response = await mcp.callTool('http.request', {
    method: 'GET',
    url: `http://localhost:8080${mapping.path}`,
    headers: {'Accept': 'application/json'}
  });
  
  // 分析响应
  analyzeApiResponse(response, mapping);
}

// 5. 检查数据库操作
const sqlStatements = extractSqlStatements(controllerContent);
for (const sql of sqlStatements) {
  const explainResult = await mcp.callTool('database.explain', {
    sql: sql
  });
  analyzeQueryPerformance(explainResult);
}
```

### 示例2：分析MyBatis映射文件
```javascript
// 1. 查找Mapper文件
const mapperFiles = await mcp.callTool('filesystem.glob', {
  pattern: '**/*Mapper.xml'
});

// 2. 读取Mapper内容
const mapperContent = await mcp.callTool('filesystem.readFile', {
  path: mapperFiles[0]
});

// 3. 提取SQL语句
const sqlStatements = extractSqlFromMapper(mapperContent);

// 4. 分析每个SQL语句
for (const sql of sqlStatements) {
  // 验证SQL语法
  const validationResult = await mcp.callTool('database.validate', {
    sql: sql
  });
  
  // 分析执行计划
  const explainResult = await mcp.callTool('database.explain', {
    sql: sql
  });
  
  // 搜索优化建议
  const optimizationTips = await mcp.callTool('http.request', {
    method: 'GET',
    url: `https://api.sql-optimizer.com/analyze?sql=${encodeURIComponent(sql)}`
  });
}

// 5. 检查表结构
const tables = extractTableNames(sqlStatements);
for (const table of tables) {
  const tableSchema = await mcp.callTool('database.query', {
    sql: `DESCRIBE ${table};`
  });
  analyzeTableStructure(table, tableSchema);
}
```

### 示例3：安全漏洞分析
```javascript
// 1. 分析pom.xml依赖
const pomContent = await mcp.callTool('filesystem.readFile', {
  path: 'pom.xml'
});
const dependencies = extractDependencies(pomContent);

// 2. 检查每个依赖的安全漏洞
for (const dep of dependencies) {
  const vulnerabilityReport = await mcp.callTool('http.request', {
    method: 'GET',
    url: `https://ossindex.sonatype.org/api/v3/component-report/maven:${dep.groupId}:${dep.artifactId}:${dep.version}`
  });
  
  if (vulnerabilityReport.vulnerabilities.length > 0) {
    // 搜索修复方案
    const fixSuggestions = await mcp.callTool('http.request', {
      method: 'GET',
      url: `https://search.maven.org/solrsearch/select?q=g:"${dep.groupId}"+AND+a:"${dep.artifactId}"&rows=5&wt=json`
    });
  }
}

// 3. 检查代码中的安全问题
const javaFiles = await mcp.callTool('filesystem.glob', {
  pattern: '**/*.java'
});

for (const file of javaFiles.slice(0, 10)) { // 限制检查数量
  const content = await mcp.callTool('filesystem.readFile', {
    path: file
  });
  
  // 检查SQL注入
  if (containsSqlInjectionPatterns(content)) {
    // 搜索SQL注入修复方案
    const sqlInjectionFixes = await mcp.callTool('http.request', {
      method: 'GET',
      url: 'https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html'
    });
  }
  
  // 检查XSS漏洞
  if (containsXssPatterns(content)) {
    // 搜索XSS防护方案
    const xssPrevention = await mcp.callTool('http.request', {
      method: 'GET',
      url: 'https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html'
    });
  }
}
```

## 输出格式

### 标准分析报告结构
```
# Java代码分析报告

## 1. 项目概览
- 项目路径: [使用filesystem.stat获取]
- 代码行数: [使用filesystem.wc工具或手动统计]
- 文件数量: [使用filesystem.glob统计]
- 主要技术栈: [分析pom.xml/build.gradle]

## 2. 代码结构分析
- 包结构: [使用filesystem.tree工具]
- 类关系: [分析import语句]
- 依赖关系: [分析Maven/Gradle配置]

## 3. 安全问题
- 依赖漏洞: [使用HTTP MCP查询漏洞数据库]
- 代码漏洞: [静态分析结果]
- 修复建议: [使用HTTP MCP搜索修复方案]

## 4. 性能分析
- SQL性能: [使用database.explain分析]
- API性能: [使用http.request测试]
- 内存使用: [分析代码模式]

## 5. 数据库分析
- 表结构: [使用database.query获取]
- 索引情况: [使用database.query分析]
- 数据关系: [分析外键约束]

## 6. 建议和优化
- 立即修复: [高优先级问题]
- 建议优化: [中优先级改进]
- 长期规划: [架构改进建议]
```

### 交互式输出选项
当使用MCP的交互功能时，提供以下选项：
1. **查看详细报告**：生成完整的分析报告
2. **导出为文件**：使用filesystem.writeFile保存报告
3. **只查看安全问题**：过滤显示安全问题
4. **只查看性能问题**：过滤显示性能问题
5. **查看数据库分析**：显示数据库相关分析
6. **生成修复代码**：生成具体的修复代码示例

## 注意事项

### MCP服务器要求
1. **权限配置**：确保MCP服务器有适当的文件系统访问权限
2. **网络连接**：HTTP MCP需要网络连接访问外部资源
3. **数据库连接**：数据库MCP需要有效的数据库连接字符串
4. **资源限制**：注意MCP调用的频率限制和资源使用

### 性能考虑
1. **批量操作**：合并相关的MCP调用，减少开销
2. **缓存结果**：对重复的查询结果进行缓存
3. **异步处理**：长时间的操作使用异步模式
4. **进度反馈**：长时间分析时提供进度更新

### 错误处理
1. **MCP连接失败**：提供备选方案或指导用户检查配置
2. **权限错误**：指导用户调整权限设置
3. **网络错误**：提供离线分析选项
4. **数据错误**：验证输入数据的有效性

## 扩展能力

### 自定义MCP工具
你可以扩展本技能支持更多的MCP服务器：

1. **监控MCP**：连接应用性能监控(APM)系统
2. **日志MCP**：分析应用日志文件
3. **测试MCP**：运行和验证测试用例
4. **部署MCP**：检查部署配置和环境

### 集成现有工具
通过MCP集成现有代码分析工具：
- SonarQube MCP：集成代码质量分析
- Checkstyle MCP：集成代码规范检查
- SpotBugs MCP：集成静态代码分析
- JaCoCo MCP：集成代码覆盖率分析

---

*通过MCP增强，本技能能够访问文件系统、数据库、网络资源等，提供比传统静态分析更深入、更实时的Java代码分析能力。特别适合大型企业级Java项目的代码审查和优化工作。*