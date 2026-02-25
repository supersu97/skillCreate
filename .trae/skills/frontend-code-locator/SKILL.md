---
name: "frontend-code-locator"
description: "帮助前端开发者快速定位前端代码位置，根据功能描述、组件名、接口调用等找到对应的前端代码实现。当需要查找特定功能的前端代码位置、定位前端问题代码或理解前端代码结构时调用。"
---

# 前端代码定位助手

*本技能专为Claude Code设计，帮助前端开发者快速定位和查找前端代码中的特定位置。*

## 功能介绍

这个技能让前端开发者能够：

1. **代码定位**：根据功能描述快速找到相关的前端代码实现
2. **组件查找**：根据组件名、页面名快速定位前端组件
3. **问题定位**：根据前端错误信息或问题描述定位问题代码
4. **定义查找**：根据变量名、方法名或函数名查找定义和使用位置
5. **代码导航**：分析前端代码结构，提供清晰的代码导航路径
6. **关联查找**：查找相关的组件、页面、服务、工具函数等关联代码

## 何时调用

当你遇到以下情况时，应该调用这个技能：

- 需要找到某个特定功能的前端实现代码位置
- 根据组件名、页面名快速定位前端组件
- 根据前端错误信息或问题描述定位问题所在代码
- 想要了解某个变量、方法或函数的定义和使用位置
- 需要理解前端代码库的结构和导航路径
- 查找相关的组件、页面、服务、工具函数等关联代码
- 想要快速了解某个功能的完整前端代码实现链路

## 使用方法

### 基本用法

1. **指定查找目标**：支持以下格式指定要查找的内容：
   - **功能描述**：如"用户登录功能"、"订单支付流程"
   - **组件/页面名**：如 `LoginForm`、`UserProfilePage`
   - **错误信息**：如"组件未定义"、"状态更新错误"
   - **变量/函数名**：如 `userService`、`handleSubmit`
   - **文件路径**：如 `src/components/LoginForm.vue`
   - **API调用**：如 `axios.post('/api/auth/login')`

2. **描述查找需求**：详细说明你想要查找的具体前端内容
3. **指定查找范围**：（可选）指定要查找的前端代码类型（组件、页面、服务、工具函数等）
4. **等待定位结果**：AI会分析前端代码并找到对应的位置
5. **查看定位结果**：获取详细的前端代码位置信息和导航路径

### 交互式查找流程（推荐）

当你调用本技能时，系统会提供交互式查找流程：

1. **选择查找类型**：
   ```
   请选择要查找的内容类型：
   1. 根据功能描述查找前端代码
   2. 根据组件/页面名定位组件
   3. 根据错误信息定位问题代码
   4. 根据变量/函数名查找定义和使用
   5. 分析前端代码结构和导航路径
   6. 查找相关的组件和页面关联关系
   ```

2. **提供查找信息**：根据你的选择，系统会询问相应的前端相关信息
3. **执行查找**：系统会分析前端代码并找到对应的位置
4. **查看结果**：查看找到的前端代码位置和详细信息

### 高级用法

#### 1. 根据功能描述查找前端代码

```
请帮我找到用户注册功能的前端完整实现代码：
- 注册表单组件及其子组件
- 注册页面布局和样式
- 注册相关的状态管理
- 注册表单验证逻辑
- 注册API调用服务
```

#### 2. 根据组件名或API调用查找代码

```
请帮我找到以下组件或API调用的完整前端实现链路：
查找目标：LoginForm组件或登录API调用
需要查找：
1. LoginForm组件的定义和使用位置
2. 登录相关的状态管理代码
3. 登录表单验证逻辑
4. 登录API调用服务及其调用位置
```

#### 3. 根据错误信息定位前端问题代码

```
请帮我定位以下前端错误对应的代码：
错误信息："组件LoginForm未定义"
错误发生在页面加载过程中
```

#### 4. 根据变量/函数名查找定义和使用

```
请帮我查找以下函数的所有定义和使用位置：
函数名：validateLoginForm
查找范围：前端项目代码
```

#### 5. 分析前端代码结构

```
请帮我分析用户管理模块的前端代码结构：
- 用户列表页面组件
- 用户详情页面组件
- 用户表单组件
- 用户相关的状态管理
- 用户API调用服务
```

#### 6. 查找前端组件关联关系

```
请帮我建立用户管理功能的前端组件关联关系：
- 用户列表页面和相关组件
- 用户详情页面和相关组件
- 用户表单组件和相关子组件
- 用户状态管理和相关组件
```

## 查找结果格式

查找完成后，会提供以下信息：

### 1. 代码位置信息
- **文件路径**：找到的代码文件路径
- **行号范围**：相关代码的行号范围
- **代码片段**：相关的代码片段预览
- **功能描述**：该代码的功能描述

### 2. 导航路径
- **代码层级**：代码在项目结构中的层级
- **依赖关系**：与其他代码的依赖关系
- **调用链路**：代码的调用链路和流程

### 3. 相关代码
- **组件代码**：相关的Vue/React组件、页面等
- **服务代码**：相关的API调用服务、工具函数等
- **状态管理**：相关的状态管理代码（Vuex、Redux等）
- **工具函数**：相关的工具函数、工具类等
- **样式文件**：相关的CSS/SCSS样式文件

## 示例输出

### 示例1：根据功能描述查找

```
# 用户登录功能代码定位结果

## 前端代码位置
1. **登录页面组件**：src/pages/LoginPage.vue (行号: 45-120)
   ```vue
   <!-- 登录表单组件 -->
   <template>
     <form @submit="handleLogin">
       <input v-model="username" type="text" placeholder="用户名">
       <input v-model="password" type="password" placeholder="密码">
       <button type="submit">登录</button>
     </form>
   </template>
   ```

2. **登录服务**：src/services/authService.js (行号: 25-60)
   ```javascript
   export const login = async (username, password) => {
     const response = await axios.post('/api/auth/login', {
       username,
       password
     });
     return response.data;
   };
   ```

## 相关前端代码
3. **登录状态管理**：src/store/auth.js (行号: 30-65)
   ```javascript
   export const useAuthStore = defineStore('auth', {
     state: () => ({
       user: null,
       isLoggedIn: false
     }),
     actions: {
       async login(credentials) {
         const user = await authService.login(credentials);
         this.user = user;
         this.isLoggedIn = true;
       }
     }
   });
   ```

4. **登录表单验证**：src/utils/validators.js (行号: 45-70)
   ```javascript
   export const validateLoginForm = (formData) => {
     const errors = {};
     if (!formData.username) errors.username = '用户名不能为空';
     if (!formData.password) errors.password = '密码不能为空';
     return errors;
   };
   ```

## 完整前端调用链路
用户输入 → LoginForm组件 → validateLoginForm() → authService.login() → axios.post('/api/auth/login') → 状态更新 → 页面跳转
```

### 示例2：根据组件名查找

```
# 组件定位结果：UserProfile组件

## 组件定义位置
1. **主组件文件**：src/components/UserProfile.vue (行号: 25-120)
   ```vue
   <template>
     <div class="user-profile">
       <UserAvatar :user="user" />
       <UserInfo :user="user" />
       <UserActions :user="user" />
     </div>
   </template>
   
   <script>
   import UserAvatar from './UserAvatar.vue';
   import UserInfo from './UserInfo.vue';
   import UserActions from './UserActions.vue';
   
   export default {
     name: 'UserProfile',
     components: { UserAvatar, UserInfo, UserActions },
     props: ['user']
   }
   </script>
   ```

## 相关子组件
2. **用户头像组件**：src/components/UserAvatar.vue (行号: 15-50)
   ```vue
   <template>
     <div class="user-avatar">
       <img :src="user.avatar" :alt="user.name" />
     </div>
   </template>
   ```

3. **用户信息组件**：src/components/UserInfo.vue (行号: 20-65)
   ```vue
   <template>
     <div class="user-info">
       <h3>{{ user.name }}</h3>
       <p>{{ user.email }}</p>
       <p>{{ user.bio }}</p>
     </div>
   </template>
   ```

4. **用户操作组件**：src/components/UserActions.vue (行号: 30-75)
   ```vue
   <template>
     <div class="user-actions">
       <button @click="editProfile">编辑资料</button>
       <button @click="sendMessage">发送消息</button>
     </div>
   </template>
   ```

## 相关样式文件
5. **组件样式**：src/components/UserProfile.css (行号: 10-45)
   ```css
   .user-profile {
     display: flex;
     flex-direction: column;
     gap: 20px;
     padding: 20px;
     border: 1px solid #e0e0e0;
   }
   ```

## 组件使用位置
- src/pages/ProfilePage.vue (行号: 45-60)
- src/components/UserList.vue (行号: 80-95)
- src/views/UserDetail.vue (行号: 120-135)
```

## 注意事项

1. **代码更新**：查找结果基于当前代码库状态，代码更新后可能需要重新查找
2. **精确匹配**：尽量提供详细的描述信息，有助于更精确地定位代码
3. **代码安全**：查找过程中不会修改任何代码，只进行读取和分析
4. **性能考虑**：大型项目可能需要更多时间进行分析

---

*使用本技能可以快速定位前端代码位置，理解前端代码结构，提高前端开发效率和代码理解能力。特别适用于大型前端项目和团队协作场景。*