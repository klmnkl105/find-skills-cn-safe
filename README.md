# find-skills-cn

快速、全面地找到最匹配的可安装 Agent Skill，支持 Claude Code 与 Codex 双平台。

## 能力

- **中文理解**：直接理解中文需求（"做 PPT 的 skill"、"找论文文献综述的 skill"），自动提取关键词与英文变体。
- **快速**：skills.sh 优先、GitHub 兜底、Anthropic 官方技能全览作补充，单主查询，每步 15 秒超时，命中即停。
- **少而准**：明确需求给 3 个候选（第 1 个主推），模糊需求给最多 5 个覆盖不同方向。
- **匹配度优先**：按与需求的贴合程度排序；installs / star 只在匹配度相近时作参考打破平局，**不唯热度**。
- **有效信息**：每条含 名称 / 详细介绍（功能、用法、场景、依赖）/ 流行度（installs 或 star 数）/ **可点击链接**（GitHub 仓库主页）。
- **排版清晰**：候选块用横向分隔线隔开，名称加粗、介绍用引用块、元数据与链接分行列出，不堆砌。
- **只做搜索输出**：不做安装、不做安全检查、不执行任何候选脚本，是否安装由用户自己决定。

## 文件

- `SKILL.md` — 主流程
- `references/search.md` — 关键词提取与多源搜索
- `agents/openai.yaml` — Codex 兼容接口配置

## 安装

- **Claude Code**：复制整个目录到 `~/.claude/skills/find-skills-cn/`
- **Codex**：复制整个目录到 `~/.codex/skills/find-skills-cn/`
- **其他 agent**：需要时复制一份到对应 skills 目录即可

## 许可

MIT License
