# JSONKit 技术方案

## 技术栈

- **前端**：HTML + CSS + 原生 JS（纯前端，无后端）
- **编辑器**：CodeMirror 6（语法高亮、行号、括号匹配）
- **构建**：MVP 阶段 CDN 引入，正式部署用 Vite 打包
- **主题**：CSS 变量系统，支持亮色/暗色切换

## 架构设计

```
┌─────────────────────────────────┐
│           UI Layer              │
│  Header + Sidebar + 双栏布局    │
├─────────────────────────────────┤
│        Editor Layer             │
│    CodeMirror 6 (JSON mode)     │
├─────────────────────────────────┤
│       Processing Layer          │
│    Web Worker (独立线程)         │
│  - 格式化 / 校验 / Diff / 树形  │
├─────────────────────────────────┤
│        Display Layer            │
│  树形视图 (虚拟滚动 - 二期)      │
└─────────────────────────────────┘
```

## 关键技术决策

### 1. CodeMirror 6 (非 Monaco)
- 打包体积小（几百KB vs Monaco 2MB+）
- 移动端友好
- JSON 工具场景不需要 VS Code 级功能

### 2. Web Worker
- 格式化、校验、diff 全部放 Worker，主线程不阻塞
- Worker 独立文件 `json-worker.js`，加时间戳防缓存
- Worker Manager 用 Promise + id 管理异步调用

### 3. 功能模块懒加载
- 按工具拆分 chunk，首屏只加载当前工具
- 正式部署用 Vite code splitting

### 4. CSS 变量主题系统
- Design Token → CSS 变量，亮色/暗色一套变量名不同值
- localStorage 持久化用户偏好

## 性能要求

| 指标 | 目标 |
|------|------|
| 首屏加载 | < 2s (4G) |
| 1MB 格式化 | < 500ms |
| 10MB 格式化 | < 3s |
| 文件上传上限 | 50MB |

## 二期技术规划

1. **虚拟滚动** — 树形视图 10000+ 节点性能优化
2. **Vite 打包** — 替换 CDN，code splitting + tree shaking
3. **SEO** — 每个工具独立可索引页面，SSG 或多页面
4. **JSON 转换** — JSON ↔ YAML / XML / CSV / TypeScript
5. **JSONPath 查询**
6. **Schema 校验** — ajv
