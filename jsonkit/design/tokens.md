# JSONKit Design Tokens

## Brand

**产品名**: JSONKit
**Tagline**: The JSON toolkit that doesn't suck.
**定位**: 现代、专业、克制

---

## Colors

### Brand Colors
| Token | Light | Dark | 用途 |
|-------|-------|------|------|
| `brand-500` | `#6366F1` | `#818CF8` | 主品牌色（Indigo） |
| `brand-600` | `#4F46E5` | `#6366F1` | Hover / Active |
| `brand-400` | `#818CF8` | `#A5B4FC` | 轻量元素 |
| `brand-50` | `#EEF2FF` | `#1E1B4B` | 背景点缀 |

> 选择 Indigo 的理由：区别于竞品的绿色(jsonformatter)和蓝色(JSON Crack)，Indigo 兼具专业感和现代感，在暗色背景上尤其出彩。

### Semantic Colors
| Token | Light | Dark | 用途 |
|-------|-------|------|------|
| `success` | `#10B981` | `#34D399` | 校验通过、新增(diff) |
| `error` | `#EF4444` | `#F87171` | 校验失败、删除(diff) |
| `warning` | `#F59E0B` | `#FBBF24` | 修改(diff)、注意 |
| `info` | `#3B82F6` | `#60A5FA` | 提示、链接 |

### Neutral (Dark Theme - Primary)
| Token | Value | 用途 |
|-------|-------|------|
| `gray-950` | `#0A0A0F` | 页面背景 |
| `gray-900` | `#111118` | 侧边栏、面板背景 |
| `gray-850` | `#16161F` | 编辑器背景 |
| `gray-800` | `#1E1E2A` | 卡片/容器背景 |
| `gray-700` | `#2A2A3C` | 边框、分割线 |
| `gray-600` | `#3F3F5C` | Disabled、次要边框 |
| `gray-500` | `#6B6B8A` | 占位文字 |
| `gray-400` | `#9494B0` | 次要文字 |
| `gray-300` | `#B8B8D0` | 正文文字 |
| `gray-200` | `#D4D4E8` | 标题文字 |
| `gray-100` | `#EDEDF5` | 高亮文字 |
| `gray-50` | `#F8F8FC` | 最亮文字 |

### Neutral (Light Theme)
| Token | Value | 用途 |
|-------|-------|------|
| `light-bg` | `#FAFAFA` | 页面背景 |
| `light-surface` | `#FFFFFF` | 卡片/面板 |
| `light-border` | `#E5E5ED` | 边框 |
| `light-text` | `#1A1A2E` | 正文 |
| `light-text-secondary` | `#6B6B8A` | 次要文字 |

---

## Typography

**字体栈**:
- Code: `'JetBrains Mono', 'Fira Code', 'SF Mono', 'Cascadia Code', monospace`
- UI: `'Inter', -apple-system, 'Segoe UI', sans-serif`

### Scale
| Token | Size | Weight | Line Height | 用途 |
|-------|------|--------|-------------|------|
| `h1` | 24px | 700 | 32px | 页面标题 |
| `h2` | 20px | 600 | 28px | 区域标题 |
| `h3` | 16px | 600 | 24px | 卡片标题 |
| `body` | 14px | 400 | 22px | 正文 |
| `body-sm` | 13px | 400 | 20px | 辅助文字 |
| `caption` | 12px | 500 | 16px | 标签、徽章 |
| `code` | 13px | 400 | 20px | 代码（等宽） |

---

## Spacing

基于 4px 网格：

| Token | Value | 用途 |
|-------|-------|------|
| `space-1` | 4px | 紧凑间距 |
| `space-2` | 8px | 元素内间距 |
| `space-3` | 12px | 小组件间距 |
| `space-4` | 16px | 组件间距 |
| `space-5` | 20px | 区域间距 |
| `space-6` | 24px | 大区域间距 |
| `space-8` | 32px | 面板间距 |
| `space-10` | 40px | 页面边距 |
| `space-12` | 48px | 最大间距 |

---

## Border Radius

| Token | Value | 用途 |
|-------|-------|------|
| `radius-sm` | 4px | 小元素（标签、徽章） |
| `radius-md` | 6px | 按钮、输入框 |
| `radius-lg` | 8px | 卡片、面板 |
| `radius-xl` | 12px | 大容器、弹窗 |
| `radius-full` | 9999px | 圆形元素 |

---

## Shadows (Dark Theme)

| Token | Value | 用途 |
|-------|-------|------|
| `shadow-sm` | `0 1px 2px rgba(0,0,0,0.3)` | 按钮、输入框 |
| `shadow-md` | `0 4px 12px rgba(0,0,0,0.4)` | 卡片、下拉 |
| `shadow-lg` | `0 8px 24px rgba(0,0,0,0.5)` | 弹窗 |
| `shadow-glow` | `0 0 20px rgba(99,102,241,0.15)` | Focus 发光 |

---

## Transitions

| Token | Value | 用途 |
|-------|-------|------|
| `ease-default` | `150ms ease` | 按钮、hover |
| `ease-smooth` | `250ms cubic-bezier(0.4,0,0.2,1)` | 面板展开 |
| `ease-spring` | `300ms cubic-bezier(0.34,1.56,0.64,1)` | 弹性动效 |

---

## Z-Index

| Token | Value | 用途 |
|-------|-------|------|
| `z-base` | 0 | 默认 |
| `z-sidebar` | 10 | 侧边栏 |
| `z-header` | 20 | 顶栏 |
| `z-dropdown` | 30 | 下拉菜单 |
| `z-modal` | 40 | 弹窗 |
| `z-toast` | 50 | Toast 通知 |
