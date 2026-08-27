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
   原始结果已按 installs 降序，但**只当参考排序**，展示顺序以匹配度为准。
   **注意：** 可能返回多个同名 skill（不同来源仓库）。按匹配度挑最合适的保留，`source` 相同时合并；其余同名的用 `source` 区分，避免混淆。

2. **GitHub 仓库搜索**（第 1 步不足时兜底）：
   `curl -s --max-time 15 "https://api.github.com/search/repositories?q=<关键词>+claude-skill&per_page=10"`
   返回 `full_name`、`description`、`stargazers_count`、`html_url`。
   原始结果已按 star 降序，但**只当参考**。只看能定位到具体 `SKILL.md` 的仓库（README 里通常有 `--skill` 安装说明）。

网络不通时说明"外部搜索暂不可用，只能基于已安装 Skill 推荐"。

### 官方渠道说明（2026-08-27 验证）

Anthropic 官方技能集中在 **`anthropics/skills`** 仓库（GitHub ⭐ 44.3k，18 个文档/示例技能，含 pptx/pdf/xlsx/docx 等）：
- 它是标准 Plugin marketplace（`.claude-plugin/marketplace.json`），可 `git clone` 后按需取用。
- 官方 README **只有技能列表、没有描述**，无法做关键词匹配 → 不作为自动搜索源。
- 官方技能已通过 **skills.sh 收录**（搜索结果含 `anthropics/skills` 来源，installs 最高），主源命中即覆盖。
- 使用方式：官方技能列表见 `https://github.com/anthropics/skills/tree/main/skills`，逐个技能读 `SKILL.md` 确认。

需要"官方技能全览"或"主源未命中官方技能"时，用上述方式兜底，并在推荐中标注「官方」。不要因为非官方来源搜不到就跳过官方。

## 去重与排序

- 按"来源仓库 + Skill 名"去重：同一 skill 合并，介绍取信息最全的那条。
- **排序以匹配度为准**：名字/描述与需求贴合程度、是否解决用户核心问题 → 最贴合的排最前。
- 热度（installs / star）只作为**参考维度**，不决定顺序：匹配度接近时，才用热度打破平局（热度高者在前）。
- 描述只有模板话术、看不出具体功能的，排后面或直接不展示。

## 流行度指标（双指标，互相印证）

**installs（skills.sh）与 star（GitHub）是两个不同性质的指标，必须同时展示**：

- installs 是 skills.sh 平台的安装统计，可能受平台推广、自动安装工具影响，不直接等于社区认可度；
- star 是 GitHub 社区的客观认可度，但仓库刚建时天然很低，也不等于质量差；
- **两者反差悬殊时如实标注**：如 installs 数万但 star 接近 0 → 注明「⭐ 很低，热度存疑」；如 star 很高但 installs 很低 → 注明「安装量低，可能较新或未进平台」。
- 拿不到某一边就不写，绝不编造；两边都拿得到就都写。

## 结果链接

**每条推荐必须附上可点击的具体链接**（可直接复制进 `[文字](链接)` 或作为 bare URL）。链接和元数据在搜索阶段一并拿到，不额外发请求：

| 来源 | 取链接方式 | 链接到哪 |
|------|-----------|----------|
| skills.sh | 直接用返回的 `source` 字段 → `https://github.com/<source>`；source 形如 `repo/skill` 时去掉末尾 skill 段 | 仓库主页，用于核对作者与 README |
| GitHub | 直接用 `html_url` 字段 | 仓库主页（自带的 `html_url` 就是可点击链接） |

找不到链接字段时，用 skill 名回退到 GitHub 搜索首页（`https://github.com/search?q=<name>`），但要如实标注"链接为搜索结果，非仓库主页"。链接来自搜索元数据、未经安装校验，不作为安全承诺。

## 候选资格

必须有可定位的 Skill 目录、有效 `SKILL.md`、可识别 `name`/`description`。MCP server、Plugin、App、Agent、包、模板集、普通仓库即使被返回也排除；多技能仓库记录具体 Skill 名，不把整个仓库当一个 Skill。
