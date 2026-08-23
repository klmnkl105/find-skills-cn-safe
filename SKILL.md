---
name: find-skills-cn-safe
description: >-
  Quickly find, compare, install, and update third-party Agent Skills from
  Chinese or English requests. Search only for installable Agent Skills, check
  basic usability and obvious malicious behavior, and preserve source URLs for
  user-approved updates. Do not recommend MCP servers, plugins, apps, agents,
  packages, or general GitHub projects as Skills.
---

# Find Skills CN Safe

快速找到与用户任务匹配、可安装使用且没有明显危险行为的 Agent Skill。默认只搜索和推荐；安装与更新必须由用户明确批准。

## 只找 Agent Skills

候选必须有可定位的 Skill 根目录和有效 `SKILL.md`。仓库里偶然出现 `SKILL.md`，或名称中含有 skill，不足以证明它是可安装的 Agent Skill。

MCP server、Plugin、App、Agent、提示词合集、软件包和普通开源项目不能进入推荐结果。搜索源误返回这些内容时直接排除；只有用户询问原因时才简要说明。

## 默认立即搜索

只要用户表达了有意义的用途，就按原意理解并开始搜索，不要因为需求宽泛而先阻塞：

- “找毕业论文相关的 Skill”是可搜索需求。将其理解为论文研究、文献综述、引用、写作或排版等相邻用途，返回覆盖面不同的候选。
- “找论文的文献检索与综述 Skill”已经足够具体，直接搜索。
- 只有用户完全没有说明用途（例如只说“帮我找一个 Skill”），或两个解释会导向完全不同且不可兼容的结果时，才先问一个简短问题。

生成一个主要英文查询；只有相邻词或中英文变体可能明显改善结果时，再增加最多两个查询。不要把用户的每个词拆成大量独立搜索。

## 快速搜索流程

1. 快速检查当前已安装 Skill 的名称和描述，避免重复安装。
2. 外部搜索时读取 [references/search-sources.md](references/search-sources.md)，优先使用当前已有的 Skills CLI / skills.sh，并在结果不足时补充 GitHub 或一个可验证的中文来源。
3. 搜索阶段只获取名称、描述、来源网址、Skill 路径和安装标识；独立来源可以并行查询。
4. 用“规范仓库网址 + Skill 路径”去重，按实际任务匹配排序。默认给出一个主推荐和最多四个有明显用途差异的备选。
5. 对主推荐做快速可用性和明显危险检查；用户要求比较时，对实际进入比较的候选做同样检查。具体检查见 [references/trust-review.md](references/trust-review.md)。
6. 没有可信结果时说明没有找到，不要用 MCP、Plugin 或普通项目凑数。

不要默认调查许可证、作者背景、Stars、安装量排行榜或长期维护情况。它们不是“能否使用、是否有明显危险”的必要条件。搜索结果已经提供安装量等元数据时可以附带展示，但不得因此排除更匹配的 Skill。

## 推荐结果

保持简短，每个候选只需说明：

- 名称和解决的任务；
- 规范仓库网址与具体 Skill 路径；
- 当前环境下是“可直接使用”“需要额外依赖”还是“无法确认”；
- 是否发现明显危险；
- 准确的 Skills CLI 安装标识或命令。

“未发现明显危险”只代表本次静态检查没有命中明显问题，不是绝对安全保证。搜索、比较和推荐不等于授权安装。

## 安装并记录来源

用户明确选定候选并同意安装后：

1. 现场查看当前 Skills CLI 帮助，确认准确命令、Skill 标识、目标客户端和安装范围；
2. 使用 Skills CLI 安装，不自动安装新的 CLI、系统组件或未说明的全局依赖；
3. 验证 Codex 能发现该 Skill；
4. 检查 Skills CLI 的安装记录或锁文件，确认至少记录了来源网址、Skill 路径和内容哈希或版本；
5. 如果没有可靠记录，明确告诉用户该 Skill 无法按来源定向更新，不要假装已经追踪。

安装回执只需给出名称、实际路径、来源网址、Skill 路径和追踪状态。

## 用户确认后更新

只处理用户点名的 Skill，不批量更新：

1. “检查某 Skill 更新”只进行只读比较：从安装记录确定来源网址、Skill 路径和当前哈希或版本，查看上游变化，并按明显危险检查复核新增或修改内容。
2. 简短说明是否有更新、主要功能变化、新增依赖和新增危险行为。
3. 用户明确同意更新后，才使用当前 Skills CLI 的定向更新命令。
4. 更新后重新确认实际版本、可用性和来源记录。

来源记录缺失、目标同名但来源不唯一、或本地有无法解释的修改时，停止更新并说明原因。

## 边界

- 默认只读；不得未经同意安装或更新。
- 不执行候选脚本，不运行远程下载安装脚本，不自动改变系统配置。
- 不因正常联网、读取用户指定文件、写入任务输出或使用明确依赖而判定 Skill 危险。
- 不把静态扫描“无发现”说成安全保证。
