# find-skills-cn-safe

快速精准地找到一个可安装的 Agent Skill：先定位，再阅读，用户同意后才安装。

## 定位

- 只推荐可安装的 Agent Skill（有 Skill 目录、有效 `SKILL.md`、可识别 `name`/`description`）。
- 排除 MCP server、Plugin、App、Agent、提示词合集、软件包和普通项目。

## 用法

在支持 Skill 的 Agent 里直接描述需求，例如：

> 帮我找一个做论文文献综述的 Skill。
> 有没有做 PPT 的 Skill？

## 设计原则

- **快**：单个主查询，串行搜索，每步 15 秒超时，命中即停；不追求覆盖全渠道。
- **只给有效信息**：每条推荐只含 名称 / 一句话作用 / 安装命令。不输出仓库网址、skill 路径、安装量、Stars。
- **安全**：默认只读。安装前先读候选 `SKILL.md` 做明显危险检查，用户明确批准后才安装。
- **真实命令**：安装命令以本机 `skills --help` 现场确认为准，不凭记忆写。

## 文件

- `SKILL.md` —— 主流程
- `references/search.md` —— 搜索来源与超时规则
- `references/trust.md` —— 安装前的快速危险检查

## 许可

MIT License
