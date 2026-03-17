# DevKit 设计规范 v1.0

> 所有工具页面必须遵循此规范。已有不符合的页面列入修复清单，逐步统一。

---

## 1. 布局系统

### 1.1 页面结构
```
┌─────────────────────────────────────────────────┐
│  NavBar (56px, sticky top:0, z-index:100)       │
├─────────────────────────────────────────────────┤
│  padding-top: 24px                              │
│  ┌─────────────────────┐  ┌──────────────┐      │
│  │  .main              │  │  侧边栏       │      │
│  │  max-width: 960px   │  │  280px fixed  │      │
│  │  margin: 0 auto     │  │  right: 24px  │      │
│  │                     │  │  ≥1568px 显示  │      │
│  └─────────────────────┘  └──────────────┘      │
│  padding-bottom: 24px                           │
└─────────────────────────────────────────────────┘
```

### 1.2 核心参数
| 参数 | 值 | 说明 |
|------|-----|------|
| 内容区最大宽度 | `960px` | 所有工具页统一 |
| 内容区居中 | `margin: 0 auto` | **不可修改** |
| 内容区 padding | `24px` | 四周统一 |
| 侧边栏宽度 | `280px` | fixed 定位 |
| 侧边栏断点 | `≥1568px` | 低于此宽度隐藏 |
| 侧边栏 top | 动态 `offsetTop` | 与 .main 顶部齐平 |
| NavBar 高度 | `56px` | sticky, z-index:100 |

### 1.3 全屏工具例外
以下工具**不加侧边栏**，允许全屏布局：
- json / json-schema / code-formatter / sql-formatter / base64 / url-codec / yaml
- 首页 index.html

### 1.4 响应式断点
| 断点 | 行为 |
|------|------|
| `≥1568px` | 显示右侧广告栏 |
| `≤768px` | 双栏变单栏，底部浮动操作条 |
| `≤480px` | 侧边栏按钮变底部横排 |
| `max-height: 800px` | 减小 topbar/padding/margins |

---

## 2. 色彩系统

### 2.1 品牌色
| 用途 | 色值 | 说明 |
|------|------|------|
| 品牌主色 | `#6366F1` (Indigo) | Logo、导航、开发者工具默认 |
| 品牌渐变 | `#6366F1 → #818CF8` | Logo 背景 |

### 2.2 学科色彩（教育工具）
| 学科 | 主色 | 深色 | 适用工具 |
|------|------|------|----------|
| 语文/历史 | `#F59E0B` | `#D97706` | 古诗、成语、作文、汉字、默写 |
| 数学 | `#8B5CF6` | `#7C3AED` | 几何、函数、数列、排列组合 |
| 物理 | `#3B82F6` | `#2563EB` | 物理实验、公式 |
| 英语 | `#10B981` | `#059669` | 语法、单词、音标、拼读 |
| 化学 | `#10B981` | `#059669` | 元素周期表、离子方程式 |
| 高考/考试 | `#EF4444` | `#DC2626` | 高考倒计时、错题本 |
| 医疗/健康 | `#10B981` | `#059669` | BMI、视力表（需⚠️免责声明） |

### 2.3 语义色
| 用途 | Light | Dark |
|------|-------|------|
| 成功 | `#10B981` | `#34D399` |
| 错误 | `#EF4444` | `#F87171` |
| 警告 | `#F59E0B` | `#FBBF24` |
| 信息 | `#3B82F6` | `#60A5FA` |

### 2.4 中性色
| 用途 | Light | Dark |
|------|-------|------|
| 背景 | `#FAFAFA` | `#0A0A0F` |
| 表面 | `rgba(255,255,255,0.72)` | `rgba(30,30,42,0.72)` |
| 表面实色 | `#FFF` | `#1E1E2A` |
| 边框 | `rgba(0,0,0,0.08)` | `rgba(255,255,255,0.08)` |
| 主文字 | `#1A1A2E` | `#F8F8FC` |
| 次文字 | `#6B7280` | `#9494B0` |
| 弱文字 | `#9CA3AF` | `#6B6B8A` |

### 2.5 背景 Mesh 渐变
每个工具页根据学科色使用 mesh 背景：
```css
background: radial-gradient(ellipse at 20% 0%, rgba([学科色],0.05) 0%, transparent 50%),
            radial-gradient(ellipse at 80% 100%, rgba([辅助色],0.03) 0%, transparent 50%),
            var(--bg);
```

---

## 3. 字体系统

### 3.1 字体栈
| 用途 | 字体 |
|------|------|
| UI 文字 | `-apple-system, 'SF Pro Display', 'Inter', 'PingFang SC', sans-serif` |
| 代码/数据 | `'JetBrains Mono', 'SF Mono', 'Fira Code', monospace` |
| 字帖/书法 | `KaiTi, STKaiti`（需 `document.fonts.ready`）|

### 3.2 字号层级
| 级别 | 大小 | 字重 | 用途 |
|------|------|------|------|
| 页面标题 | `24px` | 900 | 工具名称 |
| 区块标题 | `13px` | 800 | section-title |
| 正文 | `14px` | 400-600 | 输入输出内容 |
| 标签/徽章 | `10-11px` | 700 | pill、badge |
| 辅助文字 | `9-10px` | 600 | 注释、legend |

---

## 4. 间距系统

严格 **4px/8px 网格**，不允许任意值。

| Token | 值 | 用途 |
|-------|-----|------|
| `--sp-1` | `4px` | 最小间距 |
| `--sp-2` | `8px` | 紧凑元素间距 |
| `--sp-3` | `12px` | 卡片内间距 |
| `--sp-4` | `16px` | 区块内 padding |
| `--sp-5` | `20px` | glass 卡片 padding |
| `--sp-6` | `24px` | 区块间距、页面 padding |
| `--sp-8` | `32px` | 大区块分隔 |

---

## 5. 圆角系统

| Token | 值 | 用途 |
|-------|-----|------|
| `--radius` | `16px` | glass 卡片、大容器 |
| `--radius-sm` | `10px` | 输入框、内部卡片 |
| `--radius-xs` | `6-8px` | 按钮、标签、pill |
| `--radius-pill` | `20px` | 全圆角按钮 |
| `--radius-round` | `50%` | 圆形头像、步骤编号 |

---

## 6. 阴影系统

| 级别 | 值 | 用途 |
|------|-----|------|
| sm | `0 1px 3px rgba(0,0,0,0.04)` | glass 卡片默认 |
| md | `0 4px 12px rgba(0,0,0,0.06)` | hover 状态 |
| lg | `0 4px 24px rgba(0,0,0,0.08)` | 侧边栏、弹窗 |
| xl | `0 8px 32px rgba(0,0,0,0.12)` | 底部浮动条 |

---

## 7. 组件规范

### 7.1 NavBar
```css
.nav {
  position: sticky; top: 0; z-index: 100;
  backdrop-filter: blur(40px) saturate(180%);
  background: var(--surface);
  border-bottom: 0.5px solid var(--border);
  padding: 0 24px; height: 56px;
  display: flex; align-items: center;
}
.nav-brand { flex-shrink: 0; } /* 防止 logo 被压缩 */
```

### 7.2 Glass 卡片
```css
.glass {
  background: var(--surface);
  backdrop-filter: blur(40px) saturate(180%);
  border: 0.5px solid var(--border);
  border-radius: 16px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.04);
  padding: 20px;
  margin-bottom: 16px;
}
```

### 7.3 按钮（必须包含交互状态）
```css
.btn {
  padding: 12px 24px;
  border-radius: 20px;
  border: none;
  font-weight: 800;
  cursor: pointer;
  transition: all 150ms;
}
.btn:hover {
  transform: translateY(-1px);
  filter: brightness(1.05);
  box-shadow: 0 4px 12px rgba(0,0,0,0.06);
}
.btn:active {
  transform: scale(0.97);
  filter: brightness(0.95);
  box-shadow: none;
}
```

> ⚠️ **所有可点击元素都必须有 `:hover` 和 `:active` 状态。这是底线，不可遗漏。**

### 7.4 Toggle 开关（关闭态）
```css
/* 关闭态 */
background: #D1D5DB; /* 实色灰，不透明 */
border: 1.5px solid rgba(0,0,0,0.1);
/* 滑块阴影 */
box-shadow: 0 1px 4px rgba(0,0,0,0.2);
```

### 7.5 输入框
```css
input, textarea {
  padding: 12px 16px;
  border-radius: 10px;
  border: 1.5px solid var(--border);
  background: var(--surface-solid);
  font-size: 14px;
  outline: none;
  transition: border-color 200ms;
}
input:focus, textarea:focus {
  border-color: var(--brand); /* 当前工具的学科色 */
}
```

### 7.6 Pill / Tab
```css
.pill {
  padding: 5px 12px;
  border-radius: 12px;
  border: 1.5px solid var(--border);
  background: var(--surface-solid);
  font-size: 10px; font-weight: 700;
  cursor: pointer;
  transition: all 150ms;
}
.pill.active {
  background: var(--brand);
  color: #fff;
  border-color: var(--brand);
}
.pill:hover { border-color: var(--brand); }
.pill:active { transform: scale(0.96); }
```

---

## 8. 交互规范

### 8.1 全局交互状态（必须遵循）
| 元素 | hover | active |
|------|-------|--------|
| 按钮 | `translateY(-1px)` + `brightness(1.05)` + shadow | `scale(0.97)` + `brightness(0.95)` + no shadow |
| 卡片 | `translateY(-2px)` + `border-color: brand` + shadow-md | `scale(0.99)` |
| Pill/Tab | `border-color: brand` | `scale(0.96)` |
| 输入框 | — | — (focus: `border-color: brand`) |

### 8.2 过渡
- 默认 `transition: all 150ms ease`
- 布局变化 `transition: all 300ms ease`
- 颜色变化 `transition: color 200ms, background 200ms`

### 8.3 无障碍
- 颜色对比度 ≥ 4.5:1
- 所有可交互元素需 `focus-visible` 样式
- 图片/图标需 `aria-label`
- 语义化 HTML（`<nav>`, `<main>`, `<aside>`, `<section>`）

---

## 9. 暗色模式

- 通过 `[data-theme="dark"]` 切换 CSS 变量
- localStorage key: `devkit-theme`
- **不是简单反色**，是重新设计的暗色方案
- 所有组件必须用 CSS 变量，不能硬编码颜色值
- 特殊页面（如星座 `constellation`）允许默认暗色

---

## 10. 图标规范

- **全站使用 SVG 图标**，不使用 emoji 作为功能图标（跨平台一致性）
- 侧边栏图标由 `sidebar.js` 统一管理
- 教育工具的分类 emoji 仅用于装饰文字（如 section-title 前缀）

---

## 11. 特殊规范

### 11.1 教育工具
- 使用 `--theme-warm` 变体（Liquid Glass + 暖色调）
- 练习模式：正确 → 绿色反馈，错误 → 红色反馈
- 错题本：❌ 红色背景 vs ✅ 绿色背景 双栏对比

### 11.2 医疗/健康工具
- 必须包含 ⚠️ 免责声明
- 声明样式：黄色边框卡片，底部固定

### 11.3 公式渲染
- 使用 KaTeX `katex@0.16.9` CDN
- 数学公式不用图片

### 11.4 汉字/书法
- 笔顺动画使用 HanziWriter 库
- 字帖字体使用 `KaiTi/STKaiti`

---

## 12. 侧边栏规范（V4 最终版）

```
位置: position: fixed; right: 24px
宽度: 280px
显示断点: ≥1568px
top: 动态读取 .main 的 offsetTop（与内容区顶部齐平）
max-height: calc(100vh - 80px)
overflow-y: auto（隐藏滚动条）
```

内容从上到下：
1. 操作按钮（分享/收藏/复制链接/反馈）
2. 广告位 300×250 ×1
3. 热门工具推荐 ×4

**主内容区 `max-width: 960px; margin: 0 auto` 绝不修改。**

---

## 13. 不合规页面修复清单

以下已知问题需要逐步修复：
- [ ] 部分页面 padding-top 不统一（16px vs 24px）→ 统一为 24px
- [ ] 部分早期工具缺少 `:hover` / `:active` 交互状态
- [ ] 部分页面硬编码颜色值，未使用 CSS 变量
- [ ] 个别页面 `.nav-brand` 缺少 `flex-shrink: 0`

---

*文档版本: v1.0 | 维护者: Designer | 最后更新: 2026-03-17*
