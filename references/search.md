# 搜索与关键词

目标：快速定位可安装的 Agent Skill。优先质量，不追求覆盖所有来源。

## 关键词提取

从用户原话提取 1~3 个关键查询词：

- 核心技术/任务词保留原文（转英文，如 "后端"→"backend"、"可视化"→"visualization"）；
- 需要时补一个中英文变体或同义词；不要把一个需求拆成很多个独立搜索；
- 中文直接理解，无需用户解释。

## 来源顺序（串行，命中即停）

每个来源限时 15 秒，超时跳过不重试。不读任何 agent 的本地已安装目录。

1. **skills.sh**（主来源）：
   `curl -s --max-time 15 "https://skills.sh/api/search?q=<关键词>&limit=10"`
   返回 `name`、`installs`（真实安装量）、`source`（仓库）、`description`。
   结果按 `installs` **降序**排——installs 最高的排最前，等于热门优先。

2. **SkillsMP**（聚合站，第 1 步结果不足时）：
   `curl -s --max-time 15 "https://skillsmp.com/api/skills?q=<关键词>&limit=10"`
   返回 `name`、`description`、`stars`、`githubUrl`、`path`。
   结果按 `stars` **降序**排。

3. **GitHub 仓库搜索**（第 2 步仍不足时）：
   `curl -s --max-time 15 "https://api.github.com/search/repositories?q=<关键词>+claude-skill&per_page=10"`
   返回 `full_name`、`description`、`stargazers_count`。
   结果按 `stargazers_count` **降序**排。只看能定位到具体 `SKILL.md` 的仓库。

网络不通时说明"外部搜索暂不可用，只能基于已安装 Skill 推荐"。

## 去重与排序

- 按"来源仓库 + Skill 名"去重，不同来源的同一 skill 合并，优先保留 installs 高的那条。
- 展示顺序按流行度降序：先 installs 高的，其次 stars 高的。

## 候选资格

必须有可定位的 Skill 目录、有效 `SKILL.md`、可识别 `name`/`description`。MCP server、Plugin、App、Agent、包、模板集、普通仓库即使被返回也排除；多技能仓库记录具体 Skill 名，不把整个仓库当一个 Skill。
