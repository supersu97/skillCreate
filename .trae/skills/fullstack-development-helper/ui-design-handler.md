# UI设计图处理模块

本文件是 fullstack-development-helper 的UI设计图处理模块，用于处理前端UI修改时的设计图分析。

## 何时使用本模块

当满足以下条件时，必须使用本模块：
- 前端修改涉及UI/样式变更
- 用户提供了设计图或UI截图
- 需要新增页面或组件且有设计稿

## 设计图处理流程

### 步骤1：获取设计图

**情况A：本地设计图文件**
```javascript
调用 Read 工具读取图片文件：
- file_path: [用户提供的设计图路径]
- 支持格式：PNG, JPG, JPEG, SVG, WebP
```

**情况B：在线设计图链接**
```javascript
// 如果是Figma/蓝湖等设计工具链接
提示用户：
"请提供设计图的截图或导出的PNG/JPG文件，因为在线设计工具需要登录权限。"

// 如果是图片直链
调用 WebFetch 工具：
- url: [设计图URL]
- prompt: "分析这张UI设计图，提取布局、颜色、尺寸等信息"
```

**情况C：用户描述UI**
```
跳过设计图分析，直接根据用户的文字描述生成UI代码。
```

### 步骤2：分析设计图

使用图像分析能力（如果可用）或引导用户描述设计图内容：

**自动分析（优先）**：
```javascript
// 如果有图像分析工具
调用图像分析工具：
- image_path: [设计图路径]
- analysis_type: "ui_design"
- extract: ["layout", "colors", "typography", "spacing", "components"]

提取信息：
1. 布局结构
   - 整体布局方式（flex/grid/absolute）
   - 组件层级关系
   - 对齐方式

2. 颜色信息
   - 主色调（提取主要颜色的hex值）
   - 背景色
   - 文字颜色
   - 边框颜色

3. 尺寸信息
   - 组件宽高
   - 内外边距
   - 字体大小
   - 图标尺寸

4. 组件识别
   - 按钮（样式、尺寸、圆角）
   - 输入框（边框、高度、placeholder）
   - 卡片（阴影、圆角、间距）
   - 列表项（高度、分割线）
   - 图标（位置、大小）
```

**手动描述（备选）**：
```
如果无法自动分析，询问用户：

请描述设计图的以下信息：

1. 整体布局：
   - 是横向还是纵向排列？
   - 有几个主要区域？
   - 各区域的位置关系？

2. 颜色：
   - 主色调是什么颜色？（如：蓝色、绿色）
   - 背景色是什么？（如：白色、灰色）
   - 文字颜色？（如：黑色、深灰）

3. 组件：
   - 有哪些UI组件？（如：按钮、输入框、卡片）
   - 每个组件的样式特点？（如：圆角按钮、带阴影的卡片）

4. 尺寸：
   - 大致的宽高比例？
   - 组件之间的间距？（如：紧密、宽松）
```

### 步骤3：生成样式代码

根据分析结果，生成对应的样式代码：

**SCSS示例**：
```scss
// 从设计图提取的样式变量
$primary-color: #1890ff;  // 从设计图提取的主色
$bg-color: #f5f5f5;       // 背景色
$text-color: #333;        // 文字颜色
$border-radius: 8px;      // 圆角
$spacing: 16px;           // 间距

.container {
  display: flex;
  flex-direction: column;
  padding: $spacing;
  background-color: $bg-color;

  .card {
    background: #fff;
    border-radius: $border-radius;
    padding: $spacing;
    margin-bottom: $spacing;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);

    .title {
      font-size: 18px;
      font-weight: bold;
      color: $text-color;
      margin-bottom: 8px;
    }

    .button {
      background-color: $primary-color;
      color: #fff;
      border: none;
      border-radius: $border-radius;
      padding: 8px 16px;
      cursor: pointer;
    }
  }
}
```

**CSS Modules示例**：
```css
.container {
  display: flex;
  flex-direction: column;
  padding: 16px;
  background-color: #f5f5f5;
}

.card {
  background: #fff;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.title {
  font-size: 18px;
  font-weight: bold;
  color: #333;
  margin-bottom: 8px;
}

.button {
  background-color: #1890ff;
  color: #fff;
  border: none;
  border-radius: 8px;
  padding: 8px 16px;
  cursor: pointer;
}
```

### 步骤4：生成组件结构

根据设计图的组件层级，生成对应的组件代码：

**React + TypeScript示例**：
```tsx
import React from 'react';
import styles from './index.scss';

interface CardProps {
  title: string;
  content: string;
  onButtonClick: () => void;
}

const Card: React.FC<CardProps> = ({ title, content, onButtonClick }) => {
  return (
    <div className={styles.card}>
      <div className={styles.title}>{title}</div>
      <div className={styles.content}>{content}</div>
      <button className={styles.button} onClick={onButtonClick}>
        确定
      </button>
    </div>
  );
};

export default Card;
```

**Vue 3示例**：
```vue
<template>
  <div :class="$style.card">
    <div :class="$style.title">{{ title }}</div>
    <div :class="$style.content">{{ content }}</div>
    <button :class="$style.button" @click="handleClick">
      确定
    </button>
  </div>
</template>

<script setup lang="ts">
interface Props {
  title: string;
  content: string;
}

const props = defineProps<Props>();
const emit = defineEmits<{
  buttonClick: [];
}>();

const handleClick = () => {
  emit('buttonClick');
};
</script>

<style module lang="scss">
.card {
  background: #fff;
  border-radius: 8px;
  padding: 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.title {
  font-size: 18px;
  font-weight: bold;
  color: #333;
  margin-bottom: 8px;
}

.button {
  background-color: #1890ff;
  color: #fff;
  border: none;
  border-radius: 8px;
  padding: 8px 16px;
  cursor: pointer;
}
</style>
```

### 步骤5：适配项目样式规范

**关键**：生成的样式必须符合项目现有的样式规范（步骤1.5中分析的）

1. **使用项目的样式变量**
   ```scss
   // 不要硬编码颜色
   ❌ color: #1890ff;

   // 使用项目的样式变量
   ✅ color: $primary-color;
   ✅ color: var(--primary-color);
   ```

2. **遵循项目的类名命名规范**
   ```scss
   // 如果项目使用BEM命名
   .card {}
   .card__title {}
   .card__button {}

   // 如果项目使用语义化命名
   .card {}
   .title {}
   .button {}
   ```

3. **使用项目的间距规范**
   ```scss
   // 不要随意设置间距
   ❌ padding: 15px;

   // 使用项目的间距变量
   ✅ padding: $spacing-md;  // 16px
   ✅ padding: var(--spacing-md);
   ```

4. **复用项目的组件样式**
   ```tsx
   // 如果项目已有类似的按钮组件，直接复用
   import Button from '@components/Button';

   <Button type="primary" onClick={handleClick}>
     确定
   </Button>
   ```

## 注意事项

1. **优先复用现有组件**
   - 检查项目中是否已有类似的组件
   - 能复用就不要重新实现

2. **保持样式一致性**
   - 新增的样式必须与项目整体风格一致
   - 颜色、字体、间距都要符合设计规范

3. **响应式设计**
   - 如果设计图有多个尺寸，生成对应的媒体查询
   - 使用项目的断点变量

4. **无障碍性**
   - 确保颜色对比度符合WCAG标准
   - 添加必要的aria属性

5. **性能优化**
   - 避免过度嵌套
   - 合理使用CSS选择器

---

**相关引用**

- 代码风格指南：参考核心 SKILL.md 步骤1.5
- 前端执行模块：参考 [frontend-executor.md](frontend-executor.md)
