# find-skills-cn

快速、全面地找到最匹配的可安装 Agent Skill，支持 Claude Code 与 Codex 双平台。

## 能力

- **中文理解**：直接理解中文需求（"做 PPT 的 skill"、"找论文文献综述的 skill"），自动提取关键词与英文变体。
- **快速**：skills.sh 优先、SkillsMP 补充、GitHub 兜底，单主查询，每步 15 秒超时，命中即停。
- **少而准**：明确需求给 3 个候选（第 1 个主推），模糊需求给最多 5 个覆盖不同方向。
- **热门排序**：结果按 installs（skills.sh）或 star（SkillsMP/GitHub）降序，先展示最热门的。
- **有效信息**：每条只含 名称 / 两三句话介绍 / 流行度（installs 或 star 数）。
- **默认只读**：只搜索推荐，安装由用户决定。不读任何 agent 的本地已安装目录。

## 文件

- `SKILL.md` — 主流程
- `references/search.md` — 关键词提取与三源搜索
- `references/trust.md` — 安装前快速危险检查
- `agents/openai.yaml` — Codex 兼容接口配置

## 安装

- **Claude Code**：复制整个目录到 `~/.claude/skills/find-skills-cn/`
- **Codex**：复制整个目录到 `~/.codex/skills/find-skills-cn/`
- **其他 agent**：需要时复制一份到对应 skills 目录即可

## 许可

MIT License
