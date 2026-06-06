# MeteorDreamWiki

*[English](./README.md) · 中文*

一个接入了 [`Ar9av/obsidian-wiki`](https://github.com/Ar9av/obsidian-wiki) 技能集（skill collection）的 [Obsidian](https://obsidian.md) 知识库 —— 由 Claude Code（以及其他 AI Agent）持续摄入你的笔记、对话与原始资料，**蒸馏并整合到一张相互链接的 wiki 中**，而不是让模型在每次提问时从零开始重新发现知识。

仓库根目录本身就是 Obsidian 仓库（vault）。用 Obsidian 打开这个文件夹即可。

本项目是 [Andrej Karpathy 的 LLM Wiki 模式](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 的一个具体实现。该模式的奠基性论述已经被摄入到本 vault —— 见 [[concepts/llm-wiki-pattern]] 与 [[references/karpathy-llm-wiki-essay]]。

> **关于上游框架。** `.claude/skills/`、`.hermes/skills/`、`.agents/skills/` 下的技能（skills）来自 [`Ar9av/obsidian-wiki`](https://github.com/Ar9av/obsidian-wiki) —— *"一套让 AI 编码 Agent 能用 Obsidian 做查看器、用 LLM 做维护者，构建并维护个人知识管理系统的框架。"* 该框架与具体 Agent 解耦：它为 Claude Code、Cursor、Windsurf、Codex、Gemini CLI、Pi、Kiro、Hermes、OpenClaw、GitHub Copilot 等都提供了 skills。**MeteorDreamWiki** 是基于这个框架搭建的一个具体 vault。

> **关于上游框架。** `.claude/skills/`、`.hermes/skills/`、`.agents/skills/` 下的技能（skills）来自 [`Ar9av/obsidian-wiki`](https://github.com/Ar9av/obsidian-wiki) —— *"一套让 AI 编码 Agent 能用 Obsidian 做查看器、用 LLM 做维护者，构建并维护个人知识管理系统的框架。"* 该框架与具体 Agent 解耦：它为 Claude Code、Cursor、Windsurf、Codex、Gemini CLI、Pi、Kiro、Hermes、OpenClaw、GitHub Copilot 等都提供了 skills。**MeteorDreamWiki** 是基于这个框架搭建的一个具体 vault。

---

## LLM Wiki 方法论

> Obsidian 是 IDE，LLM 是程序员，wiki 才是源代码。 — Andrej Karpathy

大多数 LLM + 文档的工作流程都长得像 RAG：上传一堆文件，查询时检索相关片段，再交给模型生成答案。这套机制能用，但**模型每次都得从零开始重新发现知识**。什么都没有沉淀下来：跨文档的洞见、内部矛盾、推论链路 —— 每次提问都得重新推导一遍。

LLM Wiki 模式把这种"综合"工作**前移到摄入阶段**。LLM **持续地构建并维护一个长期存在的 wiki** —— 一组结构化、相互链接的 Markdown 文件，夹在你和原始资料之间。当一份新资料进来时，模型不只是把它索引起来供以后查询；它会**阅读**这份资料，**抽取**有价值的内容，并**整合**进现有的 wiki —— 更新实体页、改写摘要、标注矛盾、加强综合判断。知识被**"编译"一次，然后保持新鲜**，而不是在每次查询时被重新推导。

### 三个层次，三个所有者

```
                                      schema（架构层）
                                  （技能 + .env）
                                         │
                                  约束 LLM 的行为
                                         │
                                         ▼
   raw sources       ──读取──►          LLM         ──写入──►        wiki
  （原始资料）                 （技能/Agent）            （Markdown 页面）

   你来策展                     全程在循环中            LLM 拥有，你阅读
   不可变                       Ingest / Query / Lint   链接、图谱、frontmatter
```

| 层 | 所有者 | 是什么 | 在本 vault 中对应 |
|---|---|---|---|
| **原始资料（Raw sources）** | 你 | 文章、论文、Agent 对话记录、网页剪藏 —— 不可变，是真理之源 | `_raw/`、`~/.claude`、`~/.codex`、`~/.hermes`，加上 `OBSIDIAN_SOURCES_DIR` 里指向的任何目录 |
| **Wiki 本身** | LLM | 蒸馏过的、互相链接的 Markdown 页面，带 frontmatter、wikilinks 和持续维护的图谱 | `concepts/`、`entities/`、`skills/`、`references/`、`synthesis/`、`journal/`、`projects/` |
| **架构（Schema）** | 你和 LLM 共同进化 | 把一个 LLM 从"通用聊天机器人"变成"有纪律的 wiki 维护者"的那套约定 | `.claude/skills/`、`.hermes/skills/`、frontmatter 规则、标签分类、`.env` |

### 三个核心操作

这三个原语就够用了。本仓库里的每一个 skill，都只是其中之一的细化。

1. **Ingest（摄入）** —— 把一份资料丢进 `_raw/`（或者把 `OBSIDIAN_SOURCES_DIR` 指过去）。LLM 会阅读它、写一份摘要页、更新索引、刷新 10–15 个相关页面，并在日志里追加一条记录。*一份资料应当波及多个页面 —— 这就是关键。*
2. **Query（查询）** —— 提问。LLM 先读 `index.md`，定位到相关页面，再深入阅读，给出带引用的答案。**好的回答应当被回填进 wiki 成为新页面**，这样**探索本身也会复利**，而不只是摄入会复利。
3. **Lint（健康检查）** —— 周期性地审视 wiki：页面之间的矛盾、被新资料推翻的旧论断、没有任何反向链接的孤儿页、被反复提及但从未单列页面的概念、缺失的交叉引用。LLM 还会建议**新的问题和资料**来填补空白。

#### 摄入的内部分阶段（上游框架的四步划分）

`wiki-ingest` 把"摄入"显式拆成 4 个阶段，这是上游框架文档化的执行模型：

1. **Ingest（读入）** —— 直接读取原始资料：Markdown、PDF、聊天导出、图片（仅在视觉模型可用时）。
2. **Pull Information（信息抽取）** —— 抽取概念、实体、声明、关系。每条声明在抽取的当下就标注 *extracted*（原文）/ *inferred*（推断）/ *ambiguous*（存疑）。
3. **Merge（合并）** —— 把新知识**整合**进既有页面，而不是替换。更新摘要、补 wikilinks、记录来源。一次摄入触达 10–15 个页面。
4. **Schema（架构维护）** —— 在图谱演化过程中持续维护 frontmatter、标签分类与关系类型的一致性。这是上百页规模下防止漂移的关键。

`.manifest.json` 这个台账记录了每一份曾经被摄入过的资料（路径、内容哈希、修改时间、产出的页面），以便后续运行时**只处理真正变化的部分** —— 这就是**增量追踪（delta tracking）**，也是 append 模式的摄入即使在数千份资料的 vault 上也依然快的原因。

#### 摄入的内部分阶段（上游框架的四步划分）

`wiki-ingest` 把"摄入"显式拆成 4 个阶段，这是上游框架文档化的执行模型：

1. **Ingest（读入）** —— 直接读取原始资料：Markdown、PDF、聊天导出、图片（仅在视觉模型可用时）。
2. **Pull Information（信息抽取）** —— 抽取概念、实体、声明、关系。每条声明在抽取的当下就标注 *extracted*（原文）/ *inferred*（推断）/ *ambiguous*（存疑）。
3. **Merge（合并）** —— 把新知识**整合**进既有页面，而不是替换。更新摘要、补 wikilinks、记录来源。一次摄入触达 10–15 个页面。
4. **Schema（架构维护）** —— 在图谱演化过程中持续维护 frontmatter、标签分类与关系类型的一致性。这是上百页规模下防止漂移的关键。

`.manifest.json` 这个台账记录了每一份曾经被摄入过的资料（路径、内容哈希、修改时间、产出的页面），以便后续运行时**只处理真正变化的部分** —— 这就是**增量追踪（delta tracking）**，也是 append 模式的摄入即使在数千份资料的 vault 上也依然快的原因。

### 两个特殊文件让它可被导航

- **`index.md`** —— **面向内容**。所有页面的目录，每条带一句摘要，按类别组织。LLM 在回答查询时**先读 index**，再下钻到具体页面。在几百页的规模下，这个机制已经够用，无需引入基于嵌入的 RAG 基础设施。
- **`log.md`** —— **面向时间**。仅追加（append-only）的操作记录：每次摄入、查询、lint。每条记录都有可解析的固定前缀，简单 grep 就能拉出时间线。

本 vault 还额外引入了两个：`hot.md`（约 500 字的语义快照，包含近期活动、关键洞见、活跃话题）以及 `.manifest.json`（机器可读的台账，让增量摄入跳过已经处理过的资料）。

### 它为什么真的有效

维护一个知识库，乏味的部分**不是阅读和思考，而是簿记**：更新交叉引用、保持摘要新鲜、追踪矛盾、在几十个页面间维持一致性。人类放弃 wiki，是因为维护成本增长得比价值还快。

LLM 不会厌倦、不会忘记更新一个反向链接，而且能在**一次操作中触达 15 个文件**。**Wiki 之所以能持续被维护，是因为维护成本趋近于零。**

你负责策展资料、提出好问题、思考其中的含义。LLM 负责其余一切。

这一模式其实在精神上呼应 Vannevar Bush 1945 年的 [Memex](https://en.wikipedia.org/wiki/Memex) 设想 —— 一个私人、人工策展、文档之间带"关联轨迹"的知识装置。Bush 找到了正确的**形式**；LLM 终于解决了**人力**这一关。

---

## 仓库目录结构

| 路径 | 用途 |
|---|---|
| `concepts/` | 蒸馏出的概念、理论、心智模型 |
| `entities/` | 人物、产品、库、组织 |
| `skills/` | 操作类知识、流程类知识 |
| `references/` | 外部资料、URL、引文 |
| `synthesis/` | 跨多个概念的综合页 |
| `journal/` | 时间锚定的笔记 |
| `projects/` | 按项目组织的知识（在摄入过程中自动生成） |
| `_raw/` | 把新资料丢这里；`wiki-ingest` 会处理并删除原文件 |
| `_staging/` | 当 `WIKI_STAGED_WRITES=true` 时，AI 写入的页面会先进入这里供你审核 |
| `_archives/` | 重建/恢复操作的快照存档 |
| `index.md` | 自动维护的全 wiki 索引 |
| `log.md` | 仅追加的时间线记录 |
| `hot.md` | 约 500 字的近期活动语义快照 |
| `.manifest.json` | 簿记：哪些资料已经摄入过，附带内容哈希 |
| `.obsidian/` | Obsidian 应用配置（共享） |
| `.claude/skills/` | Claude Code 加载的 skill 定义 |
| `.hermes/skills/` | Hermes 加载的 skill 定义 |
| `skills-lock.json` | 从 `Ar9av/obsidian-wiki` 锁定的 skill 版本 |
| `.env` | Vault 配置（路径、历史源、阈值） |

---

## 上手指南

### 0. （仅当从零开始搭建一个新 vault 时）安装框架

本仓库已经配置好了，可以直接跳到第 1 步。如果你想从零搭一个**新的 vault**，上游提供三种安装方式：

```bash
# 方式 A —— pip 安装（最适合一般用户）
pip install obsidian-wiki
obsidian-wiki setup --vault /path/to/vault

# 方式 B —— Skills CLI（把 skills 安装进既有项目）
npx skills add Ar9av/obsidian-wiki

# 方式 C —— git clone（直接跟随上游变动）
git clone https://github.com/Ar9av/obsidian-wiki
cd obsidian-wiki && bash setup.sh
```

安装完成后，在你的 AI Agent 里运行 `/wiki-setup` 来生成 vault 的目录结构（分类、特殊文件、`.env`）。

### 1. 用 Obsidian 打开 vault

`File → Open Vault → ` *（选中本仓库根目录）*

### 2. （可选）安装社区插件

为了获得完整体验，推荐：

- **Dataview** —— 查询 frontmatter，构建动态表格。规模上来后必备。
- **Graph Analysis** —— 在内置图谱视图之上提供更丰富的结构指标
- **Templater** —— 当你偶尔想手写页面时使用模板
- **Obsidian Git** —— 自动把 vault 备份到 git 仓库
- **Web Clipper** —— 浏览器 → Markdown 直接落到 `_raw/`，是最快的资料喂入方式

### 3. 摄入第一批资料

在 Claude Code 中运行：

```
/wiki-status     # 看看有什么待处理
/wiki-ingest     # 把资料蒸馏成 wiki 页面
```

挖掘过去的 AI 对话历史：

```
/claude-history-ingest    # ~/.claude 会话
/codex-history-ingest     # ~/.codex 会话
/hermes-history-ingest    # ~/.hermes 会话
```

### 4. 提问，并让答案复利

```
/wiki-query           # 基于本 vault 给出带引用的答案
/wiki-research <X>    # 自主多轮联网研究，结果回填到 wiki
/wiki-synthesize      # 找到反复共现、需要单独综合页的概念
```

### 5. 维持图谱的健康

```
/wiki-lint              # 找孤儿页、坏链接、矛盾
/cross-linker           # 自动补齐缺失的 wikilinks
/wiki-status --insights # 中心节点、桥接页、松散标签簇
/daily-update           # 适合 cron 的日常刷新
```

---

## Agent 兼容性

上游框架为多种 AI 编码 Agent 都准备了 skill 包，它们读取的是**同一个 vault 格式**。**MeteorDreamWiki 当前提交了其中三家的 skill 文件**（`.claude/`、`.hermes/`、`.agents/`），其它的可以按需加上。

| Agent | Skill 发现路径 | 本仓库状态 |
|---|---|---|
| Claude Code | `.claude/skills/` | ✅ 已提交 |
| Hermes | `.hermes/skills/` | ✅ 已提交 |
| Cursor / Windsurf / 通用 | `.agents/skills/` | ✅ 已提交 |
| Codex | `~/.codex`（仅历史挖掘） | 已配置历史路径 |
| GitHub Copilot CLI | `~/.copilot/` | 用 `/copilot-history-ingest` 挖掘 |
| Pi (OpenCode) | `~/.pi/agent/sessions/` | 用 `/pi-history-ingest` 挖掘 |
| OpenClaw | `~/.openclaw/` | 用 `/openclaw-history-ingest` 挖掘 |
| Gemini CLI / Kiro | 各 Agent 各自路径 | 本 vault 暂未接入 |

vault 里的所有内容（每一个 `.md` 页面、index、log、manifest）都是**纯 Markdown**，与具体 Agent 无关。换 Agent 只是换加载哪一套 skill 文件。

---

## 配置（`.env`）

| 变量 | 用途 |
|---|---|
| `OBSIDIAN_VAULT_PATH` | Vault 根目录。`.` 表示"就是这个仓库"。 |
| `OBSIDIAN_SOURCES_DIR` | 逗号分隔的目录列表，`wiki-ingest` 会扫描它们。**默认 `_raw`** —— 资料放那里。**不要**设成 `.`，否则 skill 文件会被当作资料处理。 |
| `CLAUDE_HISTORY_PATH` | `~/.claude` —— 启用 `claude-history-ingest` |
| `CODEX_HISTORY_PATH` | `~/.codex` —— 启用 `codex-history-ingest` |
| `HERMES_HISTORY_PATH` | `~/.hermes` —— 启用 `hermes-history-ingest` |
| `WIKI_TOKEN_WARN_THRESHOLD` | 当全 wiki 的总 token 超过该值时给出警告（默认 `100000`，设 `0` 禁用） |
| `WIKI_STAGED_WRITES` | 设为 `true` 时，所有由 AI 写入的页面先进 `_staging/` 等你审核 |
| `QMD_*` | 可选 —— 启用 [QMD](https://github.com/tobi/qmd) 作为语义搜索后端；未配置时回落到 grep |

---

## Skill 目录

`.claude/skills/` 下打包了约 38 个 skill。每一个都是 Ingest / Query / Lint 之一的细化版本。

### 摄入类（Ingestion）
- **`wiki-ingest`** —— 从 `OBSIDIAN_SOURCES_DIR` 把文档蒸馏成 wiki 页面
- **`data-ingest`** —— 万能兜底：原始文本转储、ChatGPT 导出、各种对话记录
- **`ingest-url`** —— 抓取一个 URL 并写成 wiki 页面
- **`claude-history-ingest`** / **`codex-history-ingest`** / **`hermes-history-ingest`** / **`pi-history-ingest`** / **`copilot-history-ingest`** / **`openclaw-history-ingest`** —— 各家 Agent 的对话挖掘器
- **`wiki-history-ingest`** —— 上面这些的统一调度入口
- **`wiki-quick-chat-capture`** —— 60 秒内把当前对话快速落到 `_raw/`
- **`wiki-capture`** —— 把当前对话固化成结构化 wiki 页面

### 查询与探索（Query）
- **`wiki-query`** —— 基于 vault 回答问题，带引用
- **`wiki-agent`** —— 查询**某个特定 Agent** 的历史（"我在 Codex 里关于 X 做过什么？"）
- **`memory-bridge`** —— 在不同 AI 工具的记忆之间做差异比对，暴露盲点
- **`wiki-context-pack`** —— 为下游任务打包一份 token 预算受控的上下文
- **`wiki-research`** —— 自主多轮联网研究，结果回填到 vault

### 维护类（Lint 操作）
- **`wiki-status`** —— 待摄入的资料、token 占用、中心节点/桥接页洞察
- **`wiki-lint`** —— 孤儿页、坏 wikilinks、矛盾；`--consolidate` 进入"既报告又修复"模式
- **`wiki-dedup`** —— 合并同一概念但不同命名的页面
- **`cross-linker`** —— 发现并自动补齐缺失的 wikilinks
- **`wiki-synthesize`** —— 从概念共现模式中提议新的综合页
- **`tag-taxonomy`** —— 维持受控的标签分类
- **`daily-update`** —— 适合 cron 的日常刷新流程
- **`wiki-digest`** —— 人类可读的每周/每月知识摘要

### 生命周期（Lifecycle）
- **`wiki-setup`** —— 首次初始化 vault（本仓库已经跑过）
- **`wiki-rebuild`** —— 归档并从原始资料完整重建
- **`wiki-export`** / **`wiki-import`** —— graph.json、GraphML、Cypher、可交互 HTML
- **`wiki-stage-commit`** —— 当 `WIKI_STAGED_WRITES=true` 时，审阅并提升 staged 页面
- **`wiki-switch`** —— 在多个具名 vault profile 之间切换
- **`wiki-update`** —— 从任何项目把当前知识同步回这个 vault

在 Claude Code 中以 `/<skill-name>` 的形式调用。

### 两个跨项目的"全局" skill

有两个 skill 设计成**跨项目可用**的，不只在 vault 内才行 —— 它们是任何项目和你的知识库之间的桥梁：

- **`/wiki-update`** —— **推送（push）。** 在任意项目目录里，把你最近做的事蒸馏回 vault。skill 会自己判断该落到哪个项目桶。
- **`/wiki-query`** —— **拉取（pull）。** 在任意项目目录里，提问，获得一条由 wiki 拼装的、带引用的答案。**不需要切换到 vault 目录**。

二者结合，让 vault 成为你跨项目的长期记忆 —— 不管你当前在哪个项目里，都可以读、可以写。

---

## 约定

- **你来策展，LLM 来写。** 不要轻易手改页面 —— 让 skill 维持一致性。
- **frontmatter 必填。** 每个页面都有 `title`、`tags`、`summary`、`sources`、`base_confidence`、`lifecycle`、`tier`、`provenance`。详见 `tag-taxonomy`。
- **链接优先于平文。** `[[概念名]]` 才是构筑图谱的东西。
- **每条声明都标记来源。** `^[extracted]`（资料原文）、`^[inferred]`（推断/综合）、`^[ambiguous]`（资料之间冲突）。Wiki 的价值取决于你能区分**信号**与**综合**。
- **`_raw/` 是一次性的。** `wiki-ingest` 成功摄入后会**删除原始草稿**。
- **`hot.md` 会被频繁重写。** 不要手改，让 skill 维护它。
- **Wiki 就是一个 git 仓库。** 你免费获得版本历史、分支、协作。

---

## 为什么值得花这点功夫

大多数个人知识库死掉，是因为簿记成本超过了价值。这一个不会死，因为簿记的人是 LLM。**你积累的是一张图谱，不是一堆文件** —— 这张图会随着每一份新资料、每一次提问而越来越密。

Wiki 永远是你的，AI 来了又走。

---

## 许可

本项目以 **[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International（CC BY-NC-SA 4.0）](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)** 许可发布 —— 完整文本见 [LICENSE](./LICENSE)。

简单来说：你可以自由分享和改编内容，但需要署名、仅限非商业用途，且任何衍生作品必须使用同一许可。

`.claude/skills/`、`.hermes/skills/`、`.agents/skills/` 下的 skill 集来自 [`Ar9av/obsidian-wiki`](https://github.com/Ar9av/obsidian-wiki)，遵循该上游仓库的许可 —— 锁定版本见 `skills-lock.json`。

LLM Wiki 方法论由 Andrej Karpathy 在[原始 gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 中提出，本 vault 已将其摄入为 [[references/karpathy-llm-wiki-essay]]。该模式呼应了 [Vannevar Bush 1945 年的 Memex](https://en.wikipedia.org/wiki/Memex)（[[references/memex]]）。
