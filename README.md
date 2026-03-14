# bot-doc

李世超开发团队文档仓库。存放所有项目的需求、设计、架构、测试、运营文档。

## 项目总览

| # | 项目 | 类型 | 状态 | 备注 |
|---|------|------|------|------|
| 1 | 俄罗斯方块 | Web 游戏 | ✅ 已完成 | 3 轮 bug 修复，QA 通过 |
| 2 | DevKit 开发者工具站 | Web 工具平台（14 个工具） | ✅ 已上线 | [线上地址](https://l-s-c.github.io/devkit/)，MPA 架构，7 轮 QA，域名备案中 |
| 3 | 简压照片缩图 | 微信小程序 | ⏳ 审核中 | 图片压缩工具，v7 浅色设计，个人主体 |
| 4 | 掘金技术文章 | 内容运营 | ✅ 3 篇已发 | DevKit 介绍 / 团队故事 / 被 AI 催着干活 |
| 5 | 小红书运营 | 内容运营 | ✅ 1 篇已发 | 团队介绍帖 + 5 条推广文案待发 |
| 6 | 团队文档体系 | 基础建设 | ✅ 已建立 | PRD / 技术方案 / 开发规范 / 测试用例 |

## 目录结构

```
bot-doc/
├── README.md
├── tetris/                ← 俄罗斯方块
├── jsonkit/               ← DevKit（原 JSONKit）
│   ├── prd/               ← PM: 需求文档
│   ├── design/            ← Designer: 设计规范
│   ├── architecture/      ← Architect: 技术方案
│   ├── test/              ← QA: 测试用例与报告
│   └── content/           ← Editor: 推文与脚本
├── image-compress/        ← 简压照片缩图小程序
│   └── architecture/      ← Architect: 技术方案
└── miniprogram/           ← 小程序通用规范
    └── dev-standards.md   ← 开发规范（rpx/图标/按钮/Review Checklist）
```

## 团队成员

| 角色 | 职责 |
|------|------|
| PM | 需求管理、进度跟踪、项目协调 |
| Architect | 技术可行性评估、架构设计、代码 Review |
| Developer | 编码实现、Bug 修复 |
| QA | 测试用例、质量把关、提审门禁 |
| Designer | UI/UX 设计、设计稿交付 |
| Editor | 内容运营（掘金/小红书）、上架文案 |
| Business Lead | 商业分析、变现策略 |

## 提交规范

- Commit message 格式：`类型: 描述`
  - `feat: 小程序开发规范文档`
  - `fix: 修复 diff.js showLoading bug`
  - `docs: 更新 README`
- 所有文档通过 Architect 统一提交
- 跨目录修改先在群里沟通

## 相关链接

- **DevKit 代码仓库**：<https://github.com/l-s-c/devkit>
- **团队沟通**：Discord #team 频道
- **掘金专栏**：已发布 3 篇文章
