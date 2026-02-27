# 项目位置收集器

本文件是 fullstack-development-helper 的项目位置收集模块，包含如何收集前端和后端项目位置的详细流程。

## 步骤0.1：收集项目位置信息

**根据步骤0的选择，只收集需要的项目位置**

### 0.1.1 如果只修改前端（只需收集前端项目路径）

**必须使用交互式选择**，执行以下流程：

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
        "description": "在当前目录或指定目录下自动查找前端项目"
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
请输入要检测的根目录（直接回车表示当前目录）：
```

然后使用 Glob 工具自动查找项目：

```javascript
调用 Glob 工具：
- path: [用户指定的目录]
- pattern: "**/package.json"
```

### 0.1.2 如果只修改后端（只需收集后端项目路径）

**必须使用交互式选择**，执行以下流程：

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
        "description": "在当前目录或指定目录下自动查找后端项目"
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
请输入要检测的根目录（直接回车表示当前目录）：
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

### 0.1.3 如果同时修改前后端（需要收集两个项目路径）

**必须使用交互式选择**，执行以下流程：

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
        "description": "在当前目录或指定目录下自动查找前端和后端项目"
      },
      {
        "label": "使用脚手架目录",
        "description": "前端在脚手架文件夹内，后端在外部或其他位置"
      }
    ]
  }]
}
```

**情况A：手动输入路径**

1. 询问前端项目位置：
   ```
   请输入前端项目文件夹路径（例如：./my-vue-project 或 D:/projects/frontend）：
   ```

2. 询问后端项目位置：
   ```
   请输入后端项目文件夹路径（例如：./my-spring-boot 或 D:/projects/backend）：
   ```

**情况B：自动检测**

1. 询问要在哪个目录检测：
   ```
   请输入要检测的根目录（直接回车表示当前目录）：
   ```

2. 使用 Glob 工具自动查找项目：
   ```javascript
   // 查找前端项目特征文件
   调用 Glob 工具：
   - path: [用户指定的目录]
   - pattern: "**/package.json"
   
   调用 Glob 工具：
   - path: [用户指定的目录]
   - pattern: "**/vue.config.js"
   
   // 查找后端项目特征文件
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

3. 分析检测结果，确定项目类型和位置

**情况C：使用脚手架目录**

1. 询问脚手架根目录：
   ```
   请输入脚手架项目的根目录（例如：./scaffold 或 D:/projects/scaffold）：
   ```

2. 询问前端项目在脚手架中的位置：
   ```
   请输入前端项目在脚手架中的相对路径（例如：client、frontend、app）：
   ```

3. 询问后端项目位置：
   ```
   请输入后端项目位置：
   1. 在脚手架内的相对路径（例如：server、backend）
   2. 在脚手架外部的绝对路径或相对路径
   ```

4. 使用 Glob/Read 工具验证项目结构：
   ```javascript
   // 验证前端项目
   调用 Glob 工具：
   - path: [脚手架目录]/[前端相对路径]
   - pattern: "package.json"
   
   // 验证后端项目
   调用 Glob 工具：
   - path: [后端目录]
   - pattern: "pom.xml" 或 "build.gradle" 或 "requirements.txt"
   ```

### 0.1.4 确认项目位置

根据选择的修改范围显示检测到的项目：

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

- 修改范围选择：参考核心 SKILL.md 步骤0
- 项目结构分析：参考核心 SKILL.md 步骤1
