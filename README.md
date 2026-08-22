# my-skill

面向 AI 编码助手的可复用 Agent Skills 集合。每个技能都以 `skills/<skill-name>/SKILL.md` 为核心入口，并提供必要的平台适配文件。

## 技能列表

| Skill | 用途 | 核心入口 |
| --- | --- | --- |
| `frontend-rule` | 统一前端目录、命名、类型、接口、路由、样式、枚举和工程化规范 | `skills/frontend-rule/SKILL.md` |
| `ai-expert` | 面向程序员讲解、复习和巩固 AI 大模型知识 | `skills/ai-expert/SKILL.md` |

## 目录结构

```text
.
├── .claude/skills/          # Claude 项目 skill 适配入口
│   ├── ai-expert/
│   └── frontend-rule/
├── .codex-plugin/           # Codex 插件清单
├── .trae/rules/             # Trae 规则适配入口
├── skills/                  # 平台无关的核心技能目录
│   ├── ai-expert/
│   │   ├── agents/openai.yaml
│   │   └── SKILL.md
│   └── frontend-rule/
│       ├── agents/openai.yaml
│       └── SKILL.md
├── LICENSE
└── README.md
```

## 使用方式

Codex / Skill Hub / 魔搭平台使用 `skills/` 下的标准技能目录。每个技能至少包含带 YAML frontmatter 的 `SKILL.md`；`agents/openai.yaml` 提供 Codex 界面元数据。

```text
使用 $frontend-rule 检查并实现前端代码，确保目录、命名、枚举和工程化设计符合规范。

使用 $ai-expert 为有前端开发基础的学习者解释 RAG，并给出正例、反例和简短回顾。
```

Claude 和 Trae 的文件只负责平台发现与转发，核心内容不重复维护：

- Claude：`.claude/skills/<skill-name>/SKILL.md`
- Trae：`.trae/rules/<skill-name>.md`
- 核心规则：`skills/<skill-name>/SKILL.md`

如果适配入口与核心规则冲突，以核心 `SKILL.md` 为准。

## 发布

- GitHub：发布整个仓库，保留根目录的许可证、说明文件和平台适配目录。
- 魔搭 Skills：选择需要发布的单个 `skills/<skill-name>/` 目录作为技能包；入口文件必须是该目录下的 `SKILL.md`。
- 发布前不要把日志、构建产物、编辑器临时文件或密钥放入技能目录。

## License

MIT
