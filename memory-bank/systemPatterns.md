# BeavFlatten 系统架构模式与技术决策

## 架构概览

### 系统架构模式
**选择**: **JAMstack 架构** (JavaScript + APIs + Markup)
**理由**: 
- 静态站点的高性能和安全性
- 易于部署和维护
- 优秀的 SEO 表现
- 成本效益高 (GitHub Pages 免费托管)

### 架构图

```
┌─────────────────────────────────────────────────────────────┐
│                    用户访问层                                  │
├─────────────────────────────────────────────────────────────┤
│  浏览器 (Chrome/Firefox/Safari/Edge)                        │
│  ├── 桌面端 (1920x1080+)                                    │
│  ├── 平板端 (768x1024)                                      │
│  └── 移动端 (375x667+)                                      │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                     CDN 分发层                               │
├─────────────────────────────────────────────────────────────┤
│  GitHub Pages + Cloudflare (可选)                           │
│  ├── 全球 CDN 节点                                          │
│  ├── 自动 HTTPS                                             │
│  ├── 缓存优化                                               │
│  └── DDoS 防护                                              │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    前端应用层                                │
├─────────────────────────────────────────────────────────────┤
│  主索引页 (index.html)                                       │
│  ├── 多语言切换                                             │
│  ├── SEO 优化                                               │
│  ├── 版本导航                                               │
│  └── 品牌展示                                               │
│                                                             │
│  工具页面 (version-*/index.html)                            │
│  ├── 压缩包处理                                             │
│  ├── 文件扁平化                                             │
│  ├── 用户交互                                               │
│  └── 结果下载                                               │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                   客户端处理层                               │
├─────────────────────────────────────────────────────────────┤
│  JavaScript 核心引擎                                         │
│  ├── JSZip.js (压缩文件处理)                                │
│  ├── FileSaver.js (文件下载)                                │
│  ├── i18n 引擎 (多语言)                                     │
│  └── SEO 管理器                                             │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    数据存储层                                │
├─────────────────────────────────────────────────────────────┤
│  浏览器本地存储                                              │
│  ├── LocalStorage (用户偏好)                                │
│  ├── SessionStorage (临时数据)                              │
│  ├── IndexedDB (大文件缓存)                                 │
│  └── Cookies (语言偏好)                                     │
└─────────────────────────────────────────────────────────────┘
```

## 核心技术栈决策

### 前端框架选择
**决策**: 原生 HTML/CSS/JavaScript
**理由**:
- 无框架依赖，加载速度快
- 更好的 SEO 支持
- 简化部署流程
- 降低维护成本
- 更好的浏览器兼容性

### CSS 框架选择
**决策**: Tailwind CSS
**理由**:
- 原子化 CSS，高度可定制
- 优秀的响应式设计支持
- 内置暗色模式支持
- 小体积，按需加载
- 现代化的设计系统

### 多语言解决方案
**决策**: 自定义 JavaScript i18n 系统
**实现模式**:
```javascript
// 语言配置对象
const i18n = {
  'en': {
    'title': 'BeavFlatten - Zip Flattener Tool',
    'description': 'Flatten ZIP archives easily'
  },
  'zh-CN': {
    'title': '狸压侠 - 压缩包扁平化工具',
    'description': '轻松扁平化压缩包目录结构'
  },
  'zh-TW': {
    'title': '狸壓俠 - 壓縮包扁平化工具',
    'description': '輕鬆扁平化壓縮包目錄結構'
  }
};

// 语言切换函数
function setLanguage(lang) {
  localStorage.setItem('preferred-language', lang);
  document.cookie = `lang=${lang}; path=/; max-age=31536000`;
  updatePageContent(lang);
}
```

## 设计模式与架构模式

### 1. 模块化设计模式
```javascript
// 文件结构模式
const AppModules = {
  // 核心模块
  core: {
    i18n: 'js/core/i18n.js',
    seo: 'js/core/seo.js',
    utils: 'js/core/utils.js'
  },
  
  // 功能模块
  features: {
    zipProcessor: 'js/features/zip-processor.js',
    fileFlattener: 'js/features/file-flattener.js',
    downloadManager: 'js/features/download-manager.js'
  },
  
  // UI 模块
  ui: {
    languageSwitcher: 'js/ui/language-switcher.js',
    progressBar: 'js/ui/progress-bar.js',
    filePreview: 'js/ui/file-preview.js'
  }
};
```

### 2. 观察者模式 (事件驱动)
```javascript
// 事件管理器
class EventManager {
  constructor() {
    this.events = {};
  }
  
  on(event, callback) {
    if (!this.events[event]) {
      this.events[event] = [];
    }
    this.events[event].push(callback);
  }
  
  emit(event, data) {
    if (this.events[event]) {
      this.events[event].forEach(callback => callback(data));
    }
  }
}

// 使用示例
const eventManager = new EventManager();
eventManager.on('languageChanged', updatePageContent);
eventManager.on('fileProcessed', updateProgress);
```

### 3. 策略模式 (多版本支持)
```javascript
// 版本策略模式
const VersionStrategies = {
  'deepseekR1': {
    name: 'DeepSeek R1 版本',
    features: ['基础扁平化', '排除规则', '品牌水印'],
    uiFramework: 'Bootstrap',
    processor: 'basic'
  },
  
  'geminiPro': {
    name: 'Gemini Pro 版本',
    features: ['扁平化', '隐藏文件排除'],
    uiFramework: 'Tailwind',
    processor: 'simplified'
  }
};
```

### 4. 工厂模式 (SEO 管理)
```javascript
// SEO 工厂模式
class SEOFactory {
  static createMetaTags(page, language) {
    const seoConfig = {
      'index': {
        'en': {
          title: 'BeavFlatten - Professional Zip Flattener Tool',
          description: 'Flatten ZIP archives and reorganize nested directories easily. Support for multiple languages and AI search optimization.',
          keywords: 'zip flattener, file organizer, directory flatten, archive tool'
        },
        'zh-CN': {
          title: '狸压侠 - 专业的压缩包扁平化工具',
          description: '轻松扁平化压缩包并重组嵌套目录。支持多语言和AI搜索优化。',
          keywords: '压缩包扁平化, 文件整理, 目录扁平化, 归档工具'
        }
      }
    };
    
    return seoConfig[page][language];
  }
}
```

## SEO 优化架构

### 元数据管理系统
```html
<!-- 基础 SEO 元数据 -->
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title data-i18n="meta.title">BeavFlatten - Zip Flattener Tool</title>
<meta name="description" data-i18n="meta.description" content="Flatten ZIP archives easily">
<meta name="keywords" data-i18n="meta.keywords" content="zip, flattener, tool, archive">
<meta name="author" content="Stark Yuan">

<!-- Open Graph 元数据 -->
<meta property="og:title" data-i18n="meta.title" content="BeavFlatten - Zip Flattener Tool">
<meta property="og:description" data-i18n="meta.description" content="Flatten ZIP archives easily">
<meta property="og:image" content="https://your-domain.com/assets/og-image.png">
<meta property="og:url" content="https://your-domain.com">
<meta property="og:type" content="website">

<!-- Twitter Card 元数据 -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" data-i18n="meta.title" content="BeavFlatten - Zip Flattener Tool">
<meta name="twitter:description" data-i18n="meta.description" content="Flatten ZIP archives easily">
<meta name="twitter:image" content="https://your-domain.com/assets/twitter-image.png">

<!-- 结构化数据 -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "WebApplication",
  "name": "BeavFlatten",
  "description": "Professional ZIP archive flattening tool",
  "url": "https://your-domain.com",
  "author": {
    "@type": "Person",
    "name": "Stark Yuan",
    "url": "https://github.com/yuanyuanyuan"
  },
  "applicationCategory": "Utility",
  "operatingSystem": "Web Browser"
}
</script>
```

### AI 搜索优化 (LLM.txt)
```markdown
# LLM.txt 文件结构
## 项目概述
BeavFlatten 是一个专业的 ZIP 压缩包扁平化工具，支持多语言和国际化。

## 核心功能
- ZIP 文件解压和重组
- 目录结构扁平化
- 文件重命名规则
- 隐藏文件排除
- 多语言支持

## 技术栈
- 前端：HTML5, CSS3, JavaScript ES6+
- 框架：Tailwind CSS
- 库：JSZip.js, FileSaver.js
- 部署：GitHub Pages
- CI/CD：GitHub Actions

## 作者信息
作者：Stark Yuan
GitHub：https://github.com/yuanyuanyuan
专业：前端开发、产品设计、国际化
```

## 性能优化模式

### 1. 懒加载模式
```javascript
// 懒加载实现
class LazyLoader {
  static async loadModule(moduleName) {
    const moduleMap = {
      'zipProcessor': () => import('./modules/zip-processor.js'),
      'fileFlattener': () => import('./modules/file-flattener.js')
    };
    
    if (moduleMap[moduleName]) {
      return await moduleMap[moduleName]();
    }
  }
}
```

### 2. 缓存策略模式
```javascript
// 缓存管理器
class CacheManager {
  static setLanguageCache(language, content) {
    const cacheKey = `i18n-${language}`;
    const cacheData = {
      content,
      timestamp: Date.now(),
      version: '1.0.0'
    };
    localStorage.setItem(cacheKey, JSON.stringify(cacheData));
  }
  
  static getLanguageCache(language) {
    const cacheKey = `i18n-${language}`;
    const cached = localStorage.getItem(cacheKey);
    
    if (cached) {
      const data = JSON.parse(cached);
      // 检查缓存是否过期 (24小时)
      if (Date.now() - data.timestamp < 24 * 60 * 60 * 1000) {
        return data.content;
      }
    }
    return null;
  }
}
```

## 安全性模式

### 1. 输入验证模式
```javascript
// 文件验证器
class FileValidator {
  static validateZipFile(file) {
    // 文件类型验证
    const allowedTypes = ['application/zip', 'application/x-zip-compressed'];
    if (!allowedTypes.includes(file.type)) {
      throw new Error('Invalid file type. Only ZIP files are allowed.');
    }
    
    // 文件大小验证 (最大 500MB)
    const maxSize = 500 * 1024 * 1024;
    if (file.size > maxSize) {
      throw new Error('File too large. Maximum size is 500MB.');
    }
    
    return true;
  }
  
  static sanitizeFileName(fileName) {
    // 移除危险字符
    return fileName.replace(/[<>:"/\\|?*]/g, '_')
                  .replace(/\.\./g, '_')
                  .trim();
  }
}
```

### 2. XSS 防护模式
```javascript
// XSS 防护工具
class SecurityUtils {
  static escapeHtml(text) {
    const div = document.createElement('div');
    div.textContent = text;
    return div.innerHTML;
  }
  
  static sanitizeContent(content) {
    return content.replace(/[<>&"']/g, (match) => {
      const escapeMap = {
        '<': '&lt;',
        '>': '&gt;',
        '&': '&amp;',
        '"': '&quot;',
        "'": '&#x27;'
      };
      return escapeMap[match];
    });
  }
}
```

## 错误处理模式

### 1. 统一错误处理
```javascript
// 错误处理器
class ErrorHandler {
  static handle(error, context = '') {
    console.error(`[${context}] Error:`, error);
    
    // 错误分类和处理
    if (error.name === 'QuotaExceededError') {
      this.showUserMessage('storage_quota_exceeded');
    } else if (error.message.includes('Invalid ZIP')) {
      this.showUserMessage('invalid_zip_file');
    } else {
      this.showUserMessage('general_error');
    }
    
    // 错误上报 (可选)
    this.reportError(error, context);
  }
  
  static showUserMessage(messageKey) {
    const message = i18n.get(messageKey);
    // 显示用户友好的错误信息
    alert(message);
  }
}
```

## 测试策略模式

### 1. 单元测试模式
```javascript
// 测试工具类
class TestUtils {
  static async testZipProcessing() {
    try {
      // 创建测试 ZIP 文件
      const testZip = new JSZip();
      testZip.file("folder/test.txt", "Hello World");
      
      const blob = await testZip.generateAsync({type: "blob"});
      
      // 测试处理功能
      const result = await ZipProcessor.process(blob);
      
      console.assert(result.files.length === 1, "Should process one file");
      console.assert(result.files[0].name === "folder--test.txt", "Should flatten filename");
      
      return true;
    } catch (error) {
      console.error("Test failed:", error);
      return false;
    }
  }
}
```

## 部署模式

### 1. GitHub Actions 工作流
```yaml
# .github/workflows/deploy.yml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]
  pull_request:
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
      
    - name: Run tests
      run: npm test
      
    - name: Build project
      run: npm run build
      
    - name: Deploy to GitHub Pages
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./dist
```

### 2. 版本管理模式
```javascript
// 版本配置
const VERSION_CONFIG = {
  current: '1.0.0',
  buildDate: new Date().toISOString(),
  gitCommit: process.env.GITHUB_SHA || 'development',
  environment: process.env.NODE_ENV || 'development'
};

// 版本信息注入
function injectVersionInfo() {
  const versionElement = document.getElementById('version-info');
  if (versionElement) {
    versionElement.textContent = `v${VERSION_CONFIG.current}`;
  }
  
  console.log(`BeavFlatten v${VERSION_CONFIG.current} - Build: ${VERSION_CONFIG.buildDate}`);
}
```

## 监控与分析模式

### 1. 性能监控
```javascript
// 性能监控器
class PerformanceMonitor {
  static startTiming(label) {
    performance.mark(`${label}-start`);
  }
  
  static endTiming(label) {
    performance.mark(`${label}-end`);
    performance.measure(label, `${label}-start`, `${label}-end`);
    
    const measure = performance.getEntriesByName(label)[0];
    console.log(`${label}: ${measure.duration.toFixed(2)}ms`);
    
    // 上报性能数据
    this.reportPerformance(label, measure.duration);
  }
  
  static reportPerformance(label, duration) {
    // Google Analytics 或其他分析工具
    if (typeof gtag !== 'undefined') {
      gtag('event', 'timing_complete', {
        name: label,
        value: Math.round(duration)
      });
    }
  }
}
```

### 2. 用户行为分析
```javascript
// 行为追踪器
class BehaviorTracker {
  static trackFileUpload(fileSize, fileType) {
    gtag('event', 'file_upload', {
      file_size: fileSize,
      file_type: fileType
    });
  }
  
  static trackLanguageSwitch(fromLang, toLang) {
    gtag('event', 'language_switch', {
      from_language: fromLang,
      to_language: toLang
    });
  }
  
  static trackFeatureUsage(feature) {
    gtag('event', 'feature_usage', {
      feature_name: feature
    });
  }
}
```

## 总结

这个系统架构模式文档定义了 BeavFlatten 项目的核心技术决策和设计模式。通过采用现代化的前端架构模式，我们确保了：

1. **可维护性**: 模块化设计和清晰的代码结构
2. **可扩展性**: 灵活的插件系统和版本策略
3. **性能优化**: 懒加载、缓存和性能监控
4. **安全性**: 输入验证和 XSS 防护
5. **国际化**: 完整的多语言支持架构
6. **SEO 友好**: 结构化数据和 AI 搜索优化
7. **自动化**: CI/CD 流水线和自动部署

这些模式将指导整个项目的开发过程，确保我们构建一个专业、可靠、国际化的技术产品。
