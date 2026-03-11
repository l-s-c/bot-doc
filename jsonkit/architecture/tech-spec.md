# DevKit 技术架构文档

## 概述
DevKit 是一个纯前端的在线开发者工具箱，包含 14 个工具，部署在 GitHub Pages。

## 技术栈
- **构建**: Vite (MPA 多入口，16 个 rollup input)
- **编辑器**: CodeMirror 6 (CDN 加载，textarea 降级)
- **样式**: 原生 CSS（液态玻璃设计系统）
- **运行时**: 纯浏览器端，Web Worker 处理重计算
- **部署**: GitHub Pages + GitHub Actions CI/CD
- **统计**: 百度统计

## 架构

### MPA + SPA 混合
- **首页** (`pages/index.html`): 16 个工具卡片入口
- **JSON SPA** (`pages/json/index.html`): 4 个工具共享编辑器（hash 路由 `#format/#tree/#convert/#diff`）
- **13 个独立工具页**: 各自独立 HTML (`pages/{tool}/index.html`)
- **根跳转** (`index.html`): 相对路径重定向到首页

### 模块结构 (JSON SPA)
```
app.js (229行) — 入口 + 路由 + 共用 UI
├── pages/json/modules/format.js (171行) — 格式化 + 校验
├── pages/json/modules/tree.js (473行) — 树形视图 + JSONPath
├── pages/json/modules/convert.js (104行) — 8 种格式转换
└── pages/json/modules/diff.js (610行) — 语义化对比
```

### 共享模块
- `shared/liquid-glass.css` — 设计系统
- `shared/theme.js` — 主题切换 + toast + copyText
- `worker-manager.js` — Web Worker 管理
- `converters.js` — JSON 转换器（YAML/XML/CSV/TS/Go/Java/Python/Schema）
- `cm-setup.js` — CodeMirror 6 初始化

### 关键设计决策
1. **CM6 而非 Monaco**: 体积 200KB vs 2MB，移动端友好
2. **Web Worker**: JSON 解析/格式化在 Worker 线程，主线程不卡
3. **Hash 路由**: JSON SPA 用 `#` 路由，GitHub Pages 静态托管不支持 history API fallback
4. **CDN 降级**: CM6 加载失败自动降级为原生 textarea

### SEO
- 15 个页面各有 title/description/canonical/OG/Twitter Card/JSON-LD/H1/noscript
- sitemap.xml (28 URLs) + robots.txt
- `<noscript>` 内容关键（百度不执行 JS）

### 部署
- GitHub Actions: push main → `npm ci` → `vite build --base /devkit/` → deploy-pages
- `base: '/devkit/'` 配置统一处理子路径

## 仓库
- 代码: https://github.com/l-s-c/devkit
- 文档: https://github.com/l-s-c/bot-doc
- 线上: https://l-s-c.github.io/devkit/
