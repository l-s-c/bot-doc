# DevKit SEO 文档

> 每个页面的 SEO 标签记录 | 最后更新：2026-03-14

## SEO 规范

每个页面必须包含：
- `<title>` — 中文工具名 + 英文关键词 + "— DevKit"
- `<meta name="description">` — 功能描述，中英混合，包含核心搜索词
- `<meta name="keywords">` — 中英文关键词
- `<link rel="canonical">` — 规范 URL
- OG 标签（og:title, og:description, og:url）
- JSON-LD WebApplication 结构化数据
- `<h1>` — 页面标题
- `<noscript>` — 无 JS 降级内容
- 百度统计代码

---

## 各页面 SEO 标签

### 首页 (/)
- **title**: DevKit — 免费在线开发者工具箱
- **description**: DevKit 是免费在线开发者工具箱，提供 JSON 格式化、Base64 编解码、正则测试、时间戳转换等 50+ 工具。纯前端，数据不上传。

### JSON 工具 (/pages/json/)
- **title**: JSONKit — 免费在线 JSON 格式化、校验、转换、对比工具
- **description**: JSONKit 是一款免费、无广告的在线 JSON 工具箱。支持 JSON 格式化、压缩、校验（错误行定位）、树形视图、JSONPath 查询、8 种格式转换（YAML/XML/CSV/TypeScript/Go/Java/Python/JSON Schema）、语义化对比合并。纯前端，数据不上传服务器。

### Base64 编解码 (/pages/base64/)
- **title**: Base64 编解码 — DevKit 在线开发者工具箱
- **description**: 在线 Base64 编码解码工具，支持文本和文件互转，URL 安全模式。纯前端，数据不上传。

### URL 编解码 (/pages/url-codec/)
- **title**: URL 编解码 — DevKit 在线开发者工具箱
- **description**: 在线 URL 编码解码工具，支持 encodeURIComponent 和 encodeURI 两种模式。纯前端，数据不上传。

### JWT 解码器 (/pages/jwt/)
- **title**: JWT 解码器 — DevKit 在线开发者工具箱
- **description**: 在线 JWT 解码工具，解析 Header 和 Payload，自动识别时间字段并标注过期状态。纯前端，数据不上传。

### 正则表达式测试 (/pages/regex/)
- **title**: 正则表达式测试 — DevKit 在线开发者工具箱
- **description**: 在线正则表达式测试工具，实时匹配高亮、分组捕获、替换预览。纯前端，数据不上传。

### 时间戳转换 (/pages/timestamp/)
- **title**: 时间戳转换 — DevKit 在线开发者工具箱
- **description**: 在线 Unix 时间戳与日期时间互转，支持毫秒、秒，实时时钟显示，多时区。纯前端，数据不上传。

### UUID 生成器 (/pages/uuid/)
- **title**: UUID 生成器 — DevKit 在线开发者工具箱
- **description**: 在线批量生成 UUID v4，支持大写输出和无连字符格式。纯前端，数据不上传。

### Hash 计算 (/pages/hash/)
- **title**: Hash 计算 — DevKit 在线开发者工具箱
- **description**: 在线 Hash 计算工具，支持 SHA-1、SHA-256、SHA-384、SHA-512。纯前端，数据不上传。

### 颜色转换器 (/pages/color/)
- **title**: 颜色转换器 — DevKit 在线开发者工具箱
- **description**: 在线颜色转换工具，HEX/RGB/HSL/CMYK 互转，调色板生成，CSS 渐变生成，WCAG 对比度检查。

### 文本对比 (/pages/text-diff/)
- **title**: 文本对比 — DevKit 在线开发者工具箱
- **description**: 在线文本对比工具，逐行高亮差异，支持忽略空白和大小写。纯前端，数据不上传。

### 代码格式化 (/pages/code-formatter/)
- **title**: 代码格式化 — DevKit 在线开发者工具箱
- **description**: 在线 HTML/CSS/JavaScript 代码格式化工具，支持压缩和美化。纯前端，数据不上传。

### Markdown 预览 (/pages/markdown/)
- **title**: Markdown 预览 — DevKit 在线开发者工具箱
- **description**: 在线 Markdown 编辑器，实时预览渲染效果，支持导出 HTML。纯前端，数据不上传。

### Cron 解析 (/pages/cron/)
- **title**: Cron 表达式解析 — DevKit 在线开发者工具箱
- **description**: 在线 Cron 表达式解析器，可视化下次执行时间，支持 5 字段和 6 字段格式。纯前端，数据不上传。

### SQL 格式化 (/pages/sql-formatter/)
- **title**: SQL 格式化 — DevKit 在线开发者工具箱
- **description**: 在线 SQL 格式化工具，关键字大写，智能缩进。纯前端，数据不上传。

### 密码生成器 (/pages/password/)
- **title**: 密码生成器 — DevKit 在线开发者工具箱
- **description**: 在线安全密码生成器，使用 crypto.getRandomValues 生成真随机密码，支持自定义长度、字符类型、批量生成。纯前端，数据不上传。

### 进制转换 (/pages/number-base/)
- **title**: 进制转换 — DevKit 在线开发者工具箱
- **description**: 在线进制转换工具，支持二进制、八进制、十进制、十六进制互转，实时转换，支持大数。纯前端，数据不上传。

### 单位换算 (/pages/unit-converter/)
- **title**: 单位换算 — DevKit 在线开发者工具箱
- **description**: 在线单位换算工具，支持长度、重量、温度、CSS、数据、时间六大类单位实时互转。纯前端，数据不上传。

### YAML 格式化 (/pages/yaml/)
- **title**: YAML 格式化 — DevKit 在线开发者工具箱
- **description**: 在线 YAML 格式化、压缩、校验工具，支持 JSON ↔ YAML 互转。纯前端，数据不上传。

### 图片压缩 (/pages/image-compress/)
- **title**: 图片压缩 — DevKit 在线开发者工具箱
- **description**: 在线图片压缩工具，纯浏览器端处理，支持 JPG/PNG/WebP 格式转换、批量压缩、前后对比。隐私安全，零上传。

### 图片格式转换 (/pages/image-format/)
- **title**: 图片格式转换 — DevKit 在线开发者工具箱
- **description**: 在线图片格式转换工具，支持 JPG/PNG/WebP/BMP 互转，纯浏览器端处理，隐私安全，零上传。

### 二维码生成器 (/pages/qrcode/)
- **title**: 二维码生成器 — DevKit 在线开发者工具箱
- **description**: 在线二维码生成器，支持自定义颜色、Logo、WiFi/邮件/名片模板，下载 PNG/SVG/JPG。纯浏览器端，零上传。

### 倒数日计算器 (/pages/countdown/)
- **title**: 倒数日计算器 — DevKit 在线开发者工具箱
- **description**: 在线倒数日计算器，计算距离目标日期天数，支持日期差值计算、常用节日快捷入口、今年进度可视化。

### BMI 计算器 (/pages/bmi/)
- **title**: BMI 计算器 — DevKit 在线开发者工具箱
- **description**: 在线 BMI 身体质量指数计算器，基于中国成人标准，实时计算健康体重范围，渐变仪表条直观展示。

### 随机数生成器 (/pages/random/)
- **title**: 随机数生成器 — DevKit 在线开发者工具箱
- **description**: 在线随机数生成器，支持整数、浮点数、骰子模式，批量生成，Fisher-Yates 不重复，crypto 安全随机。

### 大小写转换 (/pages/case-converter/) ⚠️ 待更新
- **title**: 大小写转换 — DevKit 在线开发者工具箱
  - ➡️ 建议改为：`大小写转换 | Case Converter Online — DevKit`
- **description**: 在线大小写转换工具，支持全大写、全小写、首字母大写、camelCase、PascalCase、snake_case、kebab-case 等 8 种模式。
  - ➡️ 建议加英文：`...camelCase, PascalCase, snake_case, kebab-case converter`
- **keywords**: ⚠️ 缺失
  - ➡️ 建议加：`case converter, 大小写转换, camelCase converter, snake_case converter`

---

## 待优化项

部分早期工具缺少 `<meta name="keywords">`，后续统一补充。

---

*维护：Architect | 每个新工具上线时同步更新*
