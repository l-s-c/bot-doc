# DevKit 侧边栏完整适配方案

> 作者: Designer | 日期: 2026-03-17 | 状态: 待老板确认

---

## 1. 工具页面布局分类

经排查全站 141 个工具页面（不含首页），分为 **3 类布局**：

### A 类 — 标准 960px 居中（113 个）✅ 可直接加侧边栏

已使用 `max-width: 960px; margin: 0 auto`，`.glass` 卡片 `width: 100%` 撑满容器。

**侧边栏处理：** 直接适配，零修改。

<details>
<summary>完整列表（113 个）</summary>

age-calculator, avatar-generator, blood-type, bmi, box-shadow, calendar, case-converter, char-structure, chart, chem-balance, china-provinces, chinese-convert, code-screenshot, collage, color-picker, combination, constellation, coord-convert, copybook, cover-maker, csr-generator, csv-viewer, currency, date-diff, emoji-search, encrypt, english-grammar, equation-solver, essay-paper, essay-scorer, essay-template, eye-chart, favicon, fraction-calc, function-plot, gaokao-countdown, geometry-calc, gradient, grid-splitter, growth-chart, history-timeline, homophone, html-entity, html-to-markdown, http-tester, id-lookup, id-photo, idiom-dict, image-resize, img-to-pdf, interest, invoice, ion-equation, ip-lookup, irregular-verbs, json-schema, kaoyan-countdown, lorem, math-formulas, math-practice, mistake-book, molar-mass, morse-code, multiplication, name-generator, notepad, pdf-compress, pdf-merge, pdf-split, phonetic-chart, phonics, physics-formulas, physics-lab, pinyin-practice, pinyin-table, pinyin-tone, placeholder, poetry-dictation, poetry-practice, poetry-recite, pomodoro, prepayment, qr-scanner, radical-lookup, random-picker, score-lookup, screen-recorder, sentence-parse, sequence-sum, signature, spin-wheel, stroke-count, stroke-order, sudoku, synonym, tax, team, tense-chart, text-dedupe, toml, tts, ua-parser, unicode, unit-convert-edu, university, vaccine-schedule, watermark, word-cards, word-counter, word-formation, world-flags, world-records, writing-material, writing-prompt, zipcode, zodiac
</details>

### B 类 — 窄内容（< 960px）（22 个）⚠️ 需调整后加侧边栏

这些工具的容器 `max-width` 小于 960px（640px~900px），导致内容区窄，加侧边栏后视觉不平衡。

| 工具 | 当前 max-width | 需要的改动 |
|------|---------------|-----------|
| password | 640px | 加"密码强度说明"卡片，撑满 960px |
| number-base | 640px | 加"进制对照表"，撑满 960px |
| countdown | 640px | 加"倒计时使用技巧"，撑满 960px |
| random | 600px | 加"随机数原理说明"，撑满 960px |
| unit-converter | 640px | 加"单位换算速查表"，撑满 960px |
| cron | 700px | 加"Cron 常用表达式"，撑满 960px |
| timer | 720px | 加"番茄工作法说明"，撑满 960px |
| timestamp | 720px | 加"时区对照表"，撑满 960px |
| image-compress | 720px | 加"图片压缩知识"，撑满 960px |
| image-format | 720px | 加"格式对比说明"，撑满 960px |
| markdown | 800px | 调宽到 960px |
| qrcode | 900px | 调宽到 960px |
| color | 无 max-width | 确认实际宽度，必要时加容器 |
| hash | 无 max-width | 确认实际宽度，必要时加容器 |
| jwt | 无 max-width | 确认实际宽度，必要时加容器 |
| regex | 无 max-width | 确认实际宽度，必要时加容器 |
| text-diff | 无 max-width | 确认实际宽度，必要时加容器 |
| periodic-table | 100%（特殊） | 18 列网格需全宽，归入 C 类 |
| yaml | 768px 媒体查询 | 调整为 960px 容器 |

**改动方式：**
- `max-width` 统一改为 `960px`
- 内容不够宽的工具，Designer 补充"知识卡片"/"使用说明"填充内容
- 每个工具约 30 分钟设计 + 30 分钟开发

### C 类 — 全屏/特殊布局（6 个）❌ 不加侧边栏

| 工具 | 原因 |
|------|------|
| json | 全屏 SPA 编辑器 |
| base64 | 全屏双栏编解码 |
| code-formatter | CodeMirror 全屏 |
| sql-formatter | CodeMirror 全屏 |
| url-codec | 全屏双栏 |
| periodic-table | 18 列 CSS Grid 需全宽 |

**侧边栏处理：** `sidebar.js` 排除列表跳过，不注入。

---

## 2. 侧边栏最终参数

| 参数 | 值 | 说明 |
|------|-----|------|
| **宽度** | `280px` | 可放 300×250 AdSense 标准广告 |
| **定位** | `position: fixed; right: 24px` | 脱离文档流，不影响页面高度 |
| **断点** | `≥1380px` | 老板屏幕 1455px 可见 |
| **top** | 动态 `offsetTop` | 与 .main 顶部齐平 |
| **max-height** | `calc(100vh - 80px)` | 防止超出视口 |
| **overflow** | `overflow-y: auto`（隐藏滚动条） | 内容过多时内部滚动 |

### 内容区适配
```css
/* ≥1380px 时，内容区在剩余空间居中 */
@media (min-width: 1380px) {
  .main {
    margin-right: 328px; /* 280 + 24 gap + 24 right */
    margin-left: max(24px, calc((100vw - 960px - 328px) / 2));
  }
}
```

**前提条件：所有 B 类工具的 max-width 统一为 960px 后，此公式有效。** 因为 960px 容器在剩余空间 `(100vw - 328px)` 内居中，容器是满宽的，视觉不会偏。

---

## 3. 侧边栏内容

从上到下：
1. **操作按钮**（横排 4 个）：分享 / 收藏 / 复制链接 / 反馈
2. **广告位** 300×250 ×1（AdSense 标准矩形）
3. **热门工具推荐** ×4（图标 + 名称 + 描述）

总高度约 **500px**，绝大部分工具页面内容都比这高。

---

## 4. 执行计划

### 第一步：底部广告位（立刻可做，0.5 天）
- `footer-ad.js` 动态注入，追加在 `.main` 底部
- 728×90 横幅广告 + 热门工具推荐
- 所有 141 个页面通用，零兼容问题
- 在 `feature/sidebar` 分支开发，老板确认后合 main

### 第二步：B 类工具统一 960px（3-5 天）
- Designer 出 22 个工具的内容补充设计稿（知识卡片/说明区）
- Developer 逐个调整 `max-width` 为 960px + 补充内容
- QA 逐页验证布局
- 合并到 main

### 第三步：侧边栏上线（1 天）
- B 类统一后，所有工具页面都是 960px 居中
- `sidebar.js` 注入 `margin-right: 328px` + 右侧广告栏
- C 类 6 个页面跳过
- `feature/sidebar` 分支本地截图 → 老板确认 → 合 main

### 工作量汇总
| 步骤 | Designer | Developer | QA | 耗时 |
|------|----------|-----------|-----|------|
| 底部广告 | 设计稿 1 个 | 1 个 JS 组件 | 全站验证 | 0.5 天 |
| B 类统一 | 22 个补充设计 | 22 个页面调整 | 22 页验证 | 3-5 天 |
| 侧边栏 | 已有 V4 设计稿 | sidebar.js 注入 | 全站验证 | 1 天 |
| **合计** | | | | **5-7 天** |

---

## 5. 设计原则（避免再次返工）

1. **`.main` 的 `max-width: 960px; margin: 0 auto` 是铁律** — 未来新工具必须使用 960px 容器
2. **sidebar.js 零侵入** — 不修改任何现有 HTML 文件的结构
3. **分支开发** — 所有布局改动在 `feature/sidebar` 分支，本地确认后再合 main
4. **逐页截图验收** — 不信任 CSS 数值，看真实渲染效果

---

*提交到 bot-doc 仓库存档*
