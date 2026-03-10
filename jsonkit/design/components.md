# JSONKit Component Specs

## Layout

### App Shell
```
┌─────────────────────────────────────────────┐
│ Header (48px)          Logo  Theme  GitHub   │
├────────┬────────────────────────────────────┤
│Sidebar │  Content Area                       │
│ (56px) │                                     │
│        │  ┌─────────────┬─────────────────┐ │
│  🔧    │  │  Input       │  Output          │ │
│  ✓     │  │  Panel       │  Panel           │ │
│  🌳    │  │              │                  │ │
│  ⇔     │  │              │                  │ │
│        │  │              │                  │ │
│        │  └─────────────┴─────────────────┘ │
│        │  ┌─────────────────────────────────┐│
│        │  │  Status Bar (28px)               ││
│        │  └─────────────────────────────────┘│
├────────┴────────────────────────────────────┤
│ (No footer - clean)                          │
└─────────────────────────────────────────────┘
```

- **Header**: 48px 高，左 Logo，右工具（主题切换、GitHub 链接）
- **Sidebar**: 56px 宽，图标导航，hover 显示 tooltip
- **Content**: 弹性宽度，左右双栏 50/50（可拖拽调整）
- **Status Bar**: 28px，显示文件大小、行数、处理时间

### Dual Panel (核心布局)
- 左右等分，中间拖拽分割条(4px)
- 分割条 hover 变亮 + cursor: col-resize
- 每个面板顶部有工具栏(36px)：操作按钮 + 下拉菜单

---

## Buttons

### Primary
- 背景: `brand-500` / hover: `brand-600`
- 文字: white
- Padding: 8px 16px
- 圆角: `radius-md`
- 过渡: `ease-default`
- Focus: `shadow-glow` + 2px brand ring

### Secondary
- 背景: transparent / hover: `gray-800`
- 边框: 1px `gray-700`
- 文字: `gray-300`
- 同等尺寸

### Ghost
- 背景: transparent / hover: `gray-800`
- 无边框
- 文字: `gray-400`
- 用于工具栏图标按钮

### Icon Button
- 32×32px / 28×28px(small)
- 圆角: `radius-md`
- hover: `gray-800` 背景
- 用于工具栏操作

### States
- `:hover` — 背景加深一级
- `:active` — 背景再加深 + scale(0.98)
- `:focus-visible` — brand glow ring
- `:disabled` — opacity: 0.4, cursor: not-allowed

---

## Input / Textarea

- 背景: `gray-850`
- 边框: 1px `gray-700` / focus: `brand-500`
- 文字: `gray-200`
- Placeholder: `gray-500`
- Padding: 8px 12px
- 圆角: `radius-md`
- Focus: border-color transition + subtle glow

---

## Select / Dropdown

- 触发器样式同 Input
- 下拉面板: `gray-800` 背景, `shadow-md`
- 选项 hover: `gray-700`
- 选中项: 左侧 brand 色条 (2px)
- 圆角: `radius-lg`

---

## Toggle / Switch

- 44×24px 轨道
- OFF: `gray-700` 轨道, `gray-400` 圆点
- ON: `brand-500` 轨道, white 圆点
- 过渡: `ease-smooth`

---

## Toast

- 右下角弹出
- 背景: `gray-800`, 边框: 1px `gray-700`
- 左侧 4px 色条表示类型(success/error/info)
- 进入: slide-up + fade-in
- 3s 后自动消失 (slide-down + fade-out)
- 圆角: `radius-lg`

---

## Badge / Tag

- 类型标签: `string`=蓝, `number`=绿, `boolean`=紫, `null`=灰, `array`=橙, `object`=靛
- Padding: 2px 6px
- Font: `caption` (12px)
- 圆角: `radius-sm`
- 背景: 对应色 10% 不透明度, 文字: 对应色

---

## Tabs (工具栏内)

- 底部无线条，选中项: `brand-500` 背景 10% + 文字高亮
- 非选中: `gray-400` 文字
- hover: `gray-300` 文字
- 切换过渡: background fade `ease-default`

---

## Sidebar Nav

- 图标: 20×20px, Lucide icon set
- 非选中: `gray-500` 图标色
- Hover: `gray-300` + `gray-800` 背景
- 选中: `brand-500` 图标色 + 左侧 2px brand 色条
- Tooltip: 延迟 300ms, `gray-700` 背景, `gray-200` 文字

### Nav Items (MVP)
1. 🔧 Format — 格式化/美化
2. ✓ Validate — 校验
3. 🌳 Tree — 树形可视化
4. ⇔ Diff — JSON 对比

---

## Code Editor Area

- 使用 CodeMirror 6
- 背景: `gray-850`
- 行号: `gray-600`
- 当前行高亮: `gray-800` 背景
- 选中文字: `brand-500` 20% 不透明度背景
- 错误行: `error` 10% 不透明度背景 + 左侧 3px 红条
- 光标: `brand-400`

---

## Diff View

- 新增行: `success` 8% 背景
- 删除行: `error` 8% 背景
- 修改行: `warning` 8% 背景
- 行号区域颜色跟随
- 折叠区域: 点状虚线分隔 + "展开 N 行相同内容" 可点击

---

## Tree View

- 缩进: 每层 20px, 缩进线: 1px `gray-700`
- 折叠箭头: ▶(收起) / ▼(展开), `gray-500`, hover `gray-300`
- Key 文字: `gray-200` (Inter font)
- Value 文字: 按类型着色 (string=蓝, number=绿, boolean=紫, null=灰)
- 类型 badge: 见 Badge 规范
- Hover 行: `gray-800` 背景
- 选中行: `brand-500` 8% 背景 + 左侧 2px brand 条
- 节点计数: `gray-500` caption 文字 (如 "3 items", "5 keys")

---

## Resizable Splitter

- 宽度: 4px
- 默认: `gray-700`
- Hover: `brand-500`, cursor: col-resize
- 拖拽中: `brand-400`
