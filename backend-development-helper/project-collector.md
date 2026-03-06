# 后端项目位置收集器

本文件是 backend-development-helper 的项目位置收集模块。

## 步骤 2：收集后端项目位置

### 2.1 检测当前目录是否是后端项目

```javascript
// 第一步：检查是否存在后端项目特征文件
调用 Glob 工具：
- path: .
- pattern: "pom.xml"

调用 Glob 工具：
- path: .
- pattern: "build.gradle"

调用 Glob 工具：
- path: .
- pattern: "requirements.txt"

调用 Glob 工具：
- path: .
- pattern: "go.mod"

// 第二步：如果找到任一文件，进一步确认
如果找到 pom.xml：
  调用 Read 工具：
  - file_path: ./pom.xml

  // 检查是否是 Java 后端项目
  if (包含 <packaging>war</packaging> 或 <packaging>jar</packaging>):
    → 后端项目（Java Maven）

  if (包含 Spring 相关依赖):
    - spring-boot-starter
    - spring-webmvc
    - spring-context
    → 后端项目（Spring）

  if (包含 MyBatis 相关依赖):
    - mybatis
    - mybatis-spring
    → 后端项目（MyBatis）

如果找到 build.gradle：
  → 后端项目（Java Gradle）

如果找到 requirements.txt：
  → 后端项目（Python）

如果找到 go.mod：
  → 后端项目（Go）

如果判断为后端项目：
  询问用户："检测到当前目录是后端项目，是否使用？"
  如果确认，使用当前目录，跳过后续步骤
```

### 2.2 如果当前目录不是后端项目，使用交互式选择

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
        "description": "在指定目录下自动查找后端项目"
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
请输入要检测的根目录（直接回车表示当前目录的父目录）：
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

调用 Glob 工具：
- path: [用户指定的目录]
- pattern: "**/go.mod"
```

### 2.3 确认项目位置

```
检测到以下后端项目：
- 后端项目：[后端路径]
  技术栈：[Spring Boot/Django/Express + 具体框架版本]
  入口文件：[pom.xml/build.gradle路径]

请确认项目位置是否正确？
```
