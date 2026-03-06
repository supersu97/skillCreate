# 后端代码执行器（优化版）

本文件是 fullstack-development-helper-v2 的后端执行模块，包含后端代码修改的详细执行流程。

## v2 优化说明

相比 v1 版本：
- ✅ 移除了过于具体的 HIS 接口案例
- ✅ 抽象为通用的数据流分析原则
- ✅ 强调追溯数据源和理解业务逻辑

## 步骤 8：执行后端代码修改

**注意：修改方式已在步骤 4 中收集，本步骤直接根据已收集的修改方式执行，不再重复询问。**

### 根据修改方式执行对应流程

**方式1：直接描述修改**

1. 根据步骤 4 收集的修改描述，使用 Grep/Glob 工具定位相关文件
2. 使用 Read 工具读取文件内容
3. 分析代码，确定修改点
4. 使用 Edit/Write 工具执行修改

**方式2：根据接口信息修改**

1. 如果提供了接口路径：
   ```javascript
   调用 Grep 工具：
   - path: [后端项目路径]
   - pattern: [接口路径关键词]
   - glob: "**/*.{java,py,js,ts}"
   ```

2. 如果提供了接口文档：
   - 读取并解析接口文档（参考 SKILL.md 接口文档处理流程）
   - 提取接口信息

3. 定位后端中对应的Controller/Handler代码
4. 使用 Read 工具读取相关文件（Controller → Service → Mapper/DAO）
5. 根据接口信息修改后端代码

**方式3：指定类或文件修改**

1. 使用 Glob 工具查找指定文件：
   ```javascript
   调用 Glob 工具：
   - path: [后端项目路径]/src
   - pattern: "**/*[类名].java" 或 "**/*[类名].py"
   ```

2. 使用 Read 工具读取文件内容
3. 根据步骤 4 收集的修改描述执行修改

**方式4：新增API或功能**

1. 根据步骤 4 收集的新增信息，确定文件位置和命名
2. 参考现有代码的分层结构和风格（步骤 3.3）
3. 按分层创建/修改文件：Controller → Service → Mapper/DAO → Entity/Model
4. 如需要数据库变更，生成SQL语句

### 执行代码修改

**配置文件修改策略（CRITICAL）**

在修改配置文件（如 application.properties、application.yml、*.properties）时，必须遵循以下流程：

1. **检查配置使用情况**
   ```javascript
   // 在修改/删除配置前，先搜索该配置在代码中的使用情况
   调用 Grep 工具：
   - pattern: [配置变量名]
   - path: [后端项目路径]/src
   - output_mode: "files_with_matches"
   ```

2. **询问用户配置修改方式**
   ```javascript
   调用 AskUserQuestion 工具：
   {
     "questions": [{
       "question": "检测到需要修改配置文件，请选择配置修改方式？",
       "header": "配置修改",
       "multiSelect": false,
       "options": [
         {
           "label": "新增配置（推荐）",
           "description": "保留旧配置，新增新的配置项。适用于不影响现有功能的情况"
         },
         {
           "label": "修改现有配置",
           "description": "直接修改现有配置项。适用于确认要替换旧配置的情况"
         }
       ]
     }]
   }
   ```

3. **根据用户选择执行**

   **如果选择"新增配置"**：
   - 保留所有现有配置不变
   - 在配置文件中添加新的配置项
   - 在代码中注入新的配置变量
   - 使用新的配置变量调用接口

   **如果选择"修改现有配置"**：
   - 先确认该配置在代码中的所有使用位置
   - 询问用户确认影响范围
   - 修改配置值
   - 如果需要，同步修改相关代码

4. **URL 配置优化（如适用）**

   如果步骤 4 中识别到了共同的 base URL，应该：
   - 新增 base URL 配置项
   - 新增各接口的 path 配置项
   - 在代码中使用 `baseUrl + path` 的方式拼接完整 URL
   - 示例：
     ```java
     @Value("${remote.his.zjhospital.base.url}")
     private String zjhospitalBaseURL;

     @Value("${remote.his.zjhospital.ris.list.path}")
     private String zjhospitalRisListPath;

     // 使用时拼接
     String fullUrl = zjhospitalBaseURL + zjhospitalRisListPath;
     ```

5. **配置删除的安全检查**

   在删除任何配置前：
   - 必须先用 Grep 搜索该配置在整个项目中的使用
   - 确认没有任何地方在使用该配置
   - 如果有使用，必须先修改代码移除依赖，再删除配置
   - 如果不确定，保留配置不删除

1. 使用 Grep/Glob 工具定位需要修改的文件
2. 使用 Read 工具读取文件内容
3. 使用 Edit/Write 工具修改代码
4. 确认修改完成

### 关键注意事项：数据流相关逻辑修改（通用原则）

**CRITICAL：修改数据流相关逻辑时，必须先理解完整的数据流**

#### 通用原则

1. **追溯数据源**
   - 找到数据的真正来源（外部接口/数据库/计算）
   - 不要只看表面的赋值语句

2. **理解数据转换链**
   - Controller → Service → DAO/外部接口
   - 数据在每一层如何转换和处理

3. **定位业务逻辑层**
   - 通常在 Service 层，而不是 Controller 层
   - Controller 层通常只是数据转换和响应封装

4. **询问用户业务规则**
   - 数据为空时的默认行为是什么？
   - 异常情况如何处理？
   - 是否有特殊的业务规则？

#### 常见错误模式

❌ **错误做法**：只看 Controller 层的赋值语句
```java
// Controller层
simpleVo.setStatus(dVo.getStatus()); // 看到这里就以为是修改点
```

✅ **正确做法**：追溯到 Service 层的数据聚合逻辑
```
数据流：Controller → Service → 调用外部接口 → 数据聚合/处理 → 设置状态
真正的修改点通常在：Service层的数据聚合逻辑中
```

#### 执行步骤

修改数据流相关逻辑时，按以下步骤执行：

1. **先询问用户业务细节**
   - 外部接口/数据库没返回数据时，系统现在是怎么处理的？
   - 这个字段是在哪一层设置的？
   - 能否看一下调用外部接口/数据库的 Service 层代码？

2. **追溯数据流**
   - 从 Controller → Service → 外部接口调用/数据库查询

3. **定位真正的业务逻辑点**
   - 通常在 Service 层的数据聚合逻辑中

4. **理解现有的补齐/默认值逻辑**
   - 是否有数据补齐？
   - 默认值是什么？

5. **在正确的位置修改**
   - 不要在 Controller 层修改业务逻辑
   - 在 Service 层的正确位置修改

### 重要约束

- 必须严格遵循步骤 3.3 中分析的后端代码风格
- 严格遵循用户指定的修改范围，没有要求改动的地方必须保持原样
- **修改数据流相关逻辑前，必须先理解完整的数据流和业务规则**
- 修改完成后验证代码正确性

---

**相关引用**

- 项目结构分析：参考核心 SKILL.md 步骤 3.2
- 代码风格指南：参考核心 SKILL.md 步骤 3.3
- 接口文档处理流程：参考核心 SKILL.md 接口文档处理流程
