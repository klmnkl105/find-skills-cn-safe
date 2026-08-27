# 搜索来源

只在外出搜索时读这份文件。目标是在 15 秒内快速定位可安装的 Agent Skill，不是调查开源生态。

## 来源顺序（串行，命中即停）

1. **当前已安装的 Skill**：先看本地 `~/.claude/skills/` 有没有，避免重复推荐。
2. **skills.sh 公共 API**：主来源。
   - 命令：`curl -s --max-time 15 "https://skills.sh/api/search?q=<英文查询>&limit=10"`
   - 返回字段：`name`、`installs`（真实安装量）、`source`（来源仓库）、`skillId`。`installs` 直接作为流行度展示（约数，如 `667k`）。
3. **GitHub 仓库搜索**（第 2 步无结果时）：
   - 命令：`curl -s --max-time 15 "https://api.github.com/search/repositories?q=<查询>+claude-skill&per_page=10"`
   - 用 `stargazers_count` 作为流行度（`⭐ 5.8k`）。只看能定位到具体 `SKILL.md` 的仓库，多技能仓库必须记录具体 skill 路径。

独立来源不要并行、不要多查几页。主查询命中 2~5 个候选就停。

## 每个来源 15 秒超时

`--max-time 15` 必须带上。超时的来源直接跳过，不重试，不因此拖慢整体。网络不通时，直接说明"外部搜索暂时不可用，只能基于本地已安装的 Skill 推荐"。

## 候选必须满足

- 能定位到具体 Skill 目录；
- 目录里有可读的 `SKILL.md`；
- frontmatter 能识别 `name` 和 `description`；
- 有适用于本机 Skills CLI 的安装标识。

MCP server、Plugin、App、Agent、包、模板集、普通仓库即使被返回也要排除。多技能仓库必须记录具体 Skill 名，不能把整个仓库当一个 Skill。
