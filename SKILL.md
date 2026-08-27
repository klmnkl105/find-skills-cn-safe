---
name: find-skills-cn-safe
description: >-
  Find installable Agent Skills fast. Given a Chinese or English request,
  return only the most relevant candidates as: skill name, one-line what-it-does,
  and the exact install command. Verify basic usability and obvious malicious
  behavior before anything is installed. Installation only after the user
  reviews and approves. Do not recommend MCP servers, plugins, apps, agents,
  packages, or general GitHub projects as Skills.
---

# Find Skills CN Safe

快速精准地找到一个 Agent Skill。先定位，再阅读，用户同意后才安装。

## 只推荐可安装的 Agent Skill

候选必须有可定位的 Skill 目录、有效的 `SKILL.md` 和可识别的 `name` / `description`。仓库里偶然出现 `SKILL.md` 或名字带 skill 不算数。

MCP server、Plugin、App、Agent、提示词合集、软件包、普通开源项目不能进入推荐，搜索源返回这些时直接排除。

## 快速搜索（一次到位）

1. 看一眼当前已安装 Skill 的名称和描述，避免重复推荐。
2. 生成**一个主要英文查询**。只有中英文变体或同义词能明显改善结果时，才最多补一个查询。不要把用户的话拆成一堆词各搜一遍。
3. 按 `references/search.md` 选来源、串行搜索，**每个外部调用限时 15 秒，超时即跳过**。搜索只取元数据：名称、描述、来源、Skill 路径、安装标识。
4. 找到 2~5 个按匹配度排序的候选就停，不追求覆盖全部渠道。找到可安装的结果后立即停止扩大搜索。

## 推荐输出（只给有效信息）

每个候选只输出：

- **名称**——一行
- **作用**——一句话
- **安装命令**——准确的 Skills CLI 命令（多技能仓库用 `-s <skill名>` 指定，命令格式以 `skills --help` 现场确认的为准）

不要输出仓库网址、skill 路径、安装量、Stars、作者、官方标识等与使用无关的信息。

找不到可信结果就直接说明没有找到，不拿 MCP、Plugin 或普通项目凑数。

## 安装必须用户批准

1. 用户点名选定某个候选后，**先读它的 `SKILL.md`**，按 `references/trust.md` 做快速危险检查，并向用户说明该 Skill 会做什么、是否有额外依赖。
2. 用户明确同意后，用本机 Skills CLI 安装（命令先 `skills --help` 现场确认，不要凭记忆）。不自动安装新 CLI、系统组件或未说明的全局依赖。
3. 装完验证可发现：`skills list` 能看到该 Skill。

## 更新只处理用户点名的

用户要求检查某 Skill 更新时，只做只读比较；用户明确同意后才执行更新命令。来源不明、同名多源或本地有不明改动时停止并说明原因。

## 边界

- 默认只读：不安装、不更新、不运行任何候选脚本。
- 不下载远程代码直接执行；不用 Base64/压缩包/动态求值隐藏执行内容。
- 不把静态检查"无发现"说成安全保证。
