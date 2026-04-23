# Study-System —— 一个与主题无关的引导式学习仓库

[English](./README.md)

这是一个极简、基于文件的学习系统，你可以把它用于**任何**学习主题——
考试、认证、语言、框架、教材——并把 AI 助手（Claude Code 等）变成一个有耐心、采用苏格拉底式教学法、且**能跨会话记住你学习进度**的学习教练。

灵感来自 [karpathy 的 gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)，
它展示了如何把 AI 用作学习伙伴。这个仓库最初源于 CFP 考试备考日志
（原学习者已于 2025 年 11 月通过考试 🎉），后来被重构成一个干净、
与主题无关的模板——克隆它，开始和你的编码代理对话，教练就会帮你把一切搭建好。

---

## 这个系统能做什么

无论你学习什么主题，教练都会：

- 🧠 **用苏格拉底式方式教学** —— 先问你已知内容，再用约 200 字的小块解释，随后检查理解，再决定是否继续。
- 📒 **记录每次学习会话** —— 每个学习日、每个主题都会生成一份详细的 `topics/<topic>/sessions/YYYY-MM-DD.md`。
- 📈 **为每个主题维护一份追踪器** —— `topics/<topic>/tracker.md` 是关于已掌握子主题、未解决知识漏洞和下一步学习内容的唯一最新事实来源。
- 🎯 **自动重新排序优先级** —— 领域权重 × 覆盖率 × 未解决漏洞 → 每次会话后都会更新下一步学习计划。
- 🔍 **给出引用或承认不确定性** —— 严格程度可按主题配置（strict / standard / relaxed）。
- 🔀 **同时管理多个主题** —— 每个主题都有自己的 `topics/<short-code>/` 目录；同一天上午学 CFP、下午学 AWS 会得到两份互不干扰的文件。
- 🔗 **记录跨主题洞见** —— 当两个主题之间出现真正的联系时，教练会把它作为 `crosslinks/` 下的一等文件保存，并从两边的 tracker 互相回链。

---

## 仓库结构

```
.active-topic                  ← 单行：当前 chat 默认指向的 <short-code>
                                 （首次初始化时由教练创建）

topic-config.template.md       ← 参考模板；为每个新主题复制到
                                 topics/<short-code>/topic-config.md
topic-config.md                ← 单主题旧布局的兼容占位（向后兼容）

INIT.md                        ← 添加新主题 + 日常更新流程
CLAUDE.md                      ← 教练行为（苏格拉底式、双步骤追踪、
                                 多主题、跨主题 crosslink）
AGENTS.md                      ← 给 Codex / opencode / 其他代理使用的
                                 同版说明

topics/                        ← 每个主题一个家
  README.md                    ← 解释每主题目录结构
  <short-code>/                ← 例如 cfp/、aws-saa/、linalg/
    topic-config.md            ← 这个主题是什么
    tracker.md                 ← 这个主题的进度
    sessions/
      YYYY-MM-DD.md            ← 每个学习日一份（每主题独立）

crosslinks/                    ← 跨主题洞见
  README.md
  INDEX.md                     ← 倒序索引
  CROSSLINK-TEMPLATE.md
  YYYY-MM-DD-<slug>.md         ← 每条洞见一个文件（按需创建）

progress/
  STUDY-TRACKER-TEMPLATE.md    ← 通用追踪器模板（教练据此为每个主题
                                 生成 tracker.md）

sessions/
  SESSION-TEMPLATE.md          ← 通用单次会话笔记模板
                                 （每个主题的每日笔记都基于此模板）
```

---

## 快速开始

1. **克隆**这个仓库（或把它作为模板使用）。
2. **在你选择的编码代理中打开它** —— Claude Code、Codex、opencode 等 —— 然后直接开始聊天。比如说：
   > "我想学习 X。把它作为一个新主题加进来。"

   教练会读取 `CLAUDE.md`（或 `AGENTS.md`），发现 `topics/` 还是空的，
   然后选择（或询问你）一个 `<short-code>`，并引导你填写
   `topics/<short-code>/topic-config.md`（主题名称、目标日期、
   领域与权重、学习材料、验证级别、个性化设置）。随后它会根据模板生成
   `topics/<short-code>/tracker.md`，把 `.active-topic` 写成新的
   `<short-code>`，并基于"权重最高且覆盖率最低"的领域建议你的第一次学习会话主题。
3. **持续提问即可。** 每次会话后，教练都会在
   `topics/<short-code>/sessions/YYYY-MM-DD.md` 写入笔记，并更新该主题的追踪器——无需手动记账。

> **提示：** 你也可以先手动从 `topic-config.template.md` 复制并填写
> `topics/<short-code>/topic-config.md`，然后再让教练"初始化追踪器"——
> 这两种流程都可以。

### 与 `/init` 的兼容性

大多数编码代理都有一个 `/init` 命令，用于为项目生成说明文件
（Claude Code 使用 `CLAUDE.md`，Codex 和 opencode 使用 `AGENTS.md`）。
这个仓库已经同时包含这两个文件，所以：

- 你**不需要运行 `/init`** —— 代理会自动读取现有说明。
- 如果你确实运行了 `/init`，请保留 `CLAUDE.md` / `AGENTS.md` 原内容
  （合并而不是覆盖），以确保学习教练行为不被破坏。

完整步骤请见 [`INIT.md`](./INIT.md)。

---

## 日常循环

| 何时 | 你做什么 | 教练会做什么 |
|------|----------|--------------|
| 会话开始 | 提问、请求练习，或说"我今天该学什么？" | 读取 `.active-topic`，再读对应主题的 `topic-config.md`、`tracker.md` 和最近一次 session 笔记来获取上下文 |
| 会话进行中 | 诚实回答理解检查问题——包括直接说"我不知道" | 用苏格拉底式方式教学；按配置要求的严格程度引用来源 |
| 会话中切换主题 | 说"switch to <short-code>" | 改写 `.active-topic`，明确宣告切换，重新加载新主题 |
| 会话结束 | 什么都不用做——直接结束即可 | 写入今天的 `topics/<short-code>/sessions/YYYY-MM-DD.md`，更新该主题的追踪器（掌握项 / 知识漏洞 / 下一步），如果产生跨主题洞见还会创建 `crosslinks/` 文件 |

会话之间你可以问的问题：
- "我今天该重点学什么？"
- "针对我的薄弱点考考我。"
- "展示一下我的进度。"
- "根据考试日期重新排优先级。"
- "切换到 <其他主题>。"
- "添加一个新主题：<名字>。"
- "把我所有的 crosslink 都列出来。"

---

## 多主题与跨主题洞见

这个仓库被设计成可以**并行承载多个主题**——一门考试、一个副业框架、
一门语言，全部在同一个 git 历史下。两个机制让这成为可能：

### 每个主题独立的"家"

每个主题完整地住在 `topics/<short-code>/` 下：

```
topics/cfp/topic-config.md
topics/cfp/tracker.md
topics/cfp/sessions/2026-04-23.md

topics/aws-saa/topic-config.md
topics/aws-saa/tracker.md
topics/aws-saa/sessions/2026-04-23.md
```

同一天学两个主题就是两份独立文件——不冲突，也不会让追踪器混乱。
仓库根目录的 `.active-topic` 文件保存一行（当前主题的 `<short-code>`），
教练借此知道每次 chat 默认指向哪个主题。切换只需一句话：
_"switch to aws-saa"_，教练会改写 `.active-topic` 并明确宣告切换。

### Crosslinks（跨主题洞见）

当两个主题突然"打通"了（"哦，AWS IAM 的这个想法和 CFP
信托权限根本是同一个结构"），教练会把这个瞬间作为
`crosslinks/` 下的一等文件保存：

- `crosslinks/INDEX.md` —— 全局倒序索引
- `crosslinks/YYYY-MM-DD-<slug>.md` —— 每条洞见一个文件，YAML
  front-matter 中显式列出参与的 `topics:` 与具体的子主题
  `links:`
- 每个参与主题的 `tracker.md` 都会在 **Cross-topic Links** 区块
  得到一行回链，从任一边都能找到它。

完整教练行为见 [`CLAUDE.md`](./CLAUDE.md) §9；文件约定见
[`crosslinks/README.md`](./crosslinks/README.md)。

---

## 添加 / 切换学习主题

随时想加新主题，直接在 chat 里说：

> "添加一个新主题：<名字>。"

教练会选择（或询问你）一个 `<short-code>`，创建
`topics/<short-code>/`，引导你填写 `topic-config.md`，生成 tracker，
并更新 `.active-topic`。

要切换下一条消息默认的主题：

> "切换到 <short-code>。"

要删除一个主题，`git rm -r topics/<short-code>/` 即可（如果它正好是
活跃主题，记得更新 `.active-topic`）。其他主题与全局
`crosslinks/INDEX.md` 不受影响；引用了被删主题的 crosslink 文件
作为历史记录留下，需要的话可以手动清理。

### 旧的单主题布局（向后兼容）

在多主题布局之前的旧仓库会把配置放在根目录的 `topic-config.md`，
追踪器放在 `progress/<short-code>-study-tracker.md`。这个布局**仍然有效**——
教练会自动检测并使用它。要迁移到新结构，请见
[`INIT.md`](./INIT.md) §3。

---

## 起源：CFP 考试案例

这个系统最初由原作者（chenran）在准备 **注册理财规划师（CFP）考试**
时构建并实战验证。在 23 次学习会话（2025 年 10 月 11 日至 11 月 7 日）
中，他们达到了 82% 的掌握率（73 个子主题中掌握了 60 个），并且**在 2025 年 11 月 10 日通过了 CFP 考试**。

与 CFP 相关的专用追踪器和会话笔记已经被移除，因此当前仓库以一个干净的模板形式发布；但工作流、文件结构和教练行为与那次成功实践中使用的完全一致。

联系原作者：[LinkedIn](https://linkedin.com/in/chenran818) ·
[Twitter / X](https://x.com/chenran818) ·
[知乎](https://www.zhihu.com/people/chenran)

如果你恰好也是用这个仓库来准备 CFP，这些免费资源与该工作流也很搭配：

- [Open Exam Prep Podcast](https://open.spotify.com/show/55EmWfdtPaK641q4Rk3mI1)
- [Open Exam Prep YouTube](https://www.youtube.com/@Open-exam-prep)
- [open-exam-prep.com](https://open-exam-prep.com/)
- [Financial Planning Essentials playlist](https://open.spotify.com/playlist/6GUIZvnpaiOiYmXkanqwZ8)

对于其他主题，请在 `topic-config.md` 第 4 节中列出你自己的学习材料——教练会优先引用它们。

---

## 理念

- **对话式且无评判。** 不知道也没关系。
- **建立在你已有知识之上。** 每次解释都从你的基础出发。
- **先确认理解，再继续推进。** 不会悄悄跳过。
- **适应学习者。** 使用不同的类比、例子和颗粒度。
- **深度理解 > 机械记忆。** 目标是掌握考试结束后仍然有用的能力。
