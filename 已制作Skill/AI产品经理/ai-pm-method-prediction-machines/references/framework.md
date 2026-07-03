# 《Prediction Machines》· AI = Cheap Prediction 的 PM 判断框架

> 提炼自 Ajay Agrawal / Joshua Gans / Avi Goldfarb《Prediction Machines: The Simple Economics of Artificial Intelligence》（Updated & Expanded, HBR Press, 2022）。全书结构：Introduction + Chapter "Cheap Changes Everything" + 5 Parts（Prediction / Decision Making / Tools / Strategy / Society）共 19 章。
>
> **本文档定位**：不是读书笔记，是 AI 产品经理可直接嵌入需求评审、功能取舍、AI 商业化决策的判断框架。每条规则标注章节来源，可追溯。

---

## 一、什么时候用这份框架

| 场景 | 用哪个框架 |
|------|-----------|
| 判断某个功能 / 环节要不要 AI 化 | 框架 1（任务分解）+ 检查清单 A |
| 评估某个 AI 需求的完整度、能不能立项 | 框架 2（AI Canvas 7 格） |
| 决定 AI 在价值链哪一段能撬动战略级价值 | 框架 3（Prediction × Complements × Trade-off） |
| AI 定价、商业化、要不要开放 API | 决策规则 4 / 5 / 8 + 检查清单 B |
| 人机分工设计（哪部分给模型、哪部分留给人） | 决策规则 1 / 2 / 6 |
| **不适合本框架**：UX 细节、Prompt 工程、多智能体架构选型、模型评测方法 —— 请转其他 Skill |

---

## 二、核心框架

### 框架 1：Prediction / Judgment / Action 三分法（Part 2, Ch 7–8; Part 3, Ch 13）

**一句话**：任何决策 = **Input Data → Prediction → Judgment → Action → Outcome**（外加 Training Data、Feedback Data 两条数据回路）。AI 只擅长中间那一步 Prediction，其余环节不会因为你用了 AI 就自动升级。

**PM 用法（三步）**：

1. **拆任务**：把要评估的功能 / 工作流拆成一串独立子任务，每个子任务标注它是「预测型」（补全一段缺失信息）还是「判断型」（对结果打分、决定取舍）还是「执行型」（真的动手做）。
2. **算成本**：对每个子任务分别问「这一步预测便宜下来能省多少 / 卖多少钱」「判断成本会不会同步涨」「行动环节能不能承接更多 if-then 分支」。
3. **决策产出**：一张任务清单，每行标注 { 子任务 | P/J/A 类型 | AI 化后 ROI | 依赖的补足环节 }。**只挑 P 类高 ROI 且补足环节现成的做 AI 化**，J/A 环节保留人或另建工具。

> 关键洞察（Ch 3, Ch 7）：**"Prediction is not decision. Decision requires judgment."** PM 最常犯的错就是把整块「决策」外包给模型，实际上模型只完成了 P，而 J 和 A 的成本反而被推高。

---

### 框架 2：AI Canvas — 7 格立项模板（Part 3, Ch 13；HBR 2018-04 "A Simple Tool to Start Making Decisions with the Help of AI"）

**一句话**：在写 PRD 之前，把 AI 功能拆进 7 个格子。7 格填不满，说明还没准备好立项。

```
┌──────────────┬──────────────┬──────────────┬──────────────┐
│  PREDICTION  │   JUDGMENT   │    ACTION    │    OUTCOME   │
│  要预测什么  │  怎么权衡    │  预测出来后  │  怎么算成功  │
│  （缺哪块    │  错报 vs     │  系统做什么  │  （成功指标  │
│   信息？）   │  漏报的代价  │  动作？      │   + 反例）   │
├──────────────┴──────────────┴──────────────┴──────────────┤
│         INPUT         │      TRAINING      │   FEEDBACK   │
│  生产时喂什么数据？   │  历史训练用什么？  │ 结果怎么回流 │
│                       │                    │  持续训练？  │
└───────────────────────┴────────────────────┴──────────────┘
```

**格子定义**（Agrawal et al., HBR 2018-04；书 Ch 13）：

| 格子 | 定义 | PM 需要回答 |
|------|------|-------------|
| Prediction | 要补全的那段缺失信息 | 一句话：「模型输出 X 概率 / X 数值 / X 分类」 |
| Judgment | 不同结果、不同错误类型的相对价值 | 假阳性 vs 假阴性哪个更贵？谁来定？ |
| Action | 预测出来后，系统 / 人做的具体动作 | 是自动触发还是人工确认？ |
| Outcome | 成功 / 失败的度量方式 | 度量它需要多久拿到数据？ |
| Input | 生产环境实时喂进模型的数据 | 冷启用户怎么办？ |
| Training | 训练模型用的历史数据 | 我们有吗？还是要买 / 标 / 造？ |
| Feedback | 上线后回流用于持续训练的数据 | 用户是否愿意留下 label？ |

**PM 用法**：需求评审时打印这张 canvas，对着每一格问「这一格能不能一句话讲清」。任何格子空着或含糊，都要么补需求、要么砍需求。

---

### 框架 3：Cheap Prediction → Complements 升值（Ch 2, Ch 4, Ch 5）

**核心论断**（Ch 2 "Cheap Changes Everything"）：
> "Computers made arithmetic cheap; the internet made distribution cheap; AI has made prediction cheap. When the price of something falls, we use more of it — and its complements become more valuable, its substitutes less valuable."

**PM 三条推论**：

- **Prediction 的替代品**（会贬值）：人工做预测（分析师人力评分、专家经验判断） → 这类岗位 / 功能会被压价。
- **Prediction 的互补品**（会升值）：Judgment（人类价值判断）、Data（尤其是持续更新的输入/反馈数据）、Action（下游执行能力）—— **这些才是 AI 时代真正的定价权和护城河**。
- **PM 决策规则**：如果你的 AI 功能没有配套升级 Judgment 表达工具 / 数据回路 / 下游执行 API，你只是让最便宜的东西又便宜了一点，用户价值近乎为零。

---

## 三、决策规则（10 条，每条标章节）

1. **PM 只能优化能被清晰定义的预测目标。**如果 Prediction 一格写不出「输出 X 的概率」这样具体的句子，先别做 AI 化。（Ch 3, Ch 13）

2. **预测越便宜，Judgment（人类偏好表达工具）越值钱。**当你把某个环节 AI 化后，必须**同步投入设计一套让用户 / 运营表达"错报 vs 漏报哪个更贵"的机制**（阈值滑块 / 反馈按钮 / 权重面板）。没有这层，模型再准也会被用户骂。（Ch 5, Ch 6 "The Value of Judgment"）

3. **AI 什么时候替代人、什么时候互补人？**
   - **替代**：任务是 Known Knowns（数据丰富、规则稳定、目标客观），且预测精度已经跨过"够用"阈值。（Ch 4, Ch 15 "Fully Automated Decision Making"）
   - **互补**：任务是 Known Unknowns（人知道自己不知道，机器可以在需要时"呼叫人类"）。这是 human-in-the-loop 设计的落点。（Ch 15）
   - **警告区**：Unknown Knowns（机器学到了错误的相关性但自己不知道，例如「反向因果」）—— 全自动会危险失败。**PM 必须在这类场景保留人工兜底**。（Ch 6, Ch 15）

4. **点状 AI 化 vs 全面 AI 化的顺序**：**先拆任务，逐个跑 ROI，只从边缘任务开始**。**不要在 job 层或 strategy 层直接上 AI**，那是失败模式。全面重构工作流只有当"预测便宜"能翻转核心业务的战略 trade-off 时才值得（如 Amazon 从"shopping-then-shipping"翻转到"shipping-then-shopping"）。（Ch 13, Ch 16 "Deconstructing Workflows", Ch 17 "Decision Strategy"）

5. **数据护城河来自"持续生成的 Input + Feedback"，不是历史训练集。**训练数据是"烧掉的数据"（burned data）。做 AI 产品的商业化设计时，PM 应设计**持续采集新输入、新反馈的机制**（默认打开的行为埋点、结果回流按钮、影子模式收集），这才是可复利的资产。（Ch 4, Ch 18 "Managing AI Risk"）

6. **组织边界规则**：**能写清 SLA 的部分外包**（预测服务、算力、标注），**判断类工作留在公司内部**（reward function 设计、异常仲裁）—— 因为 judgment 无法用刚性合约表达。（Ch 14 "Decomposing Decisions" / Part 4）

7. **AI 自动化的最佳 ROI 三条件**（同时满足才值得全自动）：
   (a) 预测是流程里最后一个未自动化的环节；
   (b) 场景对速度极敏感、人类反应时间是瓶颈；
   (c) 通信延迟大到人类介入不现实（如工业机器人、无人机）。（Ch 15 "Fully Automated Decision Making"）

8. **AI 战略投入的三个前置条件**：
   (a) 业务中存在一个由不确定性主导的核心 trade-off；
   (b) 该 trade-off 是可以用"更好预测"翻转的；
   (c) 预测能被压到能真正翻转天平的精度。**三条不齐，AI 只能带来运营效率的边际改善，不构成战略价值。**（Ch 17 "Decision Strategy"）

9. **三大 trade-off 是 PM 必须显式做的选择**：**更多数据 → 更少隐私；更快预测 → 更低准确率；更强自主 → 更弱控制**。不存在"全都要"的解，写 PRD 时必须写清你站哪一边。（Introduction, Ch 18 "Managing AI Risk"）

10. **上线时机决策**：太早上线 → 精度差伤口碑；太晚上线 → 拿不到反馈数据、被有数据回路的竞对超车。**判断标准**：预测精度是否已经跨过"用户能容忍的最低阈值"，且反馈回路已经搭好。（Ch 16, Ch 18）

---

## 四、检查清单

### 清单 A：某个功能 / 任务是否适合 AI 化（Ch 13 + Ch 15）

嵌入需求评审环节，逐条打勾。**任何一项打叉，暂缓 AI 化。**

- [ ] **预测目标可精确定义**：能用一句话写出「模型输出 X 的概率 / 数值 / 分类」
- [ ] **Judgment 表达机制已设计**：假阳 / 假阴的相对代价谁定？怎么让用户 / 运营调？
- [ ] **Action 明确**：预测出来后系统或人做什么下一步动作？
- [ ] **Outcome 可度量且反馈周期 < 迭代周期**：能拿到 label 反哺训练
- [ ] **Input 数据可得**：生产环境冷启是否有数据？
- [ ] **Training 数据现成或可低成本获取**
- [ ] **Feedback 回路存在**：用户会不会自然产生正 / 负反馈？
- [ ] **失败可承受**：这个任务是 Known Knowns 还是 Unknown Knowns？若是后者，是否留了人工兜底？

### 清单 B：AI 影响价值链 / 商业模式评估（Ch 4, Ch 5, Ch 16, Ch 17）

用于评估「这个 AI 功能对我们的战略意义有多大」。

- [ ] 这个环节现在的核心成本是不是「人做预测」？（若否 → AI 化只是锦上添花）
- [ ] 预测便宜后，哪些互补品（Judgment 工具 / Data 采集 / Action 执行）我们能同步升级？
- [ ] 我们是否有持续生成 Input / Feedback 数据的机制？（若否 → 没有护城河）
- [ ] 是否存在一个由不确定性主导的战略 trade-off，能被"更准的预测"翻转？
- [ ] 我们是否有能力承担 trade-off 明确的一侧（数据 vs 隐私 / 速度 vs 精度 / 自主 vs 控制）？
- [ ] 是否有反向因果（Unknown Knowns）风险，需要保留人工监督？
- [ ] 是否评估过上线时机：精度够 + 反馈回路准备好？

---

## 五、反模式（书中明确警告）

1. **只看预测精度，不看 Judgment 成本**（Ch 5, Ch 6）：模型 95% 准确、但每个错误代价巨大且无人兜底 → 净价值为负。PM 常见错误：忽略 false-positive / false-negative 的相对成本设计。

2. **忽略 AI 犯错的下游成本**（Ch 6, Ch 18）：反向因果、数据分布漂移、对抗攻击。全自动化系统在 Unknown Knowns 场景会"自信地失败"。

3. **在 job 层 / strategy 层直接上 AI**（Ch 13, Ch 16）：正确做法是把 job 拆成任务再逐个评估。**"AI implementation fails when applied at the job or strategy level."**

4. **用 AI 优化了错误的目标 / 不完整的奖励函数**（Ch 8 "The Value of Judgment", Ch 14）：Reward function engineering 才是真正难的活。PM 若只喊"提升 CTR"而没定义完整 reward，模型会用你没想到的方式满足指标。

5. **把训练数据当护城河、不建反馈回路**（Ch 4, Ch 18）：训练数据是"烧掉的"，真正的复利资产是持续新增的 Input + Feedback。

6. **默认"更多数据一定更好"**（Introduction, Ch 18）：忽略"数据 ↔ 隐私"、"速度 ↔ 精度"、"自主 ↔ 控制"三条 trade-off，导致产品在合规、体验或安全上暴雷。

---

## 六、边界声明

- **视角**：这本书是**经济学视角**（作者三位均为 Rotman School 经济学者）。它擅长回答「要不要做、往哪儿投、如何定价」的战略级问题，**不涉及**工程实现、模型选型、Prompt 工程、Eval 方法。
- **时代**：2022 年第 2 版对**预测型 AI**（分类 / 回归 / 推荐 / 检测）覆盖最强；对 **LLM / 生成式 AI 覆盖有限**。生成式 AI 场景下，「Prediction」这一格常常要重新解释为「补全下一个 token / 生成符合意图的内容」，Judgment 部分（人类偏好、RLHF、Reward Model）**在书中框架下仍成立且甚至更加中心**，但书中没有专门讨论 chat、agent、workflow 编排。
- **适用范围**：AI PM 的**战略层 / 需求评审层 / 商业化设计层**决策。
- **不适用**：具体 UX 细节、prompt / few-shot 设计、多轮对话状态管理、Agent 架构 —— 用套件里其他 Skill（如 karpathy 视角、ai-engineering 方法论）。
- **地域偏差**：作者以北美 / 加拿大企业案例为主（Google, Amazon, Goldman Sachs, 加拿大 MBA 招生），对中国市场特有的数据合规、消费者行为模式着墨少，PM 需自行本地化。

---

## 七、来源清单

- Agrawal, Gans, Goldfarb (2022). *Prediction Machines: The Simple Economics of Artificial Intelligence*, Updated and Expanded Edition, HBR Press. 章节引用如上。
- Agrawal, Gans, Goldfarb (2018-04-17). "A Simple Tool to Start Making Decisions with the Help of AI", *Harvard Business Review*. 用于 AI Canvas 7 格。
- Rotman School Insights Hub — Prediction Machines 官方专题页。
- The Strategy Bridge (2019-02-13). "Reviewing Prediction Machines: Cutting Through the AI Hype" —— 用于结构（19 章 / 5 部）与适用性批评的交叉验证。
- Antoine Buteau — Prediction Machines Book Summary（Decision Anatomy / Rumsfeld Matrix / Reward Function Engineering 交叉验证）。
- USAFA ECE 386 Course Notes — Prediction Machines 章节（cheap prediction + complements 交叉验证）。
- rasa.io — AI Canvas 教学文章（7 格定义交叉验证）。

> 本 Skill 由 career-skill-factory 生成。内容提炼自原著，版权归原作者，仅供个人学习。
