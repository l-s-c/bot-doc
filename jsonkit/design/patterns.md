# JSONKit Interaction Patterns

## Navigation

- 侧边栏图标点击切换工具，无页面跳转（SPA 路由）
- URL 同步：`/format`, `/validate`, `/tree`, `/diff`
- 键盘快捷键：`Ctrl/Cmd + 1/2/3/4` 切换工具

## Input Patterns

### 文本输入
- 直接在 CodeMirror 编辑器中粘贴/输入
- 实时语法高亮
- 行号显示

### 文件上传
- 拖拽文件到编辑器区域
- 拖拽时编辑器边框变为 brand 虚线 + 半透明遮罩 "Drop JSON file here"
- 点击上传按钮（工具栏）选择文件
- 上传后自动填充编辑器
- 超过 50MB 显示 toast 错误提示

### URL 输入
- 工具栏提供 "Load from URL" 按钮（二期考虑）

## Output Patterns

### 一键操作
- **复制** — 点击后按钮文字变 "Copied!" + ✓ 图标，1.5s 后恢复
- **下载** — 保存为 .json 文件
- **清空** — 清空输入输出，需二次确认（或 Ctrl+Z 可撤销）

### 格式化动效
- 点击 Format 后，输出面板内容从上到下 fade-in（50ms 延迟逐行）
- 同时状态栏显示处理时间 "Formatted in 23ms"

## Feedback

### 校验反馈
- **通过**: 编辑器底部状态条变绿 + "✓ Valid JSON"
- **失败**: 状态条变红 + "✗ N error(s) found"
- 错误行左侧红条 + 行内波浪下划线
- 点击错误信息跳转到对应行

### Toast 通知
- 复制成功: info toast
- 文件过大: error toast
- 格式化完成: success toast (仅大文件时显示)

### 加载状态
- 大文件处理时：进度条在编辑器顶部（2px 高度，brand 色）
- 按钮变为 loading 状态（spinner 替换图标）

## Theme Switching

- Header 右侧 Sun/Moon 图标按钮
- 点击后整体 300ms 过渡
- 用户偏好保存到 localStorage
- 首次访问跟随系统 prefers-color-scheme

## Keyboard Shortcuts

| 快捷键 | 功能 |
|--------|------|
| `Ctrl/Cmd + Enter` | 执行当前操作（格式化/校验/对比） |
| `Ctrl/Cmd + Shift + F` | 格式化 |
| `Ctrl/Cmd + Shift + M` | 压缩(Minify) |
| `Ctrl/Cmd + Shift + C` | 复制输出 |
| `Ctrl/Cmd + 1/2/3/4` | 切换工具 |
| `Ctrl/Cmd + L` | 清空 |
| `Escape` | 关闭弹窗/取消 |

## Empty State (首次打开)

每个视图的输入区域，在用户未输入内容时展示：
- 图标：对应视图的图标（淡化处理，Indigo 8% 背景圆角方块内）
- 主文案："Paste JSON or drop a file"
- 副文案：提示三种输入方式（粘贴/拖拽/上传）
- CTA 按钮："Load sample JSON" — 一键填充示例数据
- 底部快捷键提示：`⌘V paste · drag & drop · ↑ upload`

## Loading State (大文件处理)

- **进度条**：编辑器顶部 2px 高度，brand 色渐变滑动动画
- **遮罩**：半透明暗色遮罩覆盖输出区域
- **Spinner**：24px 圆形旋转 + "Processing..." 文案 + 文件大小提示
- **按钮状态**：操作按钮 disabled + spinner 替换图标
- 处理完成后遮罩 fade-out (200ms)，结果 fade-in

## Diff Sync Scroll (对比同步滚动)

- 左右两栏滚动联动：滚动一侧，另一侧同步跟随
- 基于行号对齐，非像素对齐（因为增删行导致高度不一致）
- 可通过状态栏按钮临时取消同步

## Responsive

### Desktop (≥1280px)
- 完整双栏布局
- 侧边栏展开

### Tablet (768-1279px)
- 双栏维持，但可切换为上下布局
- 侧边栏收起为图标

### Mobile (< 768px)
- 单栏，输入/输出 tab 切换
- 底部导航替代侧边栏
- 编辑器全宽
