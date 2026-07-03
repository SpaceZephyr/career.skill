<h1 align="center">Career Skill Factory</h1>

<p align="center"><code>career.skill</code></p>

<p align="center"><em>「说出你的职业，它给你造一支能开评审会的顾问团」</em></p>

<p align="center">
  <img alt="License MIT" src="https://img.shields.io/badge/License-MIT-c8a500?style=for-the-badge">
  <img alt="Agent Skills Standard" src="https://img.shields.io/badge/Agent%20Skills-Standard-5aa524?style=for-the-badge">
  <img alt="skills.sh Compatible" src="https://img.shields.io/badge/skills.sh-Compatible-1888c8?style=for-the-badge">
  <img alt="Runtime" src="https://img.shields.io/badge/Runtime-Claude%20Code%20%C2%B7%20Codex%20%C2%B7%20Cursor%20%C2%B7%20OpenClaw%20%C2%B7%20Hermes-7b2bd9?style=for-the-badge">
</p>

<p align="center">
  公众号 / 小红书 / B站 / 知乎：<strong>空格的键盘</strong> ｜ <a href="mailto:wzfh520@gmail.com">wzfh520@gmail.com</a>
</p>

Career Skill Factory 是一套给知识工作者用的"职业 Skill 蒸馏工厂"。

你输入一个职业或行业，它自动完成三件事：

- 遴选该行业 3 位顶尖专家，蒸馏他们的心智模型，生成 3 个「专家视角 Skill」
- 遴选该行业 3 本顶尖书籍，提炼可执行方法论，生成 3 个「方法论 Skill」
- 编排出 1 个「顾问团总控 Skill」，能路由问题、召开多专家评审会

最终交付 7 个可直接安装的 Skill 文件，构成一个能陪你判断、复盘、做决策的私人顾问团。

基于开放的 Agent Skills 协议，可在 Claude Code、Codex、Cursor、OpenClaw、Hermes Agent、CodeBuddy、Workbuddy、Gemini CLI、OpenCode 等兼容 runtime 中运行。

不是给你一份"AI 职业规划"文档。

是给你一支能反复调用、越用越顺手的顾问团，专家会用他们自己的心智模型和你对话，书籍方法论会按章节可追溯地过每条决策。

看效果 · 安装 · 它能做什么 · 使用方式 · 工作原理 · 诚实边界

## 效果示例

```text
用户      ❯ 我是 AI 产品经理，帮我造一套 Skill

助手      ❯ 已完成职业调研，遴选如下：

            专家（视角）：
            1. Marty Cagan — Product Model + 四大风险 + Product Creator
            2. Andrej Karpathy — Software 1/2/3 + LLM OS + Jagged Intelligence
            3. 梁宁 — 真需求三角 + 情绪原色 + 点线面体

            书籍（方法论）：
            1. AI Engineering (Chip Huyen) — EDD + Prompt/RAG/Finetune 决策树
            2. Prediction Machines — AI Canvas + Cheap Prediction 经济学
            3. 俞军产品方法论 — 用户价值公式 + 交易模型

            + 1 个顾问团总控（自动路由 / 分歧地图 / 评审会纪要）

            将输出 7 个 SKILL.md，直接可装。
```

再看顾问团路由：

```text
用户      ❯ 我在纠结要不要做一个 AI 简历润色工具，让专家们看看

助手      ❯ 路由到 3 位成员：

            首选 · 梁宁：先过真需求三角
              - 价值：用户没这个工具会怎样？答"用 ChatGPT 也行" → 伪需求信号
              - 共识：HR 认不认 AI 改的简历？
              - 模式：能改变付费结构，还是只增加 token 成本？

            次选 · Prediction Machines：AI Canvas 7 格补齐
              - Prediction / Judgment / Action / Feedback 有没有想清楚

            补位 · 俞军方法论：换用成本四拆
              - 学习 / 数据迁移 / 信任 / 习惯，尤其信任成本

            要不要开评审会（三位并行评审 + 综合结论）？
```

再看评审会纪要：

```text
用户      ❯ 都上，开评审会

助手      ❯ 已并行调用三位视角，输出：

            ## 评审会纪要：AI 简历润色工具
            ### 共识
            - 需求真实，但天花板受"HR 信任度"制约
            ### 分歧（Cagan vs 梁宁）
            - Cagan：先看你能不能进 product model 组织交付
            - 梁宁：先看你能不能穿透价值-共识-模式三角
            ### 综合结论
            - 如果你更在乎快速验证，听梁宁走 MVP
            - 如果你在大厂内孵化，听 Cagan 先看组织土壤
            ### 本次盲区
            - HR 侧真实付费意愿数据（顾问团覆盖不到）
```

Career Skill Factory 不止是"生成一堆 Skill 文件"。

它把职业拆解成**可反复调用的判断工具**：专家视角负责心智模型，书籍方法论负责可追溯的操作框架，顾问团负责路由和分歧呈现。

## 安装

Career Skill Factory 基于开放的 Agent Skills 协议，可在任何 skills-compatible 的 AI agent runtime 中运行。

### 方式一：一行命令（推荐，跨 runtime）

打开你正在用的 agent（Claude Code、Codex、Cursor、OpenClaw、Hermes、CodeBuddy、Workbuddy、Gemini CLI、OpenCode 等），告诉它：

```text
帮我安装这个 skill：https://github.com/SpaceZephyr/career.skill
```

或者用通用 CLI 安装器：

```bash
npx skills add SpaceZephyr/career.skill
```

它会自动识别你当前的 runtime 并把 skill 放到正确目录。需要指定时可加 runtime 参数，例如 `-a codex` / `-a claude-code` / `-a cursor`。

### 方式二：手动安装

克隆仓库后，把根目录（factory 本身）复制到 skills 目录：

```bash
git clone https://github.com/SpaceZephyr/career.skill.git
```

仓库结构：

```text
career.skill/
├── SKILL.md              # career-skill-factory 主入口
├── references/           # 专家蒸馏 / 书籍提炼 / 总控模板
│   ├── expert-distillation.md
│   ├── book-distillation.md
│   └── advisory-board-template.md
├── examples/             # 参考实现（投资顾问团）
│   ├── investing-advisor-duanyongping/
│   ├── investing-method-intelligent-investor/
│   └── investing-advisory-board/
└── 已制作Skill/           # 已经蒸馏好、可直接安装的成品 Skill 套件
    └── AI产品经理/         # 首套：AI PM 顾问团 (7 个 Skill)
```

### 方式三：直接用已制作的 Skill 套件

如果你不想跑蒸馏流程，可以直接把 `已制作Skill/` 下的任意套件复制到你的 skills 目录使用。目前提供：

- **AI 产品经理**（`已制作Skill/AI产品经理/`）—— Cagan / Karpathy / 梁宁 + AI Engineering / Prediction Machines / 俞军方法论 + 顾问团总控

未来会陆续加入更多职业套件。

## 它能做什么

Career Skill Factory 本身是一个「蒸馏工厂」，它的产物是一支支「职业顾问团」。

### 工厂本身

| Skill | 解决的问题 | 输出 |
| --- | --- | --- |
| `career-skill-factory` | 输入职业 → 输出 7 个可安装的 Skill（3 专家 + 3 方法论 + 1 顾问团） | 一整套 SKILL.md |

### 已制作套件：AI 产品经理

`已制作Skill/AI产品经理/` 下的 7 个 Skill，可独立安装：

| Skill | 类型 | 核心镜片 | 最擅长的问题 |
| --- | --- | --- | --- |
| `ai-pm-advisor-cagan` | 专家视角 | Product Model + 四大风险 + Product Creator | AI PM 团队怎么组、AI 需求要不要立项 |
| `ai-pm-advisor-karpathy` | 专家视角 | Software 1/2/3 + LLM OS + Jagged Intelligence + Autonomy Slider | AI 能力评估、Agent 上线门槛、护城河判断 |
| `ai-pm-advisor-liangning` | 专家视角 | 真需求三角 + 情绪原色 + 点线面体 | 是不是真需求、C 端为何不上瘾、AI 创业方向 |
| `ai-pm-method-ai-engineering` | 方法论 | EDD + 三层 AI Stack + RAG/finetune 决策树 | 评测方案、技术选型、发布 checklist |
| `ai-pm-method-prediction-machines` | 方法论 | AI Canvas + P/J/A 三分法 + Cheap Prediction | 战略判断、立项 canvas、护城河设计 |
| `ai-pm-method-yujun` | 方法论 | 用户价值公式 + 交易模型 + 决策效用 | AI 化增量效用、用户迁移、Copilot 取舍 |
| `ai-pm-advisory-board` | 顾问团总控 | 自动路由 + 分歧地图 + 评审会纪要 | 多视角评审、条件化综合结论 |

## 使用方式

你可以用自然语言直接触发工厂，也可以直接调用已装好的套件。

| 方式 | 示例 | 会触发什么 |
| --- | --- | --- |
| 造顾问团 | `我是数据分析师，帮我造一套 Skill`、`帮我做 XX 行业的专家顾问团` | 启动 career-skill-factory，遴选 3 专家 + 3 书 + 1 总控 |
| 单专家提问 | `用 Cagan 的视角看这个 AI 需求`、`Karpathy 会怎么评价这个 Agent 方案` | 调用对应专家 Skill，角色扮演式回答 |
| 方法论过一遍 | `用 AI Canvas 拆一下这个需求`、`用俞军的用户价值公式算一下` | 调用书籍方法论 Skill，按章节可追溯地过框架 |
| 顾问团路由 | `让专家们看看这个方案`、`帮我看看这个 AI 功能能不能做` | 顾问团根据问题类型推荐 1-2 个成员 |
| 开评审会 | `都上`、`开评审会`、`我要多视角评审` | 并行调用 3 位专家，输出共识/分歧/综合结论 |
| 提问特定成员 | `梁宁怎么看这个产品的情绪价值` | 直接跳过路由，调用指定 Skill |

## 工作原理

Career Skill Factory 不是一个"生成 Skill 模板"的脚本，而是一套完整的**认知蒸馏流水线**。

它把职业拆成四层：

| 层次 | 说明 |
| --- | --- |
| 遴选 | 从公开资料中识别该行业的 3 位顶尖专家（有原创心智模型）和 3 本顶尖书籍（有可执行方法论） |
| 蒸馏 | 对每位专家做三重验证（跨域复现 / 生成力 / 排他性），提取心智模型；对每本书按章节标注可追溯的规则与清单 |
| 生成 | 输出 3 个专家 Skill（含身份卡 / 心智模型 / 决策启发式 / 表达 DNA / 反模式 / 诚实边界 / 内在张力）+ 3 个方法论 Skill（含框架 / 决策规则 / 检查清单 / 边界声明） |
| 编排 | 生成 1 个总控 Skill：路由表覆盖高频决策场景 + 分歧地图预置成员间张力 + 评审会流程（subagent 并行 + 综合结论条件化） |

三份核心方法论文档在 `references/` 下：

- `expert-distillation.md` — 专家蒸馏五层（怎么想 / 怎么判断 / 怎么说话 / 什么不做 / 知道局限）+ 三重验证
- `book-distillation.md` — 书籍提炼四原则（忠于原著 / 可执行 / 可追溯 / 有边界）+ 章节标注硬要求
- `advisory-board-template.md` — 总控编排（路由模式 + 评审会模式 + 分歧地图）

## 适合谁

- **知识工作者**：想让 AI 从"通用助手"变成"懂你行业的顾问团"
- **产品经理 / AI 从业者**：需要在决策前听 3 个视角、看清分歧
- **创业者 / 独立开发者**：需要专家陪跑但请不起真专家
- **顾问 / 研究员**：需要把自己领域的知识蒸馏成可复用的判断工具
- **重度 Skill 用户**：已经装了 100+ Skill，想按职业维度重新组织
- **教育 / 培训场景**：把一门课/一本书变成学员可反复调用的顾问

## 风控与安全性说明

Career Skill Factory 蒸馏的是公开信息，输出的是"某专家的视角快照"，不是真人代言。

- **视角非本人**：所有专家 Skill 明确声明"基于公开信息蒸馏，≠ 本人真实判断"，且首次激活会告知用户
- **书籍版权**：方法论 Skill 只做"框架 + 规则 + 章节引用"，不复制大段原文，仅供个人学习
- **调研时效**：所有 Skill 标注调研截止日期，行业变化快的部分建议重新调研
- **不做医疗 / 法律 / 投资代言**：涉及生命健康、法律责任、真金白银的决策，请咨询持牌专业人士
- **不伪造引用**：所有一手来源附 URL；无法追溯的结论标注 `[推断]`；书籍方法论每条规则标注章节
- **不模仿在世名人做误导**：Skill 明确不做人格背书、不做代人发声，仅提供"以某专家的心智模型分析问题"的辅助工具

## 诚实边界

每个顾问团都应该说明自己做不到什么。

- **蒸馏不是复刻**：即便三重验证过筛，也只覆盖专家公开表达过的部分。他们的私下判断、最新想法、面对全新问题的真实反应，Skill 无法预测
- **书籍会过时**：书出版时的具体数值、案例、benchmark 可能失效；框架层通常仍稳固，但请按当代情境校准
- **分歧地图不完备**：预置的成员间分歧基于调研可见的公开对立，未必穷尽所有场景。用户在使用中发现新分歧，请手动补入总控
- **顾问团只覆盖它认领的领域**：例如 AI PM 顾问团擅长产品判断，但不覆盖底层技术选型、中国合规、增长运营等——每个套件的 SKILL.md 都明确写出了盲区
- **重大决策不能外包**：顾问团输出是思考素材，不是最终答案。请把它当作"三位聪明的朋友给你一个下午的建议"，最终还是你自己决定

一个不告诉你边界在哪的顾问团，不值得信任。

## 参考

Career Skill Factory 的方法论基础：

- **专家蒸馏方法论**：受"如何提取一个人的认知操作系统"启发，采用三重验证（跨域复现 / 生成力 / 排他性）过筛心智模型
- **书籍提炼方法论**：改造自 book2skills 的四原则（忠于原著 / 可执行 / 可追溯 / 有边界）
- **顾问团编排**：受多智能体评审、opportunity solution tree 等启发，强调"分歧必须呈现张力，禁止和稀泥"

已提供的示例套件在 `examples/`（投资顾问团：段永平 + 芒格 + 马克斯 + 三本经典）与 `已制作Skill/`（AI 产品经理顾问团）下。

## 许可证

MIT
