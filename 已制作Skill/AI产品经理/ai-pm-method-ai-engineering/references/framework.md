---
name: ai-pm-method-ai-engineering
description: |
  《AI Engineering: Building Applications with Foundation Models》（Chip Huyen, O'Reilly 2025）方法论。
  把 Huyen 的 evaluation-driven development、三层 AI stack、prompt/RAG/finetune 决策树、agent 设计模式
  转化为 AI PM 可执行的判断框架。用于 AI 需求评审、方案讨论、发布决策。
  每条规则标注原书章节，可追溯。触发词：「AI Engineering」「Chip Huyen」「evaluation-driven development」
  「EDD」「RAG vs fine-tune」「agent 设计」「AI 上线」「AI 评测集」「模型选型」。
---

# 《AI Engineering》· 用评测驱动一切，用系统思维做 AI 产品

Chip Huyen 的核心主张：**AI 产品的胜负不在模型，在系统**——评测、数据、组件拆分、可观测性。
PM 的任务不是"想一个 AI 功能"，而是"先定义'好'长什么样，再决定用什么技术"。

---

## 什么时候用我

- 需求评审阶段判断"这个 AI 功能该不该做" → 框架 1（三层 AI Stack + Product-First 决策）
- 方案讨论阶段判断"用 prompt、RAG 还是 fine-tune" → 框架 3（三选决策树）
- 立项/PRD 阶段设计评测集与验收标准 → 框架 2（Evaluation-Driven Development）
- 有人提议"上 Agent"时判断是否值得 → 框架 4（Agent 设计与门槛）
- 发布决策：GA 之前的 checklist → 检查清单 1-3
- 不适合：早期用户调研（用 pm-market-research 系列）、纯商业化定价（本书基本不覆盖）

---

## 核心框架

### 框架 1：三层 AI Stack + PM 决策边界（来源：第 1、10 章）

**适用场景**：需求评审、跨团队方案讨论、判断某个问题该谁解决。

**三层结构**（自上而下）：

| 层 | 内容 | PM 决策权重 | PM 主要动作 |
|----|------|------------|-------------|
| Application 层 | Prompt、上下文构造、UI/UX、Guardrail、用户反馈 | 高 | 定义任务、写评测集、设计反馈闭环 |
| Model 层 | 模型选型、fine-tune、数据集、推理优化 | 中 | 提供业务约束（成本/延迟/合规），参与选型 |
| Infrastructure 层 | Serving、监控、KV/prompt cache、并行策略 | 低 | 提 SLO（TTFT/TPOT/QPS），不干预实现 |

**Huyen 的顺序原则**（第 1 章）："先穷尽 Application 层的优化，再下探到 Model 层，最后才动 Infra"。PM 用这个原则拦住"上来就要 fine-tune / 换基座"的方案。

**步骤**：
1. 拿到一个 AI 需求，先问：这个问题是 Application 层能解决的吗？（改 prompt、加 context、换 UI）
2. 如果 Application 层瓶颈，是知识不足还是行为不对？→ 进入框架 3
3. 如果需要 Model 层动作，明确业务约束（预算/延迟/隐私）再交给算法团队

**输出物**：一张"问题定位表"——问题描述 / 判断在哪一层 / 交给谁 / PM 提供的约束。

---

### 框架 2：Evaluation-Driven Development（EDD）（来源：第 3、4 章）

**适用场景**：任何 AI 需求进入立项，PRD 必须包含评测方案。

**核心主张**（第 3 章开篇）："在写代码之前，先定义你怎么评测它。" 类比 TDD。

**四步流水线**：

**Step 1 — 定义评测维度（四支柱，第 4 章）**
- Domain-specific capability：领域知识/任务能力
- Generation quality：连贯性、事实性、相关性
- Instruction-following：格式、约束、拒答边界
- Cost & latency：单次调用成本、TTFT、TPOT

PM 必须为每一项定一个"业务可翻译"的阈值，例：事实性 ≥ 80% → 客服自动化率 30%。

**Step 2 — 写评测指南（Scoring Rubric）**
- 明确"好"和"坏"分别长什么样，配具体样例
- 每一档给锚定 example
- 用 3 个人独立标注同一批数据，Kappa 一致性 < 0.6 → rubric 有歧义，回炉

**Step 3 — 组装多套评测集**
- Production baseline set：反映真实分布
- Sliced sets：按用户段、语言、场景切
- Failure-mode set：专攻已知失败模式（typo、越界、prompt injection）
- 用 bootstrap 验证统计显著性，样本量 < 30 的结论不做发布决策

**Step 4 — Meta-evaluation（元评测）**
- 你的评测本身可靠吗？分数是否可复现？
- 如果用 AI judge，要用人标数据校准 judge 的 bias（自偏、位置偏、长度偏）

**输出物**：PRD 附录中的《评测方案》——四维度阈值 / rubric / 三套评测集 / judge 校准记录。

---

### 框架 3：Prompt vs RAG vs Fine-tune 决策树（来源：第 5、6、7 章）

**适用场景**：AI 方案讨论会。有人喊"我们要 fine-tune"时用这棵树拦一下。

```
问题：模型输出不符合预期
│
├─ 从没系统试过 prompt engineering？
│  → 先做 prompt 版本管理 + few-shot + CoT，不许跳过（第 5 章）
│
├─ 失败模式是"不知道 / 说错事实"？（信息缺失型）
│  → 上 RAG（第 6 章）
│  · 私有知识、时效性、事实性场景优先
│  · 先 BM25 baseline，再上 embedding，最后混合
│
├─ 失败模式是"格式/风格/行为不对"？（行为不对型）
│  → Fine-tune（第 7 章）
│  · 优先 PEFT / LoRA，不做全量微调
│  · 数据质量 > 数据数量
│
└─ 两种都有？
   → 先 RAG 后 fine-tune；Huyen 原话："RAG is for facts, fine-tuning is for form."
```

**PM 拒绝 fine-tune 的四个理由**（第 7、8 章）：
1. 还没做 prompt 版本管理和 few-shot → 拒
2. 拿不出至少几百条高质量标注数据 → 拒（"finetuning is easy, getting data is hard"）
3. 问题本质是知识型（应上 RAG）→ 拒
4. 没有离线评测集来验证 fine-tune 是否真的提升 → 拒

**输出物**：技术方案对比表——问题类型 / 首选方案 / 备选方案 / 拒绝理由。

---

### 框架 4：Agent 设计模式与上线门槛（来源：第 6 章）

**适用场景**：有人说"我们做一个 Agent"时。

**Agent 三要素循环**：
1. **Plan generation**：任务拆解（CoT 是最简形式）
2. **Tool use + execution**：调工具、观察结果
3. **Reflection**：评估结果、修正策略（ReAct / Reflexion）

**Huyen 的关键警告**（第 6 章）：
> "The more automated the agent becomes, the more catastrophic its failures."
> Agent 的错误会跨步骤复利放大——单步 90% 正确率，10 步就掉到 35%。

**Agent 值不值得上的 PM 判断五问**：
1. 单步任务的评测通过率 ≥ 95%？（否则别叠 Agent）
2. 工具数量 ≤ 10？工具间的选择错误率可控？
3. 每步都有可观测埋点（tool 选择、参数、结果）？
4. 有失败回滚 / human-in-the-loop 兜底？
5. 用户能承受"多花 5-10 倍 token 但结果更好"的成本？

任何一项否 → 降级为 workflow（固定编排）而不是 agent（模型自主决策）。

**多 Agent 反模式**（第 6 章）：在单 Agent 都跑不稳时叠多 Agent，只会指数级放大 debug 成本。

**输出物**：Agent 立项评审表——五问打分 / 单步基线 / 观测方案 / 兜底策略。

---

## 决策规则（10 条，来源标注）

1. 如果 PRD 里没有《评测方案》章节，则不允许进立项评审。（第 3 章 EDD 原则）
2. 如果失败模式是"事实错误 / 知识不足"，则先做 RAG，不做 fine-tune。（第 6、7 章）
3. 如果失败模式是"格式/风格/行为不对"且已做过 prompt 优化，则考虑 fine-tune，优先 PEFT/LoRA。（第 7 章）
4. 如果单步任务准确率 < 95%，则禁止上 Agent（错误跨步骤复利）。（第 6 章）
5. 如果没有离线评测集，则不允许换基座、不允许 fine-tune、不允许上线新版本。（第 3、4 章）
6. 如果公共 benchmark（MMLU 等）是唯一决策依据，则拒绝该选型——benchmark 污染严重。（第 4 章）
7. 如果 AI judge 没有用人标数据校准过，则其分数不能作为发布判据。（第 3 章 meta-evaluation）
8. 如果只做端到端评测、没有组件级评测（retriever、router、tool 选择），则无法定位失败。（第 10 章）
9. 如果观测埋点只记录最终输出，则线上一定 debug 不了——必须记录中间步骤。（第 10 章）
10. 如果没定义业务翻译（"80% 事实性 = 30% 自动化"），则技术指标无法驱动决策。（第 3、4 章）

---

## 检查清单

### 清单 1：模型选型 Checklist（来源：第 4、9 章）

- [ ] 是否明确了任务所需的**领域能力**基线（不是通用能力）？
- [ ] 是否有**私有评测集**（不依赖公共 benchmark）跑过候选模型？
- [ ] 是否对齐了**延迟 SLO**（TTFT p50/p90、TPOT p50/p90）？
- [ ] 是否算过**单位业务动作成本**（不是 per token，是 per 用户任务）？
- [ ] 是否检查了**上下文窗口**是否覆盖 RAG 拼接后的最大长度？
- [ ] 是否评估了**数据隐私 / 出境合规**约束（自建 vs API）？
- [ ] 是否验证了**指令遵循与拒答行为**符合业务边界？
- [ ] 是否留了**可控性**开关（system prompt、temperature、seed）？

### 清单 2：评测集设计 Checklist（来源：第 3、4 章）

- [ ] 是否有 rubric，且每档有锚定 example？
- [ ] rubric 是否经过多人标注一致性验证（Kappa ≥ 0.6）？
- [ ] 是否有 production baseline set（真实分布）？
- [ ] 是否有 sliced sets（按用户段/语言/场景切）？
- [ ] 是否有 failure-mode set（专攻已知问题）？
- [ ] 每个集合样本量是否 ≥ 30，具备统计显著性？
- [ ] 是否有组件级评测（不只是端到端）？
- [ ] 如用 AI judge，是否用人标数据校准过 bias？
- [ ] 评测指标是否可翻译为业务指标？

### 清单 3：Agent / AI 功能上线前 Checklist（来源：第 6、10 章）

- [ ] 单步任务准确率是否 ≥ 95%（Agent 场景硬门槛）？
- [ ] 是否有 human-in-the-loop 或回滚机制？
- [ ] 是否有 kill switch（一键停用）？
- [ ] 观测是否覆盖：quality / engagement / performance / component health 四维？
- [ ] 是否有用户反馈通道（显式打分 + 隐式信号如 regenerate、early stop）？
- [ ] 是否有 prompt injection / 越权行为的红队测试？
- [ ] 成本是否受控（有每用户/每日封顶）？
- [ ] 灰度方案是否定义（先内测 → 5% → 25% → 全量，每档观察指标）？

---

## 反模式（书中明确警告）

1. **只看 vibe check，不建评测集**——多数团队用"感觉还不错"发布 AI 功能，结果无法回归、无法迭代。（第 3 章）
2. **把 prompt engineering 当终点**——"prompt 上限很低但天花板给人错觉"，没有系统化 A/B 就宣称最优。（第 5 章）
3. **过度依赖 AI judge**——LLM 打分有 self-bias、position bias、length bias，不校准就相信。（第 3 章）
4. **过早上 Agent / 多 Agent**——单 Agent 都不稳时叠多 Agent，错误率指数放大。（第 6 章）
5. **只评端到端，不评组件**——retriever 挂了还是 generator 挂了？不知道就迭代不动。（第 10 章）
6. **只记录最终输出**——线上事故时无法回放中间步骤，debug 不了。（第 10 章）
7. **拿公共 benchmark 做选型**——训练数据污染严重，MMLU 高分不代表你的场景强。（第 4 章）
8. **未定义"够好"就开跑**——工程可以无限优化，业务没有停下的信号。（第 3 章）

---

## 边界声明

- **出版时间 2025 年初**：模型能力快速演进，书中"当前模型不擅长 X"的具体结论（尤其长上下文、多模态、agent 稳定性）可能已过时；但方法论层（EDD、组件评测、AI stack 分层）仍稳固。
- **面向工程实践，弱在 UX 与商业化**：本书对定价模型、增长策略、GTM、用户体验细节几乎不涉及。这些应搭配 pm-go-to-market、pm-marketing-growth 类 Skill。
- **假设"用现成 foundation model"**：如果团队真的要从零训模型，本书不适用（Huyen 更早的《Designing Machine Learning Systems》才是对应方法论）。
- **偏英文场景**：书中评测数据集、benchmark 举例主要基于英文。中文场景需自建评测集，本书没提供中文特殊性处理。
- **数据基于英文互联网 + 硅谷公司实践**：合规讨论对齐 US/EU 语境，对中国合规（生成式 AI 备案、数据出境）不覆盖。
- **提炼来源**：本 Skill 基于 Chip Huyen 官方 aie-book 仓库 chapter-summaries、O'Reilly 章节页、Bagerbach 详细读书笔记、Alex Strick 第 1 章笔记、Medium EDD 解读交叉验证，建议关键决策对照原书章节复核。

---

## 工作流

收到用户问题后：

1. **匹配场景**
   - "该不该做 / 谁来做" → 框架 1
   - "怎么定义成功 / 怎么验收" → 框架 2
   - "prompt / RAG / fine-tune 选哪个" → 框架 3
   - "要不要上 Agent" → 框架 4
   - "能不能上线" → 检查清单 1-3

2. **补齐信息**：核心信息缺失（业务目标、失败模式、约束）→ 一次列出问；次要缺失（具体数字、样本量）→ 用「[假设]」标注补全继续推进。

3. **按框架输出中间产出**：每一步给出可以直接进 PRD/评审文档的产物（表格、清单、评测集结构）。

4. **用检查清单过一遍结论**：标注未通过项，指出这是"必须补齐"还是"可延后风险"。

5. **收尾声明边界**：提示本书方法论覆盖不到的部分，建议叠加哪个视角（UX / 商业化 / 合规）交叉验证。

---

## 参考来源

- Chip Huyen, *AI Engineering: Building Applications with Foundation Models*, O'Reilly, 2025
- 官方 chapter summaries：https://github.com/chiphuyen/aie-book/blob/main/chapter-summaries.md
- O'Reilly 章节：https://www.oreilly.com/library/view/ai-engineering/9781098166298/
- Bagerbach reading notes：https://bagerbach.com/books/ai-engineering/
- Alex Strick, Chapter 1 notes：https://alexstrick.com/posts/2025-01-19-notes-on-ai-engineering-chapter-1.html
- Keerthana MS, EDD framework 解读：https://medium.com/@keerthanams1208/chip-huyens-evaluation-driven-development-edd-framework-from-ai-engineering-a2939cc9ecf8

> 本 Skill 由 career-skill-factory 生成。内容提炼自原著，版权归原作者，仅供个人学习。
