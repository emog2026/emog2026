# 快速开始指南

## 🚀 5 分钟启动项目

### 1. 安装依赖

```bash
cd emog2026
npm install
```

### 2. 启动开发服务器

```bash
npm run dev
```

浏览器会自动打开 `http://localhost:3000`

## 📦 项目依赖说明

### 核心依赖
- `vue@^3.4.15` - Vue 3 框架
- `vue-router@^4.2.5` - 路由管理
- `pinia@^2.1.7` - 状态管理
- `axios@^1.6.5` - HTTP 请求
- `dayjs@^1.11.10` - 日期处理

### 开发依赖
- `vite@^5.0.11` - 构建工具
- `tailwindcss@^3.4.1` - CSS 框架
- `@vueuse/core@^10.7.2` - Vue 组合式工具集

## 🎨 样式自定义

### 修改主题色

编辑 `tailwind.config.js`:

```javascript
theme: {
  extend: {
    colors: {
      primary: '#3B82F6',  // 修改主色
      secondary: '#6366F1', // 修改辅色
      // 添加更多颜色
    }
  }
}
```

### 修改字体

编辑 `src/assets/main.css`:

```css
body {
  font-family: 'Your-Font', sans-serif;
}
```

## 🔗 API 集成

### 后端 API 配置

创建 `.env` 文件:

```env
VITE_API_BASE_URL=https://your-api.com/api
```

修改 `src/utils/api.js`:

```javascript
import axios from 'axios'

const api = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL,
  timeout: 10000,
})

// 替换模拟数据的函数
export const fetchTools = async () => {
  const response = await api.get('/tools')
  return response.data
}
```

## 📱 添加新页面

### 1. 创建页面组件

`src/pages/NewPage.vue`:

```vue
<template>
  <div>
    <h1>新页面</h1>
  </div>
</template>

<script setup>
// 页面逻辑
</script>
```

### 2. 添加路由

编辑 `src/router/index.js`:

```javascript
{
  path: '/new-page',
  name: 'NewPage',
  component: () => import('../pages/NewPage.vue')
}
```

## 🎯 添加新功能

### 添加搜索功能

已在 `src/components/Header.vue` 实现，通过事件传递搜索词：

```javascript
const handleSearch = (searchTerm) => {
  updateFilters({ search: searchTerm })
}
```

### 添加点赞功能

1. 在 `src/utils/api.js` 添加 API 调用:

```javascript
export const likeTool = async (toolId) => {
  const response = await api.post(`/tools/${toolId}/like`)
  return response.data
}
```

2. 在 `src/composables/useTools.js` 中调用:

```javascript
const likeTool = async (toolId) => {
  try {
    await likeToolApi(toolId)
    const tool = tools.value.find(t => t.id === toolId)
    if (tool) {
      tool.isLiked = !tool.isLiked
    }
  } catch (error) {
    console.error('点赞失败:', error)
  }
}
```

## 🚢 生产部署

### 构建项目

```bash
npm run build
```

输出目录: `dist/`

### 部署到 Vercel

```bash
npm install -g vercel
vercel
```

### 部署到 Netlify

```bash
npm install -g netlify-cli
netlify deploy --prod
```

## 🐛 常见问题

### 样式不生效

确保 `tailwind.config.js` 中的 `content` 配置正确:

```javascript
content: [
  "./index.html",
  "./src/**/*.{vue,js,ts,jsx,tsx}",
],
```

### 路由跳转报错

确保 `main.js` 中使用了 `createWebHistory()`:

```javascript
const router = createRouter({
  history: createWebHistory(),
  // ...
})
```

### API 跨域问题

在 `vite.config.js` 中配置代理:

```javascript
server: {
  proxy: {
    '/api': {
      target: 'http://localhost:3000',
      changeOrigin: true
    }
  }
}
```

## 📚 下一步

- [ ] 集成真实后端 API
- [ ] 添加用户认证
- [ ] 实现工具提交功能
- [ ] 添加评论系统
- [ ] 优化 SEO
- [ ] 添加单元测试
- [ ] 配置 CI/CD

## 💡 技巧

1. **使用 Vue DevTools**: 浏览器扩展，方便调试
2. **组合式函数复用**: 将可复用逻辑提取到 `composables/` 目录
3. **组件按需加载**: 使用路由懒加载减少初始包大小
4. **图片优化**: 使用 WebP 格式，添加压缩

---

需要帮助？查看 [README.md](./README.md) 或提交 Issue
