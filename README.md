# find-skills-cn-safe

快速、全面地找到最匹配的可安装 Agent Skill，支持 Claude Code 与 Codex 双平台。

## 能力

- **中文理解**：直接理解中文需求（"做 PPT 的 skill"、"找论文文献综述的 skill"），自动提取关键词与英文变体。
- **快速**：skills.sh 优先、GitHub 兜底，单主查询，每步 15 秒超时，命中即停。
- **少而准**：明确需求给 1~3 个候选，模糊需求给最多 5 个覆盖不同方向。
- **有效信息**：每条只含 名称 / 两三句话介绍 / 流行度（skills.sh 用真实 installs 数，GitHub 用 star 数）。
- **默认只读**：只搜索推荐，安装由用户决定。

## 文件

- `SKILL.md` — 主流程
- `references/search.md` — 关键词提取与搜索来源
- `references/trust.md` — 安装前快速危险检查
- `agents/openai.yaml` — Codex 兼容接口配置

## 安装

- **Claude Code**：复制整个目录到 `~/.claude/skills/find-skills-cn-safe/`
- **Codex**：复制整个目录到 `~/.codex/skills/find-skills-cn-safe/`

## 许可

MIT License
