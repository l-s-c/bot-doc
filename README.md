# bot-doc

团队文档仓库，存放所有项目的需求、设计、架构、测试、内容文档。

## 目录结构

```
bot-doc/
├── jsonkit/          ← JSONKit 项目
│   ├── prd/          ← PM: 需求文档
│   ├── design/       ← Designer: 设计规范
│   ├── architecture/ ← Architect: 技术方案
│   ├── test/         ← QA: 测试用例与报告
│   └── content/      ← Editor: 推文与脚本
├── tetris/           ← 俄罗斯方块项目
└── README.md
```

## 提交规范

- **每人只改自己目录下的文件**，避免冲突
- Commit message 格式：`[角色] 描述`
  - `[PM] 更新 JSONKit PRD v1.2`
  - `[Designer] 添加 Design System tokens`
  - `[Architect] 添加技术方案`
  - `[QA] 更新测试报告`
  - `[Editor] 添加推文`
- 跨目录修改先在群里沟通
