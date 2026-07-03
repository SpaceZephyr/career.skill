---
name: ai-pm-method-ai-engineering
description: |
  《AI Engineering: Building Applications with Foundation Models》(Chip Huyen, O'Reilly 2025) 方法论。
  把 Huyen 的 evaluation-driven development、三层 AI stack、prompt/RAG/finetune 决策树、agent 设计模式
  转化为 AI PM 可执行的判断框架。用于 AI 需求评审、方案讨论、发布决策。每条规则标注原书章节。
  触发词：「AI Engineering」「Chip Huyen」「evaluation-driven development」「EDD」
  「RAG vs fine-tune」「agent 设计」「AI 上线」「AI 评测集」「模型选型」。
---

# 《AI Engineering》· 用评测驱动一切，用系统思维做 AI 产品

Chip Huyen 的核心主张：**AI 产品的胜负不在模型，在系统**——评测、数据、组件拆分、可观测性。
PM 的任务不是"想一个 AI 功能"，是"先定义'好'长什么样，再决定用什么技术"。

## 什么时候用我

- 需求评审判断"这个 AI 功能该不该做" → 框架 1（三层 AI Stack）
- 立项/PRD 定义评测集与验收标准 → 框架 2（EDD）
- 方案讨论"prompt / RAG / fine-tune 选哪个" → 框架 3（决策树）
- 有人提议"上 Agent" → 框架 4（Agent 设计与门槛）
- 发布决策 → 检查清单 1-3
- 不适合：早期用户调研、纯商业化定价——用其他 Skill

## 核心框架

### 框架 1：三层 AI Stack + PM 决策边界（第 1、10 章）

| 层 | 内容 | PM 决策权重 | 主要动作 |
|---|---|---|---|
| Application | Prompt、上下文、UI/UX、Guardrail、反馈闭环 | 高 | 定义任务、写评测、设计反馈 |
| Model | 选型、fine-tune、数据集、推理优化 | 中 | 提业务约束（成本/延迟/合规），参与选型 |
| Infrastructure | Serving、监控、cache、并行 | 低 | 提 SLO（TTFT/TPOT/QPS），不干预实现 |

**顺序原则**：先穷尽 Application 层，再下探 Model 层，最后才动 Infra。用这个拦"上来就要 fine-tune / 换基座"的方案。

### 框架 2：Evaluation-Driven Development（第 3、4 章）

**核心主张**："在写代码之前，先定义你怎么评测它。" 类比 TDD。

**四步流水线**：
1. **定义评测维度**——Domain capability / Generation quality / Instruction-following / Cost & latency 四支柱，每项定"业务可翻译"阈值（例：事实性 ≥80% → 客服自动化率 30%）
2. **写 Scoring Rubric**——每档配锚定 example，3 人独立标注 Kappa ≥0.6，否则回炉
3. **三套评测集**——Production baseline（真实分布）+ Sliced（按用户段/语言/场景）+ Failure-mode（typo、越界、prompt injection）。样本 <30 不做发布决策
4. **Meta-evaluation**——评测本身可靠吗？用 AI judge 必须用人标数据校准（self-bias / position bias / length bias）

**产出**：PRD 附录《评测方案》。

### 框架 3：Prompt vs RAG vs Fine-tune 决策树（第 5、6、7 章）

```
输出不符合预期
├─ 没系统试过 prompt engineering？→ 先做版本管理 + few-shot + CoT（第 5 章）
├─ "不知道 / 说错事实"（信息缺失型）→ RAG（第 6 章）
│  · 先 BM25 baseline，再 embedding，最后混合
├─ "格式/风格/行为不对"（行为不对型）→ Fine-tune（第 7 章）
│  · 优先 PEFT/LoRA；数据质量 > 数量
└─ 两种都有 → 先 RAG 后 fine-tune ("RAG is for facts, fine-tuning is for form")
```

**PM 拒绝 fine-tune 的四个理由**：没做过 prompt 迭代 / 拿不出几百条高质量标注 / 本质是知识型 / 无离线评测。

### 框架 4：Agent 设计与上线门槛（第 6 章）

**Agent 三要素**：Plan generation → Tool use → Reflection。

**关键警告**：*"The more automated the agent becomes, the more catastrophic its failures."* 单步 90% 正确率，10 步就掉到 35%。

**Agent 值不值得上的五问**（任一否 → 降级为 workflow）：
1. 单步任务准确率 ≥ 95%？
2. 工具数量 ≤ 10 且选择错误率可控？
3. 每步有可观测埋点？
4. 有 human-in-the-loop / 回滚兜底？
5. 用户能承受多 5-10 倍 token 成本？

**多 Agent 反模式**：单 Agent 都跑不稳时叠多 Agent，只会指数级放大 debug 成本。

## 决策规则（10 条，来源标注）

1. PRD 无《评测方案》→ 不进立项评审（第 3 章）
2. 失败模式是"事实错误 / 知识不足"→ 先做 RAG，不做 fine-tune（第 6、7 章）
3. 失败是"格式/风格"且已做过 prompt 优化 → fine-tune，PEFT/LoRA 优先（第 7 章）
4. 单步准确率 <95% → 禁上 Agent（错误跨步骤复利）（第 6 章）
5. 无离线评测集 → 不允许换基座、fine-tune、上线新版本（第 3、4 章）
6. 公共 benchmark（MMLU 等）是唯一决策依据 → 拒绝该选型（benchmark 污染）（第 4 章）
7. AI judge 没经人标数据校准 → 分数不能作为发布判据（第 3 章）
8. 只做端到端评测、无组件级评测（retriever / router / tool 选择）→ 无法定位失败（第 10 章）
9. 观测只记最终输出 → 线上必然 debug 不了（第 10 章）
10. 无业务翻译（"80% 事实性 = 30% 自动化"）→ 技术指标无法驱动决策（第 3、4 章）

## 检查清单

### 清单 1：模型选型（第 4、9 章）
- [ ] 任务所需**领域能力**基线明确（不是通用能力）
- [ ] 有**私有评测集**跑过候选模型（不依赖公共 benchmark）
- [ ] 延迟 SLO 对齐（TTFT / TPOT p50/p90）
- [ ] **单位业务动作成本**（per 用户任务，不是 per token）
- [ ] 上下文窗口覆盖 RAG 拼接后最大长度
- [ ] 隐私 / 出境合规约束（自建 vs API）
- [ ] 指令遵循与拒答行为符合业务边界
- [ ] 留可控性开关（system prompt / temperature / seed）

### 清单 2：评测集设计（第 3、4 章）
- [ ] Rubric，每档有锚定 example
- [ ] 多人标注一致性 Kappa ≥0.6
- [ ] Production baseline / Sliced / Failure-mode 三集齐全
- [ ] 每集样本 ≥30（统计显著性）
- [ ] 组件级评测存在
- [ ] AI judge 用人标数据校准过
- [ ] 指标可翻译为业务指标

### 清单 3：上线前（第 6、10 章）
- [ ] 单步准确率 ≥95%（Agent 场景硬门槛）
- [ ] Human-in-the-loop / 回滚存在
- [ ] Kill switch（一键停用）
- [ ] 观测覆盖 quality / engagement / performance / component health 四维
- [ ] 用户反馈通道（显式打分 + 隐式信号）
- [ ] Prompt injection / 越权红队测试
- [ ] 成本受控（每用户/每日封顶）
- [ ] 灰度方案（内测 → 5% → 25% → 全量）

## 反模式（书中警告）

1. Vibe check 不建评测集——无法回归、无法迭代（第 3 章）
2. 把 prompt engineering 当终点，没有系统 A/B（第 5 章）
3. 过度依赖 AI judge 不校准（第 3 章）
4. 单 Agent 不稳时叠多 Agent（第 6 章）
5. 只评端到端不评组件（第 10 章）
6. 只记最终输出，事故时无法回放（第 10 章）
7. 拿公共 benchmark 做选型（污染严重）（第 4 章）
8. 未定义"够好"就开跑（第 3 章）

## 边界声明

- 出版 2025 年初，具体模型能力结论可能已过时；方法论层（EDD、组件评测、分层）稳固
- 面向工程实践，弱在 UX 与商业化——搭配 pm-go-to-market / pm-marketing-growth 类 Skill
- 假设"用现成 foundation model"；从零训模型不适用（改用 Huyen 的《Designing ML Systems》）
- 偏英文场景，中文场景需自建评测集
- 合规讨论对齐 US/EU，中国生成式 AI 备案/数据出境不覆盖

## 工作流

1. **匹配场景**：该不该做→框架 1；怎么验收→框架 2；prompt/RAG/finetune→框架 3；要不要 Agent→框架 4；能不能上线→清单 1-3
2. **补齐信息**：核心缺失（业务目标、失败模式、约束）一次列出问；次要缺失用「[假设]」补全推进
3. **按框架输出中间产出**：每步给可直接进 PRD 的表格/清单
4. **过检查清单**：标注未通过项（必须补齐 / 可延后风险）
5. **收尾声明边界**：提示叠加哪个视角交叉验证（UX / 商业化 / 合规）

## 完整方法论

详细章节引用、EDD 四支柱完整定义、Agent 反模式列表见 `references/framework.md`。

> 本 Skill 由 career-skill-factory 生成。内容提炼自原著，版权归原作者，仅供个人学习。
