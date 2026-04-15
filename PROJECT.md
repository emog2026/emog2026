# 项目概览

## 📋 项目信息

- **项目名称**: emog2026
- **版本**: 1.0.0
- **技术栈**: Vue3 + Vite + Tailwind CSS
- **许可证**: MIT

## 🎯 项目目标

基于 [ababtools.com](https://ababtools.com/) 的灵感，开发一个发现和推荐各类工具（软件、网站、AI工具等）的平台。

## 🏗️ 架构设计

### 前端架构

```
┌─────────────────────────────────────────┐
│            Browser / Client              │
├─────────────────────────────────────────┤
│  Vue3 Application                       │
│  ├─ Components (可复用组件)             │
│  ├─ Pages (页面组件)                     │
│  ├─ Composables (逻辑复用)              │
│  ├─ Router (路由管理)                   │
│  └─ Pinia Store (状态管理)               │
├─────────────────────────────────────────┤
│  API Layer (axios)                      │
├─────────────────────────────────────────┤
│  Backend API (待集成)                    │
└─────────────────────────────────────────┘
```

### 技术栈选择

| 技术 | 用途 | 理由 |
|------|------|------|
| Vue 3 | 前端框架 | 组合式 API、性能优秀、生态完善 |
| Vite | 构建工具 | 开发体验好、构建速度快 |
| Tailwind CSS | 样式框架 | 原子化 CSS、开发高效、响应式 |
| Vue Router | 路由 | 官方路由方案、功能强大 |
| Pinia | 状态管理 | Vue 3 推荐、TypeScript 友好 |
| Axios | HTTP 客户端 | 易用、功能丰富 |
| Day.js | 日期处理 | 轻量、API 简洁 |

## 📁 目录结构详解

```
emog2026/
├── public/              # 静态资源
│   └── vite.svg        # Vite 图标
├── src/
│   ├── assets/         # 资源文件
│   │   └── main.css   # 全局样式
│   ├── components/     # 可复用组件
│   │   ├── Header.vue      # 顶部导航栏
│   │   ├── ToolCard.vue    # 工具卡片
│   │   ├── Pagination.vue  # 分页组件
│   │   ├── Sidebar.vue     # 侧边栏（待开发）
│   │   └── LoadingSpinner.vue  # 加载动画（待开发）
│   ├── composables/    # 组合式函数
│   │   └── useTools.js     # 工具列表逻辑
│   ├── layouts/        # 布局组件
│   │   └── DefaultLayout.vue # 默认布局（待开发）
│   ├── pages/          # 页面组件
│   │   ├── Home.vue        # 首页
│   │   ├── ToolDetail.vue  # 工具详情页
│   │   ├── SubmitTool.vue  # 提交工具（待开发）
│   │   ├── Category.vue    # 分类页（待开发）
│   │   └── NotFound.vue    # 404 页面（待开发）
│   ├── router/         # 路由配置
│   │   └── index.js       # 路由定义
│   ├── stores/         # Pinia 状态管理
│   │   └── tools.js       # 工具状态（待开发）
│   ├── utils/          # 工具函数
│   │   └── api.js          # API 接口
│   ├── App.vue         # 根组件
│   └── main.js         # 入口文件
├── .gitignore          # Git 忽略文件
├── DEPLOYMENT.md       # 部署指南
├── index.html          # HTML 模板
├── package.json        # 项目配置
├── QUICKSTART.md       # 快速开始
├── README.md           # 项目说明
├── tailwind.config.js  # Tailwind 配置
├── vite.config.js      # Vite 配置
└── PROJECT.md          # 项目概览（本文件）
```

## 🔧 核心功能模块

### 1. 工具列表展示
- **文件**: `src/pages/Home.vue`
- **逻辑**: `src/composables/useTools.js`
- **组件**: `src/components/ToolCard.vue`

**功能点**:
- 响应式网格布局
- 工具卡片展示
- 分类标签
- 浏览量/评论数统计
- 热门标记

### 2. 搜索和筛选
- **文件**: `src/components/Header.vue`
- **逻辑**: `src/composables/useTools.js`

**功能点**:
- 实时搜索
- 分类筛选
- 标签过滤
- 排序（热门/最新）

### 3. 分页系统
- **文件**: `src/components/Pagination.vue`
- **逻辑**: `src/composables/useTools.js`

**功能点**:
- 智能页码显示
- 上一页/下一页
- 页面跳转

### 4. 工具详情
- **文件**: `src/pages/ToolDetail.vue`

**功能点**:
- 详细信息展示
- 外部链接跳转
- 收藏功能
- 相关工具推荐（待开发）

## 🔄 数据流

```
用户操作 → Header/Page → Event Emit
    ↓
useTools (Composable)
    ↓
updateFilters / likeTool
    ↓
computed (响应式数据)
    ↓
重新渲染 UI
```

## 🎨 样式系统

### Tailwind 配置

**主题色**:
- Primary: `#3B82F6` (蓝色)
- Secondary: `#6366F1` (紫色)
- Dark: `#1F2937` (深灰)
- Light: `#F9FAFB` (浅灰)

**断点**:
- `sm`: 640px
- `md`: 768px
- `lg`: 1024px
- `xl`: 1280px
- `2xl`: 1536px

## 🔌 API 接口设计

### 当前状态
- 使用模拟数据 (`src/utils/api.js`)
- 无真实后端连接

### 计划中的接口

```javascript
// 获取工具列表
GET /api/tools?page=1&category=software&search=keyword

// 获取工具详情
GET /api/tools/:id

// 提交工具
POST /api/tools/submit

// 点赞工具
POST /api/tools/:id/like

// 获取评论
GET /api/tools/:id/comments

// 提交评论
POST /api/tools/:id/comments

// 获取分类
GET /api/categories
```

## 🚀 开发工作流

### 本地开发

```bash
# 1. 启动开发服务器
npm run dev

# 2. 访问 http://localhost:3000

# 3. 热重载已启用，代码更改自动刷新
```

### 代码提交

```bash
# 1. 查看更改
git status

# 2. 添加文件
git add .

# 3. 提交更改
git commit -m "feat: add new feature"

# 4. 推送到远程
git push origin main
```

### 构建生产版本

```bash
# 构建
npm run build

# 输出目录: dist/

# 本地预览
npm run preview
```

## 📊 性能优化

### 已实现
- 路由懒加载
- 组件按需导入
- CSS 原子化（Tailwind）
- 构建压缩

### 待优化
- 图片懒加载
- 虚拟滚动（长列表）
- Service Worker 缓存
- CDN 静态资源

## 🧪 测试策略

### 计划中的测试
- 单元测试 (Vitest)
- 组件测试 (Vue Test Utils)
- E2E 测试 (Playwright)

## 📱 响应式设计

### 断点策略
- **Mobile**: < 768px - 单列布局
- **Tablet**: 768px - 1024px - 双列布局
- **Desktop**: > 1024px - 三列布局

## 🔒 安全考虑

### 待实现
- XSS 防护
- CSRF Token
- 内容安全策略 (CSP)
- API 速率限制
- 用户认证和授权

## 🌐 SEO 优化

### 待实现
- Meta 标签优化
- Sitemap 生成
- Robots.txt
- Open Graph 标签
- 结构化数据 (JSON-LD)

## 📈 监控和分析

### 待集成
- Google Analytics
- Sentry 错误监控
- 性能监控 (Web Vitals)

## 🎯 待开发功能

### 高优先级
- [ ] 后端 API 集成
- [ ] 用户认证系统
- [ ] 工具提交功能
- [ ] 评论系统

### 中优先级
- [ ] 收藏功能
- [ ] 用户个人中心
- [ ] 工具对比功能
- [ ] 相关推荐算法

### 低优先级
- [ ] 暗黑模式
- [ ] 多语言支持
- [ ] RSS 订阅
- [ ] 社交分享

## 🤝 贡献指南

### 代码规范
- 使用 ESLint 进行代码检查
- 使用 Prettier 格式化代码
- 遵循 Vue 3 组合式 API 风格

### 提交规范
```
feat: 新功能
fix: 修复 bug
docs: 文档更新
style: 代码格式调整
refactor: 重构
test: 测试相关
chore: 构建/工具链相关
```

## 📞 支持与反馈

如有问题或建议，欢迎：
- 提交 Issue
- 发起 Pull Request
- 联系项目维护者

## 📄 许可证

MIT License - 详见 LICENSE 文件

---

**项目状态**: 🟢 开发中

**最后更新**: 2026-04-15
