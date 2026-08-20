# frontend-rule

前端开发规范 Skill，用于让 AI 编码助手在 React、Vue、TypeScript、Electron 渲染进程、H5、后台管理系统等前端项目中遵循统一工程规范。

## 能力范围

- 目录职责与模块边界
- 文件、类型、变量和常量命名
- API 层、服务层和组件层边界
- 状态管理职责划分
- 路由 `name` 优先规范
- 样式、主题和 UI 框架使用规范
- 枚举常量和魔法值治理
- Electron `main`、`preload`、`renderer` 边界
- 工程化反思和验证要求

## Codex / Skill Hub

Codex 和 Skill Hub 使用以下入口：

```text
.codex-plugin/plugin.json
skills/frontend-rule/SKILL.md
skills/frontend-rule/agents/openai.yaml
```

使用示例：

```text
使用 $frontend-rule 检查并实现前端代码，确保目录、命名、枚举和工程化设计符合规范。
```

## Claude

Claude 使用以下项目 skill 入口：

```text
.claude/skills/frontend-rule/SKILL.md
```

该文件只作为 Claude 适配入口，核心规则仍以 `skills/frontend-rule/SKILL.md` 为准。

## Trae

Trae 使用以下规则入口：

```text
.trae/rules/frontend-rule.md
```

该文件只作为 Trae 适配入口，核心规则仍以 `skills/frontend-rule/SKILL.md` 为准。

## 维护原则

核心规则只维护在：

```text
skills/frontend-rule/SKILL.md
```

其他平台目录只放适配入口，避免多份规则内容不一致。

## License

MIT
