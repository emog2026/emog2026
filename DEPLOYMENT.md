# 部署指南

## 🚀 部署选项

### 1. Vercel (推荐)

#### 自动部署

1. 将代码推送到 GitHub
2. 在 [vercel.com](https://vercel.com) 导入项目
3. Vercel 会自动检测 Vite 配置并部署

#### 手动部署

```bash
# 安装 Vercel CLI
npm i -g vercel

# 登录
vercel login

# 部署
vercel --prod
```

**环境变量设置**:
在 Vercel Dashboard 设置:
```
VITE_API_BASE_URL=https://your-api.com/api
```

### 2. Netlify

#### 手动部署

```bash
# 安装 Netlify CLI
npm i -g netlify-cli

# 登录
netlify login

# 构建并部署
netlify deploy --prod
```

**netlify.toml 配置**:

```toml
[build]
  command = "npm run build"
  publish = "dist"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

### 3. Nginx

#### 构建项目

```bash
npm run build
```

#### Nginx 配置

```nginx
server {
    listen 80;
    server_name your-domain.com;
    root /path/to/dist;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /api {
        proxy_pass http://your-backend-api;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    # 缓存静态资源
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

### 4. Docker

#### Dockerfile

```dockerfile
# 构建阶段
FROM node:18-alpine as build-stage

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

# 生产阶段
FROM nginx:stable-alpine as production-stage

COPY --from=build-stage /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

#### 构建和运行

```bash
# 构建镜像
docker build -t abab-tools .

# 运行容器
docker run -p 80:80 abab-tools
```

### 5. 传统服务器

#### 1. 构建项目

```bash
npm run build
```

#### 2. 上传文件

将 `dist/` 目录上传到服务器:

```bash
scp -r dist/* user@server:/var/www/abab-tools/
```

#### 3. 配置 Web 服务器

**Apache 配置** (`.htaccess`):

```apache
RewriteEngine On
RewriteBase /
RewriteRule ^index\.html$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.html [L]
```

**Nginx 配置** (参考上面的 Nginx 部分)

## 🔧 构建优化

### 代码分割

已通过路由懒加载自动实现:

```javascript
component: () => import('../pages/ToolDetail.vue')
```

### 压缩资源

`vite.config.js` 已配置:

```javascript
build: {
  minify: 'terser',
  terserOptions: {
    compress: {
      drop_console: true,
      drop_debugger: true
    }
  }
}
```

### CDN 加速

修改 `vite.config.js`:

```javascript
build: {
  rollupOptions: {
    output: {
      manualChunks: {
        'vendor': ['vue', 'vue-router', 'pinia'],
        'utils': ['axios', 'dayjs']
      }
    }
  }
}
```

## 🌍 环境配置

### 开发环境

`.env.development`:

```env
VITE_API_BASE_URL=http://localhost:3000/api
```

### 生产环境

`.env.production`:

```env
VITE_API_BASE_URL=https://api.your-domain.com/api
```

## 🔒 HTTPS 配置

### Let's Encrypt (免费)

#### 使用 Certbot

```bash
# 安装 Certbot
sudo apt-get install certbot python3-certbot-nginx

# 获取证书
sudo certbot --nginx -d your-domain.com

# 自动续期
sudo certbot renew --dry-run
```

#### Nginx 配置

```nginx
server {
    listen 443 ssl http2;
    server_name your-domain.com;

    ssl_certificate /etc/letsencrypt/live/your-domain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/your-domain.com/privkey.pem;

    # SSL 优化
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    # ... 其他配置
}

# HTTP 重定向到 HTTPS
server {
    listen 80;
    server_name your-domain.com;
    return 301 https://$server_name$request_uri;
}
```

## 📊 性能监控

### 添加 Google Analytics

在 `index.html` 中添加:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

### 添加 Sentry 错误监控

```bash
npm install @sentry/vue
```

`src/main.js`:

```javascript
import * as Sentry from "@sentry/vue";

Sentry.init({
  app,
  dsn: "your-sentry-dsn",
  environment: import.meta.env.MODE,
});
```

## 🔄 CI/CD

### GitHub Actions

创建 `.github/workflows/deploy.yml`:

```yaml
name: Deploy to Vercel

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.ORG_ID }}
          vercel-project-id: ${{ secrets.PROJECT_ID }}
          working-directory: ./
```

## 🎯 部署后检查清单

- [ ] 所有链接正常工作
- [ ] API 连接正常
- [ ] 环境变量正确设置
- [ ] HTTPS 证书有效
- [ ] 静态资源正确加载
- [ ] 移动端显示正常
- [ ] 性能测试通过
- [ ] SEO 元数据正确
- [ ] 错误监控已启用
- [ ] 备份策略已设置

## 🚨 常见部署问题

### 路由刷新 404

确保服务器配置了 SPA fallback:

**Nginx**:
```nginx
try_files $uri $uri/ /index.html;
```

**Apache**:
```apache
FallbackResource /index.html
```

### API 跨域

配置 CORS:

```javascript
// vite.config.js
server: {
  proxy: {
    '/api': {
      target: 'http://backend-api.com',
      changeOrigin: true,
      rewrite: (path) => path.replace(/^\/api/, '')
    }
  }
}
```

### 静态资源 404

检查 `base` 路径配置:

```javascript
// vite.config.js
export default defineConfig({
  base: '/your-subdirectory/',
  // ...
})
```

---

部署完成后，建议使用 [Lighthouse](https://developers.google.com/web/tools/lighthouse) 进行性能测试。
