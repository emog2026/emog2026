# GitHub Pages 部署指南

## 🚀 自动部署

本项目已配置 GitHub Actions 自动部署，每次推送到 `main` 分支时会自动构建和部署。

## 📋 部署状态

查看 GitHub Actions 工作流运行状态：
https://github.com/emog2026/emog2026/actions

## ⚙️ 配置 GitHub Pages

### 步骤 1: 启用 GitHub Pages

1. 访问仓库设置页面：
   https://github.com/emog2026/emog2026/settings/pages

2. 在 "Build and deployment" 部分：
   - **Source**: 选择 `GitHub Actions`
   - 点击 **Save**

3. 等待 GitHub Actions 工作流完成（通常需要 2-5 分钟）

### 步骤 2: 查看部署状态

1. 访问 Actions 页面：
   https://github.com/emog2026/emog2026/actions

2. 查看最新的工作流运行状态
   - 🟢 成功 - 网站已部署
   - 🔴 失败 - 查看日志排查问题

### 步骤 3: 访问网站

部署成功后，你的网站将在以下地址运行：

**https://emog2026.github.io/emog2026/**

## 🔄 触发自动部署

### 方式 1: 推送代码（推荐）

```bash
# 修改代码后
git add .
git commit -m "your changes"
git push

# 自动触发部署
```

### 方式 2: 手动触发

1. 访问 Actions 页面：
   https://github.com/emog2026/emog2026/actions

2. 选择 `Deploy to GitHub Pages` 工作流

3. 点击 **Run workflow** → **Run workflow**

## 📊 部署配置说明

### GitHub Actions 工作流

**文件位置**: `.github/workflows/deploy.yml`

**触发条件**:
- 推送到 `main` 分支
- 手动触发（workflow_dispatch）

**部署步骤**:
1. 检出代码
2. 设置 Node.js 环境
3. 安装依赖（npm ci）
4. 构建项目（npm run build）
5. 部署到 GitHub Pages

### Vite 配置

**文件位置**: `vite.config.js`

```javascript
export default defineConfig({
  base: '/emog2026/',  // GitHub Pages 子路径
  // ... 其他配置
})
```

## 🔧 故障排除

### 问题 1: 部署失败 - 404 Not Found

**原因**: GitHub Pages 设置未正确配置

**解决方案**:
1. 访问 https://github.com/emog2026/emog2026/settings/pages
2. 确保 Source 设置为 `GitHub Actions`
3. 等待 Actions 工作流完成

### 问题 2: 网站空白或资源加载失败

**原因**: Vite base 路径配置错误

**解决方案**:
1. 检查 `vite.config.js` 中的 `base` 配置
2. 确保 `base: '/emog2026/'`
3. 重新提交代码触发部署

### 问题 3: API 请求失败

**原因**: GitHub API token 未配置

**解决方案**:
1. 在 `.env` 文件中配置 `VITE_GITHUB_TOKEN`
2. 确保 token 有正确的权限
3. 注意：GitHub Pages 是静态托管，不支持环境变量

**重要提示**:
- GitHub Pages 是静态托管，不支持 `.env` 文件
- 在 GitHub Pages 上，API 请求可能会失败（CORS 限制）
- 建议在生产环境中使用其他后端服务

### 问题 4: Actions 工作流失败

**常见原因**:
- Node.js 版本不兼容
- 依赖安装失败
- 构建错误

**解决方案**:
1. 查看 Actions 日志详情
2. 检查 `package.json` 中的依赖版本
3. 本地测试 `npm run build` 是否成功

## 🚀 高级配置

### 自定义域名

1. 访问 https://github.com/emog2026/emog2026/settings/pages

2. 在 "Custom domain" 部分：
   - 输入你的域名（如：`www.yourdomain.com`）
   - 点击 **Save**

3. 配置 DNS：
   - 添加 CNAME 记录：`www.yourdomain.com` → `emog2026.github.io`

### 环境变量（GitHub Actions）

如果需要在 GitHub Actions 中使用环境变量：

1. 访问 https://github.com/emog2026/emog2026/settings/secrets/actions

2. 点击 **New repository secret**

3. 添加密钥：
   - Name: `VITE_GITHUB_TOKEN`
   - Value: 你的 GitHub token

4. 在工作流中使用：
   ```yaml
   - name: Build
     env:
       VITE_GITHUB_TOKEN: ${{ secrets.VITE_GITHUB_TOKEN }}
     run: npm run build
   ```

### 性能优化

#### 1. 启用 gzip 压缩

GitHub Pages 默认启用 gzip 压缩。

#### 2. 使用 CDN

可以配置自定义域名并使用 CDN 加速。

#### 3. 图片优化

- 使用 WebP 格式
- 压缩图片大小
- 使用懒加载

## 📚 相关资源

- [GitHub Pages 文档](https://docs.github.com/en/pages)
- [GitHub Actions 文档](https://docs.github.com/en/actions)
- [Vite 部署指南](https://vitejs.dev/guide/static-deploy.html#github-pages)
- [自定义域名配置](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)

## 🎯 部署清单

- [ ] GitHub Pages 已启用
- [ ] Actions 工作流成功运行
- [ ] 网站可以访问
- [ ] 所有资源正确加载
- [ ] API 请求正常工作
- [ ] 移动端显示正常
- [ ] 搜索和筛选功能正常

---

**🎉 恭喜！你的网站已部署到 GitHub Pages！**

访问地址：**https://emog2026.github.io/emog2026/**

如有问题，查看 Actions 日志或提交 Issue。
