# 后端代码执行器

本文件是 fullstack-development-helper 的后端执行模块，包含后端代码修改的详细执行流程。

## 步骤6：执行后端代码修改

**注意：修改方式已在步骤2中选择，本步骤直接根据已选择的修改方式执行，不再重复询问。**

### 根据修改方式执行对应流程

**方式1：直接描述修改**

1. 根据步骤2收集的修改描述，使用 Grep/Glob 工具定位相关文件
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
3. 根据步骤2收集的修改描述执行修改

**方式4：新增API或功能**

1. 根据步骤2收集的新增信息，确定文件位置和命名
2. 参考现有代码的分层结构和风格（步骤1.5）
3. 按分层创建/修改文件：Controller → Service → Mapper/DAO → Entity/Model
4. 如需要数据库变更，生成SQL语句

### 执行代码修改

1. 使用 Grep/Glob 工具定位需要修改的文件
2. 使用 Read 工具读取文件内容
3. 使用 Edit/Write 工具修改代码
4. 确认修改完成

### 关键注意事项：数据状态逻辑修改

**CRITICAL：修改数据状态字段（如status、state等）时，必须先理解完整的数据流**

#### 常见错误模式

❌ **错误做法**：只看Controller层的赋值语句
```java
// Controller层
simpleVo.setStatus(dVo.getStatus()); // 看到这里就以为是修改点
```

✅ **正确做法**：追溯数据流，找到真正设置状态的地方
```
数据流：Controller → Service → 调用外部接口 → 数据聚合/处理 → 设置状态
真正的修改点通常在：Service层的数据聚合逻辑中
```

#### 必须询问的问题

在修改状态逻辑前，**必须先问用户**：

1. **"外部接口没返回数据时，系统现在是怎么处理的？"**
   - 是否会补齐空数据？
   - 补齐的数据状态是什么？

2. **"这个状态字段是在哪一层设置的？"**
   - Controller层？（通常只是转换）
   - Service层？（通常是真正的业务逻辑）
   - Mapper/DAO层？（通常只是数据映射）

3. **"能否看一下调用外部接口的Service层代码？"**
   - 找到数据聚合逻辑
   - 找到状态判断逻辑

#### 典型案例：HIS接口返回状态判断

**场景**：需要区分"外部接口返回但数据为空"和"外部接口没返回"

**错误位置**：Controller层的 `createXxxVo` 方法
```java
// ❌ 在这里判断无法区分两种情况
vo.setStatus(dVo.getLeftNum() == 0 ? 2 : dVo.getStatus());
```

**正确位置**：Service层调用外部接口后的数据聚合逻辑
```java
// ✅ 在这里判断，可以区分
if (hasDeptData) {  // 外部接口有返回
    for (JSONObject schedule : schedules) {
        int leftNum = schedule.getInteger("kyys");
        // 外部接口返回了数据：leftNum > 0 为有号(1)，leftNum == 0 为约满(2)
        vo.setStatus(leftNum > 0 ? 1 : 2);
    }
}
// 外部接口没返回的日期，补齐空数据
for (String date : allDates) {
    if (!dateMap.containsKey(date)) {
        // 外部接口没返回：status = 0（无号源）
        emptyVo.setStatus(0);
    }
}
```

#### 执行步骤

修改状态逻辑时，按以下步骤执行：

1. **先询问用户业务细节**（上述3个问题）
2. **追溯数据流**：从Controller → Service → 外部接口调用
3. **定位真正的状态设置点**：通常在Service层的数据聚合逻辑中
4. **理解现有的补齐/默认值逻辑**
5. **在正确的位置修改状态判断**

### 重要约束

- 必须严格遵循步骤1.5中分析的后端代码风格
- 严格遵循用户指定的修改范围，没有要求改动的地方必须保持原样
- **修改状态逻辑前，必须先理解完整的数据流和业务规则**
- 修改完成后验证代码正确性

---

**相关引用**

- 项目结构分析：参考核心 SKILL.md 步骤1
- 代码风格指南：参考核心 SKILL.md 步骤1.5
- 接口文档处理流程：参考核心 SKILL.md 接口文档处理流程
