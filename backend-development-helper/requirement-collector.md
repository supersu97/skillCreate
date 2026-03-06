# 后端需求收集器

本文件是 backend-development-helper 的需求收集模块。

## 步骤 4：收集后端开发需求

**本步骤基于步骤 1 的推断结果，只收集必要的补充信息**

### 4.1 根据推断的修改方式收集详细需求

**修改方式为"直接描述修改"时**：

```
请详细描述要修改的内容：
- 修改目标：
- 修改要求：
- 预期效果：
```

**修改方式为"根据接口信息修改"时**：

```
请提供接口信息（二选一）：
1. 接口路径和请求方法（如：POST /api/users）
2. 接口文档位置（本地文件路径或在线链接）
```

**修改方式为"指定文件修改"时**：

```
请提供要修改的文件信息：
- 文件路径或类名（如：UserController 或 src/main/java/.../UserService.java）
- 修改内容描述：
```

**修改方式为"新增API/功能"时**：

```
请提供新增内容信息：
- API 名称：
- 接口路径：
- 功能描述：
- 请求参数和响应格式：
```

### 4.2 询问是否需要数据库变更

```javascript
{
  "questions": [{
    "question": "是否需要数据库变更？",
    "header": "数据库",
    "multiSelect": false,
    "options": [
      {
        "label": "需要",
        "description": "需要修改数据库表结构，将生成相应SQL语句"
      },
      {
        "label": "不需要",
        "description": "不涉及数据库变更"
      }
    ]
  }]
}
```

**如果选择了"需要数据库变更"**：

```
请描述数据库变更内容：
- 变更类型（新增表/修改表/新增字段等）：
- 变更描述或SQL语句：
```

### 4.3 询问是否需要对接第三方接口

```javascript
{
  "questions": [{
    "question": "是否需要对接第三方接口？",
    "header": "第三方接口",
    "multiSelect": false,
    "options": [
      {
        "label": "需要对接第三方接口",
        "description": "需要调用第三方 API，请提供接口文档路径或链接"
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

**解析接口文档时的 URL 模式识别（CRITICAL）**：

当解析接口文档时，必须执行以下步骤：

1. **提取所有接口 URL**
   - 从接口文档中提取所有完整的 URL
   - 例如：
     ```
     https://apigw.zjhospital.net/ws-service/1618993775378471/1618988668813351
     https://apigw.zjhospital.net/ws-service/1618993775378471/1643313799102496
     https://apigw.zjhospital.net/ws-service/1618993775378471/1643314059149344
     ```

2. **识别共同的 base URL**
   - 分析所有 URL，找出共同的前缀部分
   - 例如上面的共同前缀：`https://apigw.zjhospital.net/ws-service/1618993775378471/`

3. **拆分为 base URL + path**
   - base URL：`https://apigw.zjhospital.net/ws-service/1618993775378471/`
   - path 列表：
     - `1618988668813351` (患者主索引接口)
     - `1643313799102496` (检查报告列表接口)
     - `1643314059149344` (检查报告详情接口)

4. **生成配置建议**
   ```
   ## URL 配置建议

   检测到多个接口共享相同的 base URL，建议使用以下配置方式：

   **Base URL 配置**：
   - remote.his.zjhospital.base.url=https://apigw.zjhospital.net/ws-service/1618993775378471/

   **各接口 Path 配置**：
   - remote.his.zjhospital.empid.path=1618988668813351
   - remote.his.zjhospital.ris.list.path=1643313799102496
   - remote.his.zjhospital.ris.detail.path=1643314059149344

   **代码中使用方式**：
   String url = baseUrl + pathConfig;

   这样可以减少配置冗余，提高可维护性。
   ```

5. **记录 URL 配置信息**
   - 将 base URL 和各 path 记录下来
   - 传递给后续的配置修改步骤使用

### 4.4 生成需求摘要

将收集到的所有信息整理为需求摘要：

```
## 后端需求摘要

### 基本信息
- 需求类型：[新增功能/修改现有功能/Bug修复/性能优化]
- 修改方式：[具体方式]

### 详细需求
[根据收集到的信息填充]

### 涉及内容
- 后端：[具体内容]
- 数据库：[如有]
- 第三方接口：[如有]
```

**记录所有收集到的信息**，传递给后续步骤使用。
