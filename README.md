# emog2026

基于 Vue3 + Tailwind CSS 构建的工具推荐网站。

## ✨ 特性

- 🚀 Vue3 + Vite 开发体验
- 🎨 Tailwind CSS 响应式设计
- 📦 模块化组件架构
- 🔍 实时搜索和筛选
- 📱 移动端友好
- ⚡ 性能优化
- 🎯 组合式 API

## 🛠️ 技术栈

- **框架**: Vue 3 (Composition API)
- **构建工具**: Vite
- **样式**: Tailwind CSS
- **路由**: Vue Router 4
- **状态管理**: Pinia
- **HTTP**: Axios
- **日期处理**: Day.js
- **工具库**: @vueuse/core
- **数据存储**: GitHub API + JSON 文件

## 📦 安装

```bash
# 克隆项目
git clone https://github.com/emog2026/emog2026.git
cd emog2026

# 安装依赖
npm install

# 配置 GitHub Token（必须）
cp .env.example .env
# 编辑 .env 文件，填入你的 GitHub Token
```

## 🚀 开发

```bash
# 启动开发服务器
npm run dev

# 构建生产版本
npm run build

# 预览生产构建
npm run preview
```

## 🌐 部署

### GitHub Pages (自动部署)

本项目已配置 GitHub Actions 自动部署到 GitHub Pages。

**访问地址**: https://emog2026.github.io/emog2026/

**触发方式**:
- 推送代码到 `main` 分支
- 在 GitHub Actions 页面手动触发

详细配置说明请查看: [GITHUB_PAGES_SETUP.md](./GITHUB_PAGES_SETUP.md)

### 本地构建

```bash
# 构建生产版本
npm run build

# 输出目录: dist/
```

### 其他平台

- **Vercel**: 参见 [DEPLOYMENT.md](./DEPLOYMENT.md)
- **Netlify**: 参见 [DEPLOYMENT.md](./DEPLOYMENT.md)
- **Docker**: 参见 [DEPLOYMENT.md](./DEPLOYMENT.md)

## 🔑 数据存储

本项目使用 **GitHub API + JSON 文件** 作为数据存储方案。

### 数据存储方式

- **文件位置**: `src/data/tools.json`
- **API**: GitHub REST API
- **读写权限**: 通过 GitHub Personal Access Token

### 配置步骤

1. **创建 GitHub Token**
   - 访问: https://github.com/settings/tokens
   - 生成新的 Personal Access Token
   - 权限: `public_repo` (公开仓库) 或 `repo` (私有仓库)

2. **配置环境变量**
   ```bash
   cp .env.example .env
   # 编辑 .env 文件
   VITE_GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
   ```

详细配置指南请查看: [GITHUB_API_SETUP.md](./GITHUB_API_SETUP.md)

## 📁 项目结构

```
src/
├── assets/           # 静态资源
├── components/       # 可复用组件
│   ├── Header.vue   # 顶部导航
│   ├── ToolCard.vue # 工具卡片
│   └── Pagination.vue # 分页组件
├── composables/      # 组合式函数
│   └── useTools.js  # 工具列表逻辑
├── layouts/         # 布局组件
├── pages/           # 页面组件
│   └── Home.vue     # 首页
├── router/          # 路由配置
├── stores/          # Pinia 状态管理
├── utils/           # 工具函数
│   └── api.js       # API 接口
├── App.vue          # 根组件
└── main.js          # 入口文件
```

## 🎯 核心功能

### 1. 工具列表展示
- 响应式网格布局
- 分类标签
- 浏览量和评论数统计
- 热门标记

### 2. 搜索和筛选
- 实时搜索
- 分类筛选
- 热门/最新排序
- 标签过滤

### 3. 分页
- 智能页码显示
- 上一页/下一页导航
- 页面跳转

### 4. 交互功能
- 点赞功能
- 外部链接跳转
- 响应式设计

## 🔧 配置说明

### 环境变量
创建 `.env` 文件:
```env
VITE_API_BASE_URL=http://localhost:3000/api
```

### Tailwind 配置
`tailwind.config.js` 已配置，可根据需要修改主题色和断点。

## 📝 后续开发计划

- [ ] 后端 API 集成
- [ ] 用户认证系统
- [ ] 工具提交功能
- [ ] 评论系统
- [ ] 收藏功能
- [ ] SEO 优化
- [ ] PWA 支持
- [ ] 暗黑模式

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📄 开源协议

MIT License

---

基于 [ababtools.com](https://ababtools.com/) 灵感开发
