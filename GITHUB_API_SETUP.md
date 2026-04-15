# GitHub API 数据存储配置指南

本项目使用 GitHub API + JSON 文件作为数据存储方案。

## 📋 概述

- **数据存储**: GitHub 仓库中的 JSON 文件 (`src/data/tools.json`)
- **数据访问**: GitHub REST API
- **数据管理**: 通过 API 读写 JSON 文件

## 🔧 配置步骤

### 1. 创建 GitHub Personal Access Token

#### 1.1 生成 Token

1. 访问 https://github.com/settings/tokens
2. 点击 **"Generate new token"** → **"Generate new token (classic)"**
3. 设置 Token 描述，例如：`emog2026-project`
4. 选择权限：
   - ✅ **public_repo** - 如果仓库是公开的
   - ✅ **repo** - 如果仓库是私有的
   - ✅ **workflow** - 如果需要 GitHub Actions（可选）
5. 点击 **"Generate token"**
6. **重要：立即复制 token**（只显示一次）

#### 1.2 权限说明

| 权限 | 用途 |
|------|------|
| `public_repo` | 读取和写入公开仓库 |
| `repo` | 读取和写入所有仓库（包括私有） |
| `workflow` | 创建和管理 GitHub Actions 工作流 |

### 2. 配置环境变量

#### 2.1 创建 .env 文件

在项目根目录创建 `.env` 文件：

```bash
# 复制示例文件
cp .env.example .env
```

#### 2.2 编辑 .env 文件

```env
VITE_GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

将 `ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` 替换为你的实际 token。

#### 2.3 验证配置

确保 `.env` 文件已正确创建且内容有效：

```bash
cat .env
```

应该输出：
```
VITE_GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

### 3. 验证配置

#### 3.1 本地测试

启动开发服务器：

```bash
npm run dev
```

在浏览器控制台查看是否有 API 错误。

#### 3.2 测试 GitHub API 连接

在浏览器控制台运行：

```javascript
// 测试 GitHub API
fetch('https://api.github.com/repos/emog2026/emog2026/contents/src/data/tools.json')
  .then(res => res.json())
  .then(data => console.log('✅ GitHub API 连接成功:', data))
  .catch(err => console.error('❌ GitHub API 连接失败:', err))
```

## 📊 数据文件结构

### src/data/tools.json

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
    "tags": ["tag1", "tag2"],
    "isHot": false,
    "image": ""
  }
]
```

## 🔧 API 使用说明

### 读取工具列表

```javascript
import { fetchTools } from './utils/github.js'

const tools = await fetchTools()
console.log(tools)
```

### 创建新工具

```javascript
import { createToolOnGitHub } from './utils/github.js'

const newTool = {
  title: "新工具",
  category: "software",
  categoryLabel: "软件",
  description: "工具描述",
  url: "https://example.com",
  tags: ["标签1", "标签2"]
}

const result = await createToolOnGitHub(newTool)
```

### 更新工具

```javascript
import { updateToolOnGitHub } from './utils/github.js'

const result = await updateToolOnGitHub(1, {
  views: 101
})
```

### 删除工具

```javascript
import { deleteToolOnGitHub } from './utils/github.js'

const result = await deleteToolOnGitHub(1)
```

## ⚠️ 重要注意事项

### 1. Token 安全

- ❌ **不要**将 `.env` 文件提交到 Git
- ❌ **不要**在代码中硬编码 token
- ❌ **不要**在公开的 GitHub Issue 或 Pull Request 中分享 token
- ✅ **使用** `.env.example` 作为示例文件
- ✅ **定期**轮换 token（每 30-90 天）

### 2. API 速率限制

GitHub API 有速率限制：

| 认证状态 | 每小时请求数 |
|---------|------------|
| 已认证 | 5,000 |
| 未认证 | 60 |

**解决方案**：
- 使用 token 认证（推荐）
- 实现请求缓存
- 优化 API 调用频率

### 3. 文件大小限制

- 单个文件最大 100MB
- 对于大量数据，考虑分片存储

### 4. 并发写入

GitHub API 不支持高并发写入同一文件。

**解决方案**：
- 实现简单的锁机制
- 使用 Git 的原子提交
- 考虑使用数据库（如 SQLite）作为中间层

## 🔄 备选方案

如果 GitHub API 遇到问题，可以回退到：

### 方案 1: 本地 JSON 文件

直接读取本地 `src/data/tools.json`：

```javascript
const response = await fetch('/src/data/tools.json')
const data = await response.json()
```

### 方案 2: 使用其他服务

- **JSONBin.io**: https://jsonbin.io
- **Firebase**: https://firebase.google.com
- **Supabase**: https://supabase.com

## 📚 相关资源

- [GitHub REST API 文档](https://docs.github.com/en/rest)
- [Personal Access Tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
- [Git Ignore 模式](https://git-scm.com/docs/gitignore)

## 🐛 故障排除

### 问题 1: 403 Forbidden

**原因**: Token 权限不足或 token 无效

**解决**:
1. 检查 token 是否有 `public_repo` 权限
2. 重新生成 token
3. 更新 `.env` 文件

### 问题 2: 404 Not Found

**原因**: 仓库或文件不存在

**解决**:
1. 确认仓库地址正确：`emog2026/emog2026`
2. 检查文件路径：`src/data/tools.json`

### 问题 3: 401 Unauthorized

**原因**: Token 未配置或格式错误

**解决**:
1. 检查 `.env` 文件是否存在
2. 确认 token 格式正确（`ghp_xxxxx`）
3. 重启开发服务器

### 问题 4: 422 Unprocessable Entity

**原因**: 请求体格式错误或必填字段缺失

**解决**:
1. 检查 JSON 格式
2. 确认所有必填字段都已提供
3. 查看错误消息中的详细信息

---

**配置完成后，你的项目就可以从 GitHub 仓库读取和写入数据了！** 🎉
