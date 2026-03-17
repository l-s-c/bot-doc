# 侧边栏 V4 需求文档

> 作者：PM | 日期：2026-03-17 | 版本：V4

---

## 背景

DevKit 已有 141 个工具，需要广告展示位为后续 AdSense 变现做准备。老板本地有 V3 原型（`sidebar-ad-v3.html`），经团队讨论后确定 V4 方案。

## 核心目标

**广告展示位** — 在工具页右侧常驻广告位，为 AdSense 接入预留位置。

## V4 方案（团队评审通过）

### 布局方案

采用 **flexbox 流式布局**，内容区和右栏同级并排：

```
.page-layout { display: flex; justify-content: center; gap: 24px; }
.main-content { max-width: 960px; flex: 1; }
.right-sidebar { width: 280px; position: sticky; top: 80px; }
```

- **≥1380px**：内容区 960px + 间距 24px + 右栏 280px，自然并排
- **<1380px**：右栏 `display:none`，内容区恢复 `margin: 0 auto` 居中
- 使用 `sticky` 定位（非 `fixed`），跟随滚动，不需手动算偏移

### 注入方式

`sidebar.js` 动态注入，**零侵入** — 不修改任何现有工具页 HTML。

页面末尾 `<script src="/shared/sidebar.js"></script>` 引入即可。

### 右栏内容

1. **操作按钮**（横排）：分享 / ❤️收藏 / 复制链接 / 反馈
2. **广告位 ×2**：300×250 标准尺寸（AdSense 标准矩形）
3. **⭐ 热门工具推荐**：4 个工具入口（Base64/时间戳/颜色转换/Hash）带图标+描述

### 响应式

| 屏幕宽度 | 右栏 | 内容区 |
|---|---|---|
| ≥1380px | 显示 280px 右栏 | 960px 左偏 |
| <1380px | 隐藏 | 960px 居中 |

### 暗色模式

全部使用 CSS 变量（`var(--bg-glass)` 等），自动适配。

---

## 排除规则

以下页面**不加侧边栏**：

| 页面 | 原因 |
|---|---|
| 首页 `index.html` | 首页有自己的布局 |
| `json/` | 全屏 SPA（老板明确要求排除） |
| `base64/` | 全屏双栏编解码 |
| `code-formatter/` | CodeMirror 全屏编辑器 |
| `sql-formatter/` | CodeMirror 全屏编辑器 |
| `url-codec/` | 全屏双栏编解码 |
| `yaml/` | 全屏双栏编辑器 |

**排除 7 个，侧边栏覆盖 134 个工具页。**

排除判断方式：`sidebar.js` 内置排除名单数组，匹配 `location.pathname` 跳过注入。

---

## 技术要点（Architect 确认）

1. **flex 布局优于 fixed 定位** — 自然流式排列，零遮挡
2. **sticky 优于 fixed** — 跟随滚动自然，不需要手动算偏移量
3. **容器 class 名不统一问题** — V4 方案往 `body` 追加 `<aside>`，不依赖现有容器 class，零侵入
4. **广告位尺寸** — 300×250 矩形（IAB 标准，AdSense 支持最好的尺寸之一）

---

## 验收标准

1. ≥1380px 右栏常驻显示，广告位可见
2. <1380px 右栏隐藏，内容区居中，工具功能不受影响
3. 134 个工具页正常注入
4. 7 个排除页面不注入
5. 暗色模式渲染正常
6. 热门工具推荐链接正确
7. 收藏功能 localStorage 正常读写
8. 零 JS 报错

---

## SEO 影响

无负面影响。侧边栏为 JS 动态注入，搜索引擎爬虫看到的仍是原始 HTML。

---

## 团队评审记录

- **Designer** ✅ — V4 设计稿 `sidebar-v4.html` 已出，flex+sticky 方案
- **Architect** ✅ — 技术可行，sticky 优于 fixed，flex 优于 margin 硬编码
- **Developer** ✅ — 半天可完成，排除名单 6 个全屏工具 + 首页
- **QA** ✅ — 关注断点隐藏/主内容区宽度/广告空态/链接路径/暗色模式
- **Business Lead** ✅ — 300×250 矩形 CTR 适中，大屏用户高价值
- **老板** — 核心需求是广告展示位；全屏工具（如 JSON）不加侧边栏
