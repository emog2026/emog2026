# GitHub API 数据存储使用指南

## 🎯 快速开始

### 1. 配置 GitHub Token

**步骤：**

1. 访问 https://github.com/settings/tokens
2. 点击 **"Generate new token"** → **"Generate new token (classic)"**
3. 输入描述（如：`emog2026-data`）
4. 勾选权限：
   - ✅ **public_repo** (如果仓库是公开的)
   - ✅ **repo** (如果仓库是私有的)
5. 点击 **"Generate token"**
6. **立即复制 token**（格式：`ghp_xxxxxxxxxxxxxxxxxxxx`）

### 2. 配置环境变量

在项目根目录创建 `.env` 文件：

```bash
cp .env.example .env
```

编辑 `.env` 文件：

```env
VITE_GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

### 3. 重启开发服务器

```bash
npm run dev
```

## 📊 数据文件

### src/data/tools.json

这是存储工具数据的主文件：

```json
[
  {
    "id": 1,
    "title": "工具名称",
    "category": "software",
    "categoryLabel": "软件",
    "description": "工具描述...",
    "url": "https://example.com",
    "views": 100,
    "comments": 0,
    "date": "2026-04-15",
    "tags": ["标签1", "标签2"],
    "isHot": false,
    "image": ""
  }
]
```

## 🔧 API 使用

### 读取工具列表

```javascript
import { fetchTools } from './utils/github.js'

// 从 GitHub 读取
const tools = await fetchTools()
console.log(tools)
```

### 创建新工具

```javascript
import { createToolOnGitHub } from './utils/github.js'

const newTool = {
  title: "新工具",
  category: "software",
  categoryLabel": "软件",
  description: "这是一个很棒的工具",
  url: "https://example.com/tool",
  tags: ["开源", "免费"]
}

const result = await createToolOnGitHub(newTool)
console.log('创建成功:', result)
```

### 更新工具

```javascript
import { updateToolOnGitHub } from './utils/github.js'

const result = await updateToolOnGitHub(1, {
  views: 101,
  isHot: true
})
console.log('更新成功:', result)
```

### 删除工具

```javascript
import { deleteToolOnGitHub } from './utils/github.js'

const result = await deleteToolOnGitHub(1)
console.log('删除成功:', result)
```

### 增加浏览量

```javascript
import { incrementViewsOnGitHub } from './utils/github.js'

const result = await incrementViewsOnGitHub(1)
console.log('浏览量增加:', result)
```

## 🎨 在组件中使用

### 示例 1: 在页面中读取工具

```vue
<script setup>
import { ref, onMounted } from 'vue'
import { fetchTools } from '../utils/github.js'

const tools = ref([])
const loading = ref(true)

const loadTools = async () => {
  try {
    const data = await fetchTools()
    tools.value = data
  } catch (error) {
    console.error('加载失败:', error)
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  loadTools()
})
</script>

<template>
  <div>
    <div v-if="loading">加载中...</div>
    <div v-else>
      <div v-for="tool in tools" :key="tool.id">
        {{ tool.title }} - {{ tool.views }} 浏览
      </div>
    </div>
  </div>
</template>
```

### 示例 2: 提交新工具

```vue
<script setup>
import { ref } from 'vue'
import { createToolOnGitHub } from '../utils/github.js'

const form = ref({
  title: '',
  category: 'software',
  categoryLabel: '软件',
  description: '',
  url: '',
  tags: []
})

const submitting = ref(false)

const handleSubmit = async () => {
  submitting.value = true

  try {
    const result = await createToolOnGitHub(form.value)
    alert('提交成功！')
    console.log('创建结果:', result)
  } catch (error) {
    alert('提交失败: ' + error.message)
  } finally {
    submitting.value = false
  }
}
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <input v-model="form.title" placeholder="工具名称">
    <textarea v-model="form.description" placeholder="工具描述"></textarea>
    <input v-model="form.url" placeholder="工具网址">
    <button type="submit" :disabled="submitting">
      {{ submitting ? '提交中...' : '提交' }}
    </button>
  </form>
</template>
```

## ⚠️ 重要注意事项

### 1. Token 安全

- ❌ **不要**将 `.env` 文件提交到 Git
- ✅ 使用 `.env.example` 作为示例
- ✅ 定期轮换 token（每 30-90 天）
- ✅ 如果 token 泄露，立即撤销并重新生成

### 2. API 速率限制

| 认证状态 | 每小时请求数 |
|---------|------------|
| 已认证 | 5,000 |
| 未认证 | 60 |

**优化建议**：
- 实现数据缓存
- 减少不必要的 API 调用
- 使用防抖和节流

### 3. 并发写入

GitHub API 不支持高并发写入同一文件。

**解决方案**：
- 实现简单的锁机制
- 使用队列处理写操作
- 考虑使用本地缓存 + 定期同步

### 4. 错误处理

```javascript
try {
  const tools = await fetchTools()
} catch (error) {
  if (error.message.includes('401')) {
    console.error('Token 无效，请检查 .env 文件')
  } else if (error.message.includes('403')) {
    console.error('Token 权限不足')
  } else if (error.message.includes('404')) {
    console.error('文件不存在')
  } else {
    console.error('未知错误:', error)
  }
}
```

## 🔍 故障排除

### 问题 1: Token 无效

**错误信息**: `401 Unauthorized`

**解决方案**:
1. 检查 `.env` 文件是否存在
2. 确认 token 格式正确（`ghp_xxxxx`）
3. 重新生成 token
4. 重启开发服务器

### 问题 2: 权限不足

**错误信息**: `403 Forbidden`

**解决方案**:
1. 检查 token 是否有 `public_repo` 权限
2. 确认仓库是否为公开仓库
3. 如果是私有仓库，需要 `repo` 权限

### 问题 3: 文件不存在

**错误信息**: `404 Not Found`

**解决方案**:
1. 确认仓库地址正确：`emog2026/emog2026`
2. 检查文件路径：`src/data/tools.json`
3. 在 GitHub 仓库中手动创建文件

### 问题 4: 速率限制

**错误信息**: `403 rate limit exceeded`

**解决方案**:
1. 等待 1 小时后重试
2. 使用 token 认证（提高限额到 5000/小时）
3. 实现请求缓存

## 🚀 高级用法

### 1. 数据缓存

```javascript
// 使用 localStorage 缓存数据
const CACHE_KEY = 'tools_cache'
const CACHE_EXPIRY = 5 * 60 * 1000 // 5 分钟

export async function fetchToolsWithCache() {
  const cached = localStorage.getItem(CACHE_KEY)

  if (cached) {
    const { data, timestamp } = JSON.parse(cached)
    if (Date.now() - timestamp < CACHE_EXPIRY) {
      console.log('使用缓存数据')
      return data
    }
  }

  console.log('从 GitHub 获取数据')
  const data = await fetchToolsFromGitHub()

  localStorage.setItem(CACHE_KEY, JSON.stringify({
    data,
    timestamp: Date.now()
  }))

  return data
}
```

### 2. 请求防抖

```javascript
// 防止短时间内多次提交
import { debounce } from '@vueuse/core'

const submitToolDebounced = debounce(async (tool) => {
  const result = await createToolOnGitHub(tool)
  console.log('提交成功:', result)
}, 1000)
```

### 3. 错误重试

```javascript
// 自动重试失败的请求
async function fetchWithRetry(fn, retries = 3) {
  try {
    return await fn()
  } catch (error) {
    if (retries > 0) {
      console.log(`重试中... 剩余 ${retries} 次`)
      await new Promise(resolve => setTimeout(resolve, 1000))
      return fetchWithRetry(fn, retries - 1)
    }
    throw error
  }
}

// 使用
const tools = await fetchWithRetry(fetchToolsFromGitHub)
```

## 📚 相关资源

- [GITHUB_API_SETUP.md](./GITHUB_API_SETUP.md) - 详细配置指南
- [GitHub REST API 文档](https://docs.github.com/en/rest)
- [GitHub Rate Limiting](https://docs.github.com/en/rest/overview/resources-in-the-rest-api#rate-limiting)

---

**现在你的项目已经集成了 GitHub API 数据存储！** 🎉

如果遇到问题，请参考 [GITHUB_API_SETUP.md](./GITHUB_API_SETUP.md) 或提交 Issue。
