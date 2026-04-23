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
- 📒 **记录每次学习会话** —— 每个学习日都会生成一份详细的 `session-notes.md`。
- 📈 **维护唯一追踪器** —— `progress/<topic>-study-tracker.md` 是关于已掌握子主题、未解决知识漏洞和下一步学习内容的唯一最新事实来源。
- 🎯 **自动重新排序优先级** —— 领域权重 × 覆盖率 × 未解决漏洞 → 每次会话后都会更新下一步学习计划。
- 🔍 **给出引用或承认不确定性** —— 严格程度可按主题配置（strict / standard / relaxed）。

---

## 仓库结构

```
topic-config.md              ← 你正在学习什么（空白模板，由你填写）
topic-config.template.md     ← 相同模板的参考副本
INIT.md                      ← 初始化与日常更新流程
CLAUDE.md                    ← 教练行为（苏格拉底式、双步骤追踪）
AGENTS.md                    ← 给 Codex / opencode / 其他代理使用的同版说明

progress/
  STUDY-TRACKER-TEMPLATE.md  ← 通用追踪器模板（教练会据此生成
                               progress/<short-code>-study-tracker.md）

sessions/
  SESSION-TEMPLATE.md        ← 通用单次会话笔记模板
  YYYY-MM-DD/                ← 每个学习日一个文件夹（由教练创建）
    session-notes.md
```

---

## 快速开始

1. **克隆**这个仓库（或把它作为模板使用）。
2. **在你选择的编码代理中打开它** —— Claude Code、Codex、opencode 等 —— 然后直接开始聊天。比如说：
   > “我想学习 X。请帮我初始化追踪器。”

   教练会读取 `CLAUDE.md`（或 `AGENTS.md`），发现 `topic-config.md`
   仍处于模板状态，然后引导你填写它（主题名称、目标日期、领域与权重、
   学习材料、验证级别、个性化设置）。随后它会根据模板生成
   `progress/<short-code>-study-tracker.md`，并基于“权重最高且覆盖率最低”的
   领域建议你的第一次学习会话主题。
3. **持续提问即可。** 每次会话后，教练都会在 `/sessions/YYYY-MM-DD/`
   下写入一份 session-notes 文件，并更新追踪器——无需手动记账。

> **提示：** 你也可以先手动填写 `topic-config.md`，然后再让教练
> “初始化追踪器”——这两种流程都可以。

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
| 会话开始 | 提问、请求练习，或说“我今天该学什么？” | 读取 `topic-config.md`、追踪器和最新会话笔记来获取上下文 |
| 会话进行中 | 诚实回答理解检查问题——包括直接说“我不知道” | 用苏格拉底式方式教学；按配置要求的严格程度引用来源 |
| 会话结束 | 什么都不用做——直接结束即可 | 写入今天的 `session-notes.md`，并更新追踪器（掌握项 / 知识漏洞 / 下一步） |

会话之间你常见可以问的问题：
- “我今天该重点学什么？”
- “针对我的薄弱点考考我。”
- “展示一下我的进度。”
- “根据考试日期重新排优先级。”

---

## 切换学习主题

由于所有行为都由 `topic-config.md` 驱动，切换主题只需要替换这一个文件：

```bash
cp topic-config.md topic-config.<old-topic>.bak.md   # 保存旧配置
cp topic-config.template.md topic-config.md          # 重新置空
# 填写你的新主题，然后让教练“初始化追踪器”
```

系统会生成新的 `progress/<new-short-code>-study-tracker.md`；之前的追踪器会保留在 `progress/` 中，作为历史参考。

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
