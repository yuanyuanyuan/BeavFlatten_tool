# BeavFlatten 技术上下文与技术栈

## 技术栈概览

### 核心技术选型

#### 前端技术栈
```yaml
核心语言:
  - HTML5: 语义化标记和结构
  - CSS3: 现代样式和动画
  - JavaScript ES6+: 核心业务逻辑

样式框架:
  - Tailwind CSS 3.x: 原子化 CSS 框架
  - 自定义 CSS: 品牌特定样式

JavaScript 库:
  - JSZip 3.10.1+: ZIP 文件处理
  - FileSaver.js 2.0.5+: 文件下载
  - 自定义 i18n: 多语言支持
```

#### 构建与部署
```yaml
版本控制:
  - Git: 代码版本管理
  - GitHub: 代码托管和协作

CI/CD:
  - GitHub Actions: 自动化构建和部署
  - GitHub Pages: 静态站点托管

开发工具:
  - VS Code: 主要开发环境
  - Live Server: 本地开发服务器
  - Chrome DevTools: 调试和性能分析
```

#### 监控与分析
```yaml
性能监控:
  - Google PageSpeed Insights: 性能评测
  - Lighthouse: 综合质量评估
  - Web Vitals: 用户体验指标

用户分析:
  - Google Analytics 4: 用户行为分析
  - Google Search Console: SEO 监控
  - Google Tag Manager: 标签管理
```

## 技术依赖关系

### 外部 CDN 依赖
```javascript
// Tailwind CSS
"https://cdn.tailwindcss.com"

// JSZip
"https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"

// FileSaver.js
"https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js"

// Font Awesome (可选)
"https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"

// Google Fonts
"https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap"
```

### 浏览器兼容性要求
```yaml
支持的浏览器:
  Chrome: 90+
  Firefox: 88+
  Safari: 14+
  Edge: 90+
  
关键 API 支持:
  - File API: 文件读取
  - Blob API: 二进制数据处理
  - Promise/async-await: 异步处理
  - ES6 Modules: 模块化
  - LocalStorage: 本地存储
  - Fetch API: 网络请求
```

### 性能要求
```yaml
加载性能:
  - 首次加载: < 3 秒
  - 后续加载: < 1 秒
  - 文件处理: < 5 秒 (100MB 文件)

资源优化:
  - HTML 压缩: 启用
  - CSS 压缩: 启用
  - JavaScript 压缩: 启用
  - 图片优化: WebP 格式
  - 缓存策略: 长期缓存
```

## 开发环境配置

### 本地开发环境
```bash
# 必需软件
Node.js: 18.x LTS
Git: 2.40+
VS Code: 最新版本

# VS Code 扩展
- Live Server
- Tailwind CSS IntelliSense
- ES6 String HTML
- Auto Rename Tag
- Prettier
- ESLint
```

### 项目结构
```
BeavFlatten_tool/
├── index.html                 # 主索引页
├── assets/                    # 静态资源
│   ├── images/               # 图片资源
│   ├── icons/                # 图标文件
│   └── og-images/            # 社交分享图片
├── js/                       # JavaScript 文件
│   ├── core/                 # 核心模块
│   │   ├── i18n.js          # 多语言支持
│   │   ├── seo.js           # SEO 管理
│   │   └── utils.js         # 工具函数
│   ├── features/             # 功能模块
│   │   ├── zip-processor.js  # ZIP 处理
│   │   └── file-flattener.js # 文件扁平化
│   └── ui/                   # UI 组件
│       ├── language-switcher.js
│       └── progress-bar.js
├── css/                      # 自定义样式
│   ├── main.css             # 主样式文件
│   └── components.css       # 组件样式
├── version-deepseekR1/       # DeepSeek R1 版本
│   └── index.html
├── version-GeminiPro/        # Gemini Pro 版本
│   └── index.html
├── locales/                  # 多语言文件
│   ├── en.json              # 英文
│   ├── zh-CN.json           # 简体中文
│   └── zh-TW.json           # 繁体中文
├── .github/                  # GitHub 配置
│   └── workflows/
│       └── deploy.yml       # 部署工作流
├── docs/                     # 文档
├── LLM.txt                   # AI 搜索优化
├── sitemap.xml              # 站点地图
├── robots.txt               # 爬虫规则
├── package.json             # 项目配置
└── README.md                # 项目说明
```

## 技术实现方案

### 多语言国际化方案
```javascript
// i18n 核心实现
class I18nManager {
  constructor() {
    this.currentLanguage = this.detectLanguage();
    this.translations = {};
    this.loadLanguageData();
  }
  
  detectLanguage() {
    // 1. URL 参数
    const urlParams = new URLSearchParams(window.location.search);
    const urlLang = urlParams.get('lang');
    if (urlLang && this.isValidLanguage(urlLang)) {
      return urlLang;
    }
    
    // 2. Cookie
    const cookieLang = this.getCookie('lang');
    if (cookieLang && this.isValidLanguage(cookieLang)) {
      return cookieLang;
    }
    
    // 3. LocalStorage
    const storedLang = localStorage.getItem('preferred-language');
    if (storedLang && this.isValidLanguage(storedLang)) {
      return storedLang;
    }
    
    // 4. 浏览器语言
    const browserLang = navigator.language || navigator.userLanguage;
    const shortLang = browserLang.split('-')[0];
    if (this.isValidLanguage(shortLang)) {
      return shortLang;
    }
    
    // 5. 默认语言
    return 'en';
  }
  
  async loadLanguageData() {
    try {
      const response = await fetch(`/locales/${this.currentLanguage}.json`);
      this.translations = await response.json();
      this.updatePageContent();
    } catch (error) {
      console.error('Failed to load language data:', error);
      // 回退到默认语言
      if (this.currentLanguage !== 'en') {
        this.currentLanguage = 'en';
        await this.loadLanguageData();
      }
    }
  }
}
```

### SEO 优化方案
```javascript
// SEO 管理器
class SEOManager {
  constructor() {
    this.metaTags = {
      title: document.querySelector('title'),
      description: document.querySelector('meta[name="description"]'),
      keywords: document.querySelector('meta[name="keywords"]'),
      ogTitle: document.querySelector('meta[property="og:title"]'),
      ogDescription: document.querySelector('meta[property="og:description"]'),
      twitterTitle: document.querySelector('meta[name="twitter:title"]'),
      twitterDescription: document.querySelector('meta[name="twitter:description"]')
    };
  }
  
  updateMetaTags(language, page = 'index') {
    const seoData = this.getSEOData(page, language);
    
    // 更新基础元标签
    this.metaTags.title.textContent = seoData.title;
    this.metaTags.description.setAttribute('content', seoData.description);
    this.metaTags.keywords.setAttribute('content', seoData.keywords);
    
    // 更新 Open Graph 标签
    this.metaTags.ogTitle.setAttribute('content', seoData.title);
    this.metaTags.ogDescription.setAttribute('content', seoData.description);
    
    // 更新 Twitter Card 标签
    this.metaTags.twitterTitle.setAttribute('content', seoData.title);
    this.metaTags.twitterDescription.setAttribute('content', seoData.description);
    
    // 更新 HTML lang 属性
    document.documentElement.lang = language;
    
    // 更新结构化数据
    this.updateStructuredData(seoData, language);
  }
  
  getSEOData(page, language) {
    const seoConfig = {
      'index': {
        'en': {
          title: 'BeavFlatten - Professional ZIP Flattener Tool | Stark Yuan',
          description: 'Professional ZIP archive flattening tool with multi-language support. Easily reorganize nested directories and flatten file structures. Created by Stark Yuan.',
          keywords: 'zip flattener, archive tool, file organizer, directory flatten, nested folders, file management, stark yuan'
        },
        'zh-CN': {
          title: '狸压侠 - 专业的压缩包扁平化工具 | Stark Yuan',
          description: '专业的ZIP压缩包扁平化工具，支持多语言。轻松重组嵌套目录并扁平化文件结构。由 Stark Yuan 创建。',
          keywords: '压缩包扁平化, 归档工具, 文件整理, 目录扁平化, 嵌套文件夹, 文件管理, stark yuan'
        },
        'zh-TW': {
          title: '狸壓俠 - 專業的壓縮包扁平化工具 | Stark Yuan',
          description: '專業的ZIP壓縮包扁平化工具，支援多語言。輕鬆重組嵌套目錄並扁平化檔案結構。由 Stark Yuan 創建。',
          keywords: '壓縮包扁平化, 歸檔工具, 檔案整理, 目錄扁平化, 嵌套資料夾, 檔案管理, stark yuan'
        }
      }
    };
    
    return seoConfig[page][language];
  }
}
```

### 性能优化方案
```javascript
// 性能优化管理器
class PerformanceOptimizer {
  constructor() {
    this.initLazyLoading();
    this.initResourcePreloading();
    this.initCaching();
  }
  
  initLazyLoading() {
    // 图片懒加载
    const images = document.querySelectorAll('img[data-src]');
    const imageObserver = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          const img = entry.target;
          img.src = img.dataset.src;
          img.removeAttribute('data-src');
          imageObserver.unobserve(img);
        }
      });
    });
    
    images.forEach(img => imageObserver.observe(img));
  }
  
  initResourcePreloading() {
    // 预加载关键资源
    const criticalResources = [
      '/js/core/i18n.js',
      '/locales/en.json',
      '/css/main.css'
    ];
    
    criticalResources.forEach(resource => {
      const link = document.createElement('link');
      link.rel = 'preload';
      link.href = resource;
      link.as = resource.endsWith('.js') ? 'script' : 
               resource.endsWith('.css') ? 'style' : 'fetch';
      document.head.appendChild(link);
    });
  }
  
  initCaching() {
    // 实现 Service Worker 缓存策略
    if ('serviceWorker' in navigator) {
      navigator.serviceWorker.register('/sw.js')
        .then(registration => {
          console.log('SW registered:', registration);
        })
        .catch(error => {
          console.log('SW registration failed:', error);
        });
    }
  }
}
```

## 部署配置

### GitHub Actions 工作流
```yaml
name: Deploy BeavFlatten to GitHub Pages

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

env:
  NODE_VERSION: '18'

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout
      uses: actions/checkout@v4
      
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: ${{ env.NODE_VERSION }}
        cache: 'npm'
        
    - name: Install dependencies
      run: npm ci
      
    - name: Run linting
      run: npm run lint
      
    - name: Run tests
      run: npm test
      
    - name: Build project
      run: npm run build
      env:
        NODE_ENV: production
        
    - name: Optimize assets
      run: |
        npm run optimize:images
        npm run optimize:css
        npm run optimize:js
        
    - name: Generate sitemap
      run: npm run generate:sitemap
      
    - name: Validate HTML
      run: npm run validate:html
      
    - name: Deploy to GitHub Pages
      if: github.ref == 'refs/heads/main'
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./dist
        cname: your-custom-domain.com
        
    - name: Notify deployment
      if: success()
      run: |
        echo "✅ Deployment successful!"
        echo "🌐 Site URL: https://your-username.github.io/BeavFlatten_tool"
```

### package.json 配置
```json
{
  "name": "beavflatten-tool",
  "version": "1.0.0",
  "description": "Professional ZIP archive flattening tool with internationalization",
  "main": "index.html",
  "scripts": {
    "dev": "live-server --port=3000 --open=/",
    "build": "npm run build:css && npm run build:js && npm run build:html",
    "build:css": "tailwindcss -i ./src/css/main.css -o ./dist/css/main.css --minify",
    "build:js": "terser js/**/*.js -o dist/js/main.min.js --compress --mangle",
    "build:html": "html-minifier --input-dir ./ --output-dir ./dist --file-ext html",
    "optimize:images": "imagemin assets/images/* --out-dir=dist/assets/images",
    "optimize:css": "cssnano dist/css/main.css dist/css/main.min.css",
    "optimize:js": "uglifyjs dist/js/main.js -o dist/js/main.min.js",
    "generate:sitemap": "node scripts/generate-sitemap.js",
    "validate:html": "html-validate dist/**/*.html",
    "lint": "eslint js/**/*.js",
    "lint:fix": "eslint js/**/*.js --fix",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "serve": "http-server dist -p 8080 -c-1",
    "analyze": "bundlesize"
  },
  "devDependencies": {
    "tailwindcss": "^3.3.0",
    "terser": "^5.19.0",
    "html-minifier": "^4.0.0",
    "imagemin": "^8.0.1",
    "cssnano": "^6.0.0",
    "uglify-js": "^3.17.0",
    "html-validate": "^8.0.0",
    "eslint": "^8.45.0",
    "jest": "^29.6.0",
    "http-server": "^14.1.0",
    "bundlesize": "^0.18.0",
    "live-server": "^1.2.2"
  },
  "bundlesize": [
    {
      "path": "./dist/css/main.min.css",
      "maxSize": "50 kB"
    },
    {
      "path": "./dist/js/main.min.js",
      "maxSize": "100 kB"
    }
  ],
  "keywords": [
    "zip",
    "flattener",
    "archive",
    "tool",
    "internationalization",
    "i18n",
    "file-management"
  ],
  "author": "Stark Yuan",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/yuanyuanyuan/BeavFlatten_tool"
  },
  "homepage": "https://yuanyuanyuan.github.io/BeavFlatten_tool"
}
```

## 监控与分析配置

### Google Analytics 4 配置
```javascript
// GA4 配置
const GA_MEASUREMENT_ID = 'G-XXXXXXXXXX';

// 初始化 GA4
window.dataLayer = window.dataLayer || [];
function gtag(){dataLayer.push(arguments);}
gtag('js', new Date());
gtag('config', GA_MEASUREMENT_ID, {
  page_title: document.title,
  page_location: window.location.href,
  language: document.documentElement.lang
});

// 自定义事件追踪
class AnalyticsTracker {
  static trackFileUpload(fileSize, fileName) {
    gtag('event', 'file_upload', {
      event_category: 'engagement',
      event_label: fileName,
      value: Math.round(fileSize / 1024) // KB
    });
  }
  
  static trackLanguageSwitch(fromLang, toLang) {
    gtag('event', 'language_switch', {
      event_category: 'user_preference',
      from_language: fromLang,
      to_language: toLang
    });
  }
  
  static trackToolUsage(feature) {
    gtag('event', 'tool_usage', {
      event_category: 'feature',
      event_label: feature
    });
  }
  
  static trackPerformance(metric, value) {
    gtag('event', 'performance_metric', {
      event_category: 'performance',
      event_label: metric,
      value: Math.round(value)
    });
  }
}
```

### 错误监控配置
```javascript
// 错误监控和上报
class ErrorMonitor {
  constructor() {
    this.setupGlobalErrorHandling();
    this.setupUnhandledPromiseRejection();
  }
  
  setupGlobalErrorHandling() {
    window.addEventListener('error', (event) => {
      this.reportError({
        type: 'javascript_error',
        message: event.message,
        filename: event.filename,
        lineno: event.lineno,
        colno: event.colno,
        stack: event.error ? event.error.stack : null
      });
    });
  }
  
  setupUnhandledPromiseRejection() {
    window.addEventListener('unhandledrejection', (event) => {
      this.reportError({
        type: 'promise_rejection',
        message: event.reason.message || event.reason,
        stack: event.reason.stack
      });
    });
  }
  
  reportError(errorInfo) {
    // 发送到 Google Analytics
    gtag('event', 'exception', {
      description: errorInfo.message,
      fatal: false
    });
    
    // 发送到自定义错误收集服务（可选）
    console.error('Error reported:', errorInfo);
  }
}
```

## 安全配置

### Content Security Policy
```html
<meta http-equiv="Content-Security-Policy" content="
  default-src 'self';
  script-src 'self' 'unsafe-inline' 
    https://cdn.tailwindcss.com 
    https://cdnjs.cloudflare.com 
    https://www.googletagmanager.com;
  style-src 'self' 'unsafe-inline' 
    https://cdn.tailwindcss.com 
    https://cdnjs.cloudflare.com 
    https://fonts.googleapis.com;
  font-src 'self' 
    https://fonts.gstatic.com 
    https://cdnjs.cloudflare.com;
  img-src 'self' data: 
    https://www.google-analytics.com;
  connect-src 'self' 
    https://www.google-analytics.com;
">
```

### 安全头配置
```html
<!-- 安全相关的 meta 标签 -->
<meta http-equiv="X-Content-Type-Options" content="nosniff">
<meta http-equiv="X-Frame-Options" content="DENY">
<meta http-equiv="X-XSS-Protection" content="1; mode=block">
<meta http-equiv="Referrer-Policy" content="strict-origin-when-cross-origin">
<meta http-equiv="Permissions-Policy" content="camera=(), microphone=(), geolocation=()">
```

## 技术债务与改进计划

### 当前技术限制
```yaml
已知限制:
  - 静态站点无法进行服务端渲染
  - 依赖第三方 CDN 的可用性
  - 浏览器兼容性限制某些新特性
  - 大文件处理的内存限制

改进计划:
  - 考虑 PWA 实现离线功能
  - 实现 WebAssembly 优化性能
  - 添加 Web Workers 处理大文件
  - 集成更先进的错误监控
```

### 技术升级路线图
```yaml
短期 (1-3 个月):
  - 实现 Service Worker 缓存
  - 添加 PWA 支持
  - 优化移动端体验
  - 完善错误处理

中期 (3-6 个月):
  - 集成 WebAssembly
  - 实现 Web Workers
  - 添加高级分析功能
  - 支持更多文件格式

长期 (6-12 个月):
  - 考虑框架迁移
  - 实现微前端架构
  - 添加 AI 功能
  - 构建插件生态
```

这个技术上下文文档为 BeavFlatten 项目提供了完整的技术实施指南，确保项目能够按照既定的技术标准和最佳实践进行开发。
