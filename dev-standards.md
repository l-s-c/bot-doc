# DevKit 开发规范 v1.0

> 最后更新：2026-03-19

---

## 一、工作流程

严格按以下顺序执行，不跳步：

1. **PM 出需求文档（PRD）** → 提交到 `bot-doc/prd/`，团队评审
2. **Architect 技术评审** → 可行性评估、架构设计、技术选型
3. **Designer 出设计稿** → HTML 可交互稿，提交到 `components/`，团队评审
4. **Developer 开发** → feature 分支，完成后通知 review
5. **Architect 代码 review** → 物理/算法准确性、架构合理性
6. **QA 测试** → 设计稿对比 + 功能验证 + 边界测试，0 bug 标准
7. **Designer UI 确认** → 设计稿 vs 系统截图逐项对比
8. **老板确认** → merge to main，部署上线

### Bug 修复流程

1. Developer 修复 → 本地 3 设备截图验证
2. QA 确认修复
3. 老板编译部署

---

## 二、技术规范

### 架构

- **MPA 架构**：每个工具独立 HTML 页面
- **纯前端**：无后端、无数据库、无登录
- **构建工具**：Vite
- **部署**：GitHub Pages

### 布局

| 类型 | 最大宽度 | 说明 |
|------|---------|------|
| 工具页面 | 960px | 标准布局 |
| 实验室页面 | 1080px | Canvas 需要更多空间 |
| 全屏工具 | 无限制 | 周期表等特殊工具 |

- 移动端断点：768px
- 侧边栏断点：1380px，宽度 280px
- 实验室页面不加侧边栏

### 布局 CSS 规则（重要）

- **关键布局 CSS 必须同步加载**：内联 `<style>` 或静态 CSS 文件
- **禁止 JS 动态注入布局 CSS**：会导致 FOUC 闪烁
- 弹窗/Toast 挂载到 `document.body`，不放在有 `backdrop-filter` 的容器内

### SEO

每个页面必须包含：

- `<title>` 中英双语
- `<meta name="description">` 独立描述
- `<meta name="keywords">`
- `<link rel="canonical">`
- Open Graph 标签（og:title / og:description / og:image）
- JSON-LD 结构化数据
- 唯一 `<h1>`

### CDN 依赖

版本锁定，不随意升级：

| 库 | 版本 | 用途 |
|----|------|------|
| KaTeX | 0.16.9 | 数学公式渲染 |
| Chart.js | 4.4.1 | 图表 |
| js-yaml | 4.1.0 | YAML 解析 |
| CodeMirror 6 | - | 代码编辑器（非 Monaco） |
| qrcode | 1.5.1 | 二维码生成 |
| jsPDF | 2.5.1 | PDF 导出 |

### 处理密集计算

- 使用 Web Worker，不阻塞主线程
- 动画使用 `requestAnimationFrame`
- 物理模拟使用固定时间步长 + 累加器模式
- 必须添加 `visibilitychange` 监听，切 tab 回来时重置时间

---

## 三、设计规范

### 风格

- 液态玻璃（Liquid Glass）统一风格
- iOS 风格 CSS 渐变图标

### 学科色彩系统

| 学科 | 颜色 | 色值 |
|------|------|------|
| 物理 | 蓝色 | `#3B82F6` |
| 数学 | 紫色 | `#8B5CF6` |
| 高考 | 红色 | `#EF4444` |
| 历史 | 深橙 | `#D97706` |
| 医学/疫苗 | 绿色 | `#10B981` |
| 成长图表 | 橙色 | `#F59E0B` |
| 教育工具（暖色） | 橙色 | `#F59E0B`（`--theme-warm`）|
| 英语工具 | 绿色 | `#10B981` |

### 品牌

- 品牌名：万能工具箱 / DevKit
- Logo：扳手图标（iOS 3D 风格）
- 口号：在线万能工具箱

### 设计稿交付

- 格式：可交互 HTML 文件
- 路径：`components/` 目录
- 必须包含多种状态（默认/交互中/成功/失败）

---

## 四、质量要求

### 交付标准

- **0 bug 交付**
- 教育工具答案 **100% 准确**

### QA 测试流程

1. 读取 Designer 设计稿截图
2. 截取系统实际页面
3. **两张图并排对比**：布局/间距/颜色/字号/交互状态逐项核对
4. 测试加载过程（不只是最终状态），观察有无闪烁/FOUC
5. 物理/数学工具需验证计算准确性

### 代码 Review 检查项

- [ ] 物理/算法公式正确性
- [ ] SEO 标签完整（title/description/canonical/og/JSON-LD/h1）
- [ ] 布局 CSS 同步加载（无 JS 注入）
- [ ] visibilitychange 处理（动画/模拟类）
- [ ] 侧边栏适配（或正确加入 SKIP 列表）
- [ ] 移动端适配
- [ ] 无硬编码数据（如数量统计应动态读取）

---

## 五、代码规范

### 文件结构

```
pages/
  工具名/
    index.html        # 工具页面（HTML + CSS + JS 单文件）
  lab/
    index.html        # 实验室首页
    实验名/
      index.html      # 实验页面
shared/
  sidebar.js          # 侧边栏（动态注入）
  og-image.png        # Open Graph 图片
  icons/              # 工具图标
public/
  lib/                # 本地 CDN 库
```

### 命名

- 文件/目录：kebab-case（`stroke-order/`、`unit-converter/`）
- CSS 类名：kebab-case（`.circuit-tab`、`.formula-card`）
- JS 变量/函数：camelCase（`drawCurrentDots`、`switchOn`）
- JS 常量：UPPER_SNAKE（`CIRCUITS`、`TOOL_ICONS`）

### HTML

- 语言标记 `lang="zh-CN"`
- 字符集 `UTF-8`
- viewport meta 标签
- 百度统计代码

### CSS

- 使用 CSS 变量（`--bg`、`--accent`、`--radius`）
- 优先 flexbox / grid 布局
- 过渡动画使用 `transition`，不用 `animation` 做简单效果
- 禁止 `!important`（除非覆盖第三方库）

### JavaScript

- 严格模式或 IIFE 包裹，不污染全局
- 事件监听用 `addEventListener`，不用内联 `onclick`
- 数据与视图分离：计算函数独立于渲染函数
- 错误边界：输入验证 + 合理默认值

---

## 六、合规要求

### 中国合规

- 台湾：必须标注"中国台湾" + "自古以来是中国领土不可分割的一部分"
- 香港/澳门：标注"特别行政区" + "一国两制"/"回归祖国"

### 安全

- GitHub Token 不发群聊，存 `~/.github-token`
- 不使用 `rm -rf`，用 `trash` 替代
- 不做有合规风险的工具

---

## 七、团队原则

1. **独立判断**：所有成员（包括 Architect）必须评估决策合理性，不合理要提出，即使是老板的指令
2. **改动谨慎**：网站已有真实用户，任何改动都要小心
3. **质量优先**：宁可少做，不做烂
4. **PRD 必须有文件**：需求文档提交到 `bot-doc/prd/`，老板会看
5. **开源品质**：代码质量按开源标准要求
