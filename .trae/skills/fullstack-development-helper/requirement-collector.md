# 需求收集器

本文件是 fullstack-development-helper 的需求收集模块，包含如何收集开发需求的详细流程。

## 步骤2：收集开发需求

**本步骤同时收集需求类型、修改方式和详细需求，后续步骤不再重复询问。**

使用 AskUserQuestion 工具，根据步骤0的选择显示不同的选项：

### 只修改前端时

```javascript
{
  "questions": [
    {
      "question": "请选择前端开发需求类型：",
      "header": "需求类型",
      "multiSelect": false,
      "options": [
        {
          "label": "新增功能",
          "description": "添加新的页面、组件或功能模块"
        },
        {
          "label": "修改现有功能",
          "description": "修改现有的前端功能或接口对接"
        },
        {
          "label": "Bug修复",
          "description": "修复前端Bug、样式问题或兼容性问题"
        },
        {
          "label": "性能优化",
          "description": "优化前端性能、用户体验或代码重构"
        }
      ]
    },
    {
      "question": "请选择前端修改方式：",
      "header": "修改方式",
      "multiSelect": false,
      "options": [
        {
          "label": "直接描述修改",
          "description": "直接描述要修改的内容和要求"
        },
        {
          "label": "根据接口信息修改",
          "description": "提供接口路径或接口文档，查找并修改前端对应代码"
        },
        {
          "label": "指定组件或文件修改",
          "description": "指定具体的组件、页面或文件进行修改"
        },
        {
          "label": "新增组件或页面",
          "description": "添加新的组件、页面或功能模块"
        }
      ]
    }
  ]
}
```

### 只修改后端时

```javascript
{
  "questions": [
    {
      "question": "请选择后端开发需求类型：",
      "header": "需求类型",
      "multiSelect": false,
      "options": [
        {
          "label": "新增功能",
          "description": "添加新的API接口或业务功能"
        },
        {
          "label": "修改现有功能",
          "description": "修改现有的API或业务逻辑"
        },
        {
          "label": "Bug修复",
          "description": "修复后端Bug、业务逻辑问题或异常处理"
        },
        {
          "label": "性能优化",
          "description": "优化后端性能、数据库查询或代码重构"
        }
      ]
    },
    {
      "question": "请选择后端修改方式：",
      "header": "修改方式",
      "multiSelect": false,
      "options": [
        {
          "label": "直接描述修改",
          "description": "直接描述要修改的内容和要求"
        },
        {
          "label": "根据接口信息修改",
          "description": "提供接口路径或接口文档，查找并修改后端对应代码"
        },
        {
          "label": "指定类或文件修改",
          "description": "指定具体的类、Service或文件进行修改"
        },
        {
          "label": "新增API或功能",
          "description": "添加新的API接口或业务功能"
        }
      ]
    },
    {
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
    }
  ]
}
```

### 同时修改前后端时

```javascript
{
  "questions": [
    {
      "question": "请选择开发需求类型：",
      "header": "需求类型",
      "multiSelect": false,
      "options": [
        {
          "label": "新增功能",
          "description": "开发一个全新的前后端功能"
        },
        {
          "label": "修改现有功能",
          "description": "修改现有的前后端功能或接口联调"
        },
        {
          "label": "Bug修复",
          "description": "修复前后端的Bug"
        },
        {
          "label": "性能优化",
          "description": "优化前后端性能或代码重构"
        }
      ]
    },
    {
      "question": "请选择前端修改方式：",
      "header": "前端方式",
      "multiSelect": false,
      "options": [
        {
          "label": "直接描述修改",
          "description": "直接描述前端要修改的内容"
        },
        {
          "label": "根据接口信息修改",
          "description": "根据后端接口路径或接口文档修改前端代码"
        },
        {
          "label": "指定组件或文件修改",
          "description": "指定具体的组件、页面或文件进行修改"
        },
        {
          "label": "新增组件或页面",
          "description": "添加新的前端组件、页面或功能模块"
        }
      ]
    },
    {
      "question": "请选择后端修改方式：",
      "header": "后端方式",
      "multiSelect": false,
      "options": [
        {
          "label": "直接描述修改",
          "description": "直接描述后端要修改的内容"
        },
        {
          "label": "根据接口信息修改",
          "description": "根据接口路径或接口文档修改后端代码"
        },
        {
          "label": "指定类或文件修改",
          "description": "指定具体的类、Service或文件进行修改"
        },
        {
          "label": "新增API或功能",
          "description": "添加新的API接口或业务功能"
        }
      ]
    },
    {
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
    }
  ]
}
```

### 收集详细需求

根据用户选择的修改方式，收集对应的详细信息：

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
1. 接口路径和请求方法（如：GET /api/users）
2. 接口文档位置（本地文件路径或在线链接）
```

**修改方式为"指定组件/文件/类修改"时**：

```
请提供要修改的文件信息：
- 文件路径或名称（如：src/components/UserList.vue 或 UserController）
- 修改内容描述：
```

**修改方式为"新增组件/页面/API"时**：

```
请提供新增内容信息：
- 名称：
- 位置：
- 功能描述：
- 包含的内容和交互逻辑：
```

**如果是前端修改且涉及UI/样式变更，额外收集**：

使用 AskUserQuestion 询问：
```javascript
{
  "questions": [{
    "question": "是否有UI设计图或样式要求？",
    "header": "UI设计",
    "multiSelect": false,
    "options": [
      {
        "label": "有设计图",
        "description": "提供设计图文件路径或截图"
      },
      {
        "label": "有样式描述",
        "description": "文字描述样式要求（颜色、布局、尺寸等）"
      },
      {
        "label": "参考现有组件",
        "description": "参考项目中已有的组件样式"
      },
      {
        "label": "无特殊要求",
        "description": "使用项目默认样式规范"
      }
    ]
  }]
}
```

**情况A：有设计图**
```
请提供设计图信息：
1. 设计图文件路径（支持 PNG/JPG/SVG）
2. 或者设计图的在线链接（Figma/蓝湖等）

如果是本地文件，将使用图像分析工具读取设计图，提取：
- 布局结构（flex/grid）
- 颜色值（主色、辅助色、文字颜色）
- 尺寸信息（宽高、间距、字体大小）
- 组件层级关系
```

**情况B：有样式描述**
```
请描述样式要求：
- 布局方式：（如：flex横向布局、grid网格、绝对定位等）
- 颜色：（如：主色#1890ff、背景色#f5f5f5）
- 尺寸：（如：宽度100%、高度200px、padding 16px）
- 字体：（如：标题18px bold、正文14px）
- 其他样式：（如：圆角、阴影、边框等）
```

**情况C：参考现有组件**
```
请提供参考组件的路径：
- 组件文件路径：（如：src/components/UserCard/index.tsx）
- 样式文件路径：（如：src/components/UserCard/index.scss）

将读取参考组件的样式，复用其设计规范。
```

**如果选择了"需要数据库变更"**：

```
请描述数据库变更内容：
- 变更类型（新增表/修改表/新增字段等）：
- 变更描述或SQL语句：
```

**记录所有收集到的信息**，传递给后续步骤使用，后续步骤不再重复询问。

---

**相关引用**

- 确认修改内容：参考 [confirmation.md](confirmation.md)
- 接口文档处理流程：参考核心 SKILL.md 接口文档处理流程
