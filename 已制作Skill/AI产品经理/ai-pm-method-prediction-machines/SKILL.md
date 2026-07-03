---
name: ai-pm-method-prediction-machines
description: |
  《Prediction Machines: The Simple Economics of Artificial Intelligence》(Agrawal / Gans / Goldfarb,
  HBR Press 2022 更新版) 方法论。把"AI = Cheap Prediction"经济学视角转化为 AI PM 的战略级判断框架，
  用于：功能要不要 AI 化、AI 需求立项 canvas、AI 商业化与护城河判断、人机分工设计。
  每条规则标注原书章节。触发词：「Prediction Machines」「AI Canvas」「Prediction / Judgment / Action」
  「cheap prediction」「AI 战略」「AI 商业化」「人机分工」「反向因果」。
---

# 《Prediction Machines》· AI = 便宜的预测，Judgment 才是护城河

**核心论断**（第 2 章 "Cheap Changes Everything"）：算术便宜了 → 用得更多；分发便宜了 → 用得更多；**预测便宜了 → 用得更多，且预测的互补品（Judgment、Data、Action）升值，替代品（人工预测）贬值**。PM 的活是识别这个变化，不是"想一个 AI 功能"。

## 什么时候用我

| 场景 | 用哪个框架 |
|---|---|
| 判断某功能/环节要不要 AI 化 | 框架 1（P/J/A 三分法）+ 清单 A |
| 评估 AI 需求完整度、能不能立项 | 框架 2（AI Canvas 7 格） |
| 决定 AI 在价值链哪段撬动战略级价值 | 框架 3（Prediction × Complements × Trade-off） |
| AI 定价 / 商业化 / API 开放 | 规则 4/5/8 + 清单 B |
| 人机分工设计 | 规则 1/2/6 |
| **不适合**：UX 细节、Prompt 工程、多智能体架构、模型评测 —— 转其他 Skill |

## 核心框架

### 框架 1：Prediction / Judgment / Action 三分法（Part 2 Ch 7-8；Part 3 Ch 13）

任何决策 = **Input Data → Prediction → Judgment → Action → Outcome**（加 Training / Feedback 两条数据回路）。**AI 只擅长 Prediction，其他环节不会因为你用了 AI 就自动升级。**

**PM 用法**：
1. 把功能/工作流拆成一串子任务，标注 P / J / A 类型
2. 对每个子任务算：预测便宜下来能省/卖多少 · 判断成本会不会同步涨 · 行动能不能承接更多 if-then
3. **只挑 P 类高 ROI 且补足环节现成的做 AI 化**，J/A 环节保留人或另建工具

> 关键洞察（Ch 3, Ch 7）："Prediction is not decision. Decision requires judgment." PM 最常犯的错就是把整块决策外包给模型，实际上模型只完成了 P，J 和 A 的成本反而被推高。

### 框架 2：AI Canvas — 7 格立项模板（Part 3 Ch 13；HBR 2018-04）

在写 PRD 之前把 AI 功能拆进 7 格。**任何一格空着或含糊 → 要么补需求要么砍需求。**

| 格子 | PM 需要一句话回答 |
|---|---|
| **Prediction** | 模型输出 X 概率 / X 数值 / X 分类 |
| **Judgment** | 假阳性 vs 假阴性哪个更贵？谁来定？ |
| **Action** | 预测出来后自动触发还是人工确认？ |
| **Outcome** | 成功 / 失败度量方式 + 拿到 label 要多久 |
| **Input** | 生产环境实时喂什么？冷启用户怎么办？ |
| **Training** | 历史数据有吗？还是要买/标/造？ |
| **Feedback** | 用户是否愿意留下 label？回流怎么设计？ |

### 框架 3：Cheap Prediction → Complements 升值（Ch 2, 4, 5）

- **替代品（贬值）**：人工做预测（分析师人力评分、专家经验）
- **互补品（升值）**：Judgment（人类价值判断工具）、Data（持续更新的输入/反馈）、Action（下游执行能力）——**这些才是 AI 时代的定价权与护城河**
- **PM 决策规则**：AI 功能没有配套升级 Judgment 表达工具 / 数据回路 / 下游执行 API → 你只是让最便宜的东西又便宜了一点，用户价值近乎为零

## 决策规则（10 条，来源标注）

1. 只优化能被清晰定义的预测目标；Prediction 一格写不出"输出 X 的概率"→ 别做 AI 化（Ch 3, 13）
2. 预测越便宜，Judgment（用户偏好表达工具）越值钱 → 每次 AI 化必须同步投入设计"错报 vs 漏报哪个更贵"的机制（阈值滑块 / 反馈按钮 / 权重面板）（Ch 5, 6）
3. **替代 vs 互补**——
   - 替代：Known Knowns（数据丰富、规则稳定、目标客观）+ 预测精度过"够用"阈值（Ch 4, 15）
   - 互补：Known Unknowns（人知道自己不知道，机器可 "呼叫人类"）（Ch 15）
   - **警告区**：Unknown Knowns（机器学到错误相关性但自己不知道，如反向因果）→ 全自动会危险失败，必须留人工兜底（Ch 6, 15）
4. **点状 AI 化 vs 全面**：先拆任务、只从边缘任务开始跑 ROI。不要在 job 层 / strategy 层直接上 AI。全面重构只在"预测便宜能翻转核心业务 trade-off"时做（Amazon 从 shopping-then-shipping 翻转到 shipping-then-shopping）（Ch 13, 16, 17）
5. **数据护城河来自"持续生成的 Input + Feedback"，不是历史训练集**——训练数据是"烧掉的数据"。设计产品时要有默认打开的行为埋点、结果回流按钮、影子模式，这才是可复利资产（Ch 4, 18）
6. **组织边界**：能写清 SLA 的部分外包（预测服务、算力、标注）；判断类工作留在公司内部（reward function 设计、异常仲裁）——judgment 无法用刚性合约表达（Ch 14 / Part 4）
7. **全自动最佳 ROI 三条件**（同时满足）：(a) 预测是流程里最后一个未自动化的环节 (b) 场景对速度极敏感 (c) 通信延迟大到人类介入不现实（工业机器人、无人机）（Ch 15）
8. **AI 战略投入三前置条件**：(a) 业务存在由不确定性主导的核心 trade-off (b) 该 trade-off 可以用"更好预测"翻转 (c) 预测能被压到翻转天平的精度。**三条不齐，AI 只带来运营边际改善，不构成战略价值**（Ch 17）
9. **三大 trade-off 必须显式站队**：更多数据 → 更少隐私；更快预测 → 更低准确率；更强自主 → 更弱控制。不存在"全都要"（Introduction, Ch 18）
10. **上线时机**：太早 → 精度差伤口碑；太晚 → 拿不到反馈、被有数据回路的对手超车。判断标准 = 精度跨过用户能容忍最低阈值 + 反馈回路已搭好（Ch 16, 18）

## 检查清单

### 清单 A：某功能是否适合 AI 化（Ch 13 + 15）

任一打叉，暂缓 AI 化：
- [ ] 预测目标可精确定义（能写出"输出 X 的概率/数值/分类"）
- [ ] Judgment 表达机制已设计（假阳/假阴代价怎么调？）
- [ ] Action 明确（下一步动作是什么）
- [ ] Outcome 可度量且反馈周期 < 迭代周期
- [ ] Input 数据可得（冷启方案）
- [ ] Training 数据现成或可低成本获取
- [ ] Feedback 回路存在
- [ ] 失败可承受（Known Knowns 还是 Unknown Knowns？后者留人工兜底）

### 清单 B：AI 影响价值链 / 商业模式（Ch 4, 5, 16, 17）

- [ ] 现在核心成本是"人做预测"吗？（否 → AI 化只是锦上添花）
- [ ] 预测便宜后哪些互补品能同步升级？
- [ ] 是否有持续生成 Input / Feedback 的机制？（否 → 无护城河）
- [ ] 是否存在由不确定性主导的战略 trade-off，能被更准预测翻转？
- [ ] 是否能承担 trade-off 明确一侧（数据 vs 隐私 / 速度 vs 精度 / 自主 vs 控制）？
- [ ] 是否评估过反向因果（Unknown Knowns）风险，需保留人工监督？
- [ ] 是否评估过上线时机（精度够 + 反馈回路准备好）？

## 反模式（书中警告）

1. 只看预测精度，不看 Judgment 成本——模型 95% 准但错误代价大且无兜底 → 净价值为负（Ch 5, 6）
2. 忽略 AI 犯错的下游成本（反向因果、分布漂移、对抗攻击）—— Unknown Knowns 场景会"自信地失败"（Ch 6, 18）
3. **在 job 层 / strategy 层直接上 AI**：正确做法是拆 job 成任务再逐个评估。"AI implementation fails when applied at the job or strategy level."（Ch 13, 16）
4. 用 AI 优化了错误的目标 / 不完整的 reward——只喊"提升 CTR"没定义完整 reward，模型会用你没想到的方式满足指标（Ch 8, 14）
5. 把训练数据当护城河、不建反馈回路（Ch 4, 18）
6. 默认"更多数据一定更好"——忽略三大 trade-off，产品在合规/体验/安全上暴雷（Ch 18）

## 边界声明

- **视角**：经济学视角（三位作者均 Rotman School 经济学者）。擅长战略级问题（要不要做、往哪投、如何定价）；**不涉及**工程实现、模型选型、Prompt、Eval
- **时代**：2022 年第 2 版对**预测型 AI**（分类/回归/推荐/检测）覆盖最强；**对 LLM/生成式 AI 覆盖有限**。生成式 AI 场景下 Prediction 常要重新解释为"补全 token / 生成符合意图内容"，Judgment（人类偏好、RLHF、Reward Model）在书中框架下更加中心，但书中没专门讨论 chat/agent/workflow
- **适用**：AI PM 战略层 / 需求评审层 / 商业化设计层决策
- **不适用**：UX 细节、prompt / few-shot 设计、多轮对话状态、Agent 架构 —— 用套件里其他 Skill（Karpathy 视角、AI Engineering 方法论）
- **地域偏差**：北美/加拿大企业案例为主，中国市场特有的合规/消费行为着墨少，PM 需本地化

## 工作流

1. **匹配场景**（见"什么时候用我"表）
2. **补齐信息**：核心缺失（预测目标、判断代价、反馈机制）一次列出问；次要缺失用「[假设]」补全
3. **按框架输出中间产出**：P/J/A 任务清单表 / AI Canvas 填格 / trade-off 站队声明
4. **过清单 A/B**：标注未通过项
5. **收尾声明边界**：本书是经济学视角，工程细节请转 AI Engineering 方法论、能力判断请转 Karpathy 视角

## 完整方法论

详细章节引用、AI Canvas 图示、Rumsfeld Matrix、更多案例见 `references/framework.md`。

> 本 Skill 由 career-skill-factory 生成。内容提炼自原著，版权归原作者，仅供个人学习。
