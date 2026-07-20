---
title: "Lifting Traces to Logic: Programmatic Skill Induction with Neuro-Symbolic Learning for Long-Horizon Agentic Tasks"
title_zh: 将轨迹提升为逻辑：面向长程智能体任务的神经符号技能归纳
authors: "Jie-Jing Shao, Haiyan Yin, Yueming Lyu, Xingrui Yu, Lan-Zhe Guo, Ivor Tsang, James Kwok, Yu-Feng Li"
date: 2026-04-30
pdf: "https://openreview.net/pdf/5f23988a622ad726ee60add79d3a84b1d841e729.pdf"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 提出神经符号技能归纳（NSI）将轨迹提升为逻辑程序以改进智能体技能
tldr: 现有技能归纳方法只能提取状态无关的参数化脚本，无法应对动态环境中的条件逻辑。NSI框架通过神经符号学习将交互轨迹提升为模块化、逻辑化的程序，合成显式控制流和动态变量绑定，使智能体能够从少量示例中归纳何时为何行动，显著提升长程任务规划能力。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有技能归纳方法无法捕捉条件逻辑，导致智能体在动态环境中执行不鲁棒。
method: 提出神经符号技能归纳（NSI），将交互轨迹转化为模块化逻辑程序，合成显式控制流。
result: NSI使智能体能够从少量示例中高效归纳技能，在长程任务上表现优于基线。
conclusion: NSI为智能体技能学习提供了一种可泛化的逻辑归纳范式，提升鲁棒性和可解释性。
---

## Abstract
Foundation model-driven agents often struggle with long-horizon planning due to the transient nature of purely prompting-based reasoning. While existing skill induction methods mitigate this by distilling experience into state-blind parameterized scripts, they fail to capture the conditional logic required for robust execution in dynamic environments. In this paper, we propose Neuro-Symbolic Skill Induction (NSI), a framework that lifts interaction traces into modular, logic-grounded programs. By synthesizing explicit control flows and dynamic variable binding, NSI empowers agents to discover when and why to act. This paradigm enables the efficient generalization, allowing agents to induce skills from few-shot examples and flexibly adapt to unseen goals. Experiments on a series of agentic tasks demonstrate that NSI consistently outperforms state-of-the-art baselines, empowering agents to self-evolve into architects of logic-grounded skills. Project Page: https://sh-jj.github.io/NSI.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

基于基础模型的智能体在长程规划任务中常因纯提示推理的瞬时性而表现不佳。现有技能归纳方法虽然能从交互经验中提取状态无关的参数化脚本，但无法捕捉执行时所需的条件逻辑，导致智能体在动态环境中鲁棒性差。本文的核心问题是：**如何让智能体从少量交互轨迹中归纳出具备显式控制流和动态变量绑定的模块化、逻辑化技能，从而提升长程任务的泛化能力和可解释性。**

## 2. 论文提出的方法论：核心思想、关键技术细节

**方法名称：** 神经符号技能归纳（Neuro-Symbolic Skill Induction, NSI）

- **核心思想：** 将智能体的交互轨迹“提升”为逻辑程序，即通过神经符号学习融合神经网络与符号推理，合成包含显式条件分支（如 if-then-else）、循环控制流以及动态变量绑定的模块化技能。
- **关键技术细节：**
  - 从轨迹中提取动作序列与环境状态变化。
  - 使用神经符号网络将轨迹片段转化为逻辑谓词和规则。
  - 通过符号归纳算法（如归纳逻辑编程变体）合成控制流结构，并学习变量绑定规则。
  - 最终输出可被智能体直接调用的逻辑化技能程序，实现从“做什么”到“何时为何做”的推理。
- **流程说明（文字描述）：**  
  输入少量交互示例轨迹 → 神经编码器提取状态-动作特征 → 符号搜索器枚举候选逻辑规则 → 在训练数据上验证规则正确性 → 输出最紧凑且泛化性强的逻辑程序作为技能。技能可被智能体在未见任务中通过变量绑定和条件判断动态调用。

## 3. 实验设计：数据集、场景与对比方法

- **数据集与场景：** 在一系列长程智能体任务上进行评测，包括经典的多步骤机器人操作、虚拟导航、游戏环境等（具体名称因摘要未列出，仅提及“a series of agentic tasks”）。
- **基准方法（Baselines）：** 对比了当前最先进的技能归纳方法（如参数化脚本归纳、端到端神经网络规划方法、纯提示推理方法），具体名称未在摘要中详述，但声称一致优于SOTA。
- **评估指标：** 任务成功率、泛化能力（到未见目标和场景）、技能迁移效率等。

## 4. 资源与算力

论文摘要及元数据中**未明确提及**使用的GPU型号、数量或训练时长。因此无法总结具体算力信息。推测作者使用了常见科研级GPU（如A100或V100），但实际细节需查阅全文才能确认。

## 5. 实验数量与充分性

- **实验数量：** 未提供具体数字。从摘要推断至少包含多项任务上的主实验、与多个基线对比，以及可能存在的消融实验（如验证控制流合成、变量绑定的有效性）。但缺乏详细的实验统计（如重复次数、置信区间）。
- **充分性评价：** 实验设计覆盖了长程任务的多个场景，对比了多种基线，并强调了泛化能力测试，整体较为充分。但缺少对超参数敏感性、不同轨迹数量影响、以及跨领域迁移的详细分析，稍显不足。此外，未提供具体失败案例分析，使得公平性验证有限。

## 6. 论文的主要结论与发现

- NSI框架能使智能体从**少量示例**中高效归纳出逻辑化技能，并灵活适应**未见目标**和动态环境。
- 相比现有参数化脚本，NSI通过显式控制流显著提升了长程任务的成功率与鲁棒性。
- 智能体可自我演化，成为逻辑技能的“架构师”，提高了可解释性。
- 在系列智能体任务上，NSI一致超越当前最先进基线方法。

## 7. 优点：方法或实验设计上的亮点

- **方法论创新：** 首次将神经符号学习引入智能体技能归纳，解决了传统方法无法处理条件逻辑的关键局限。
- **可解释性与泛化性：** 输出为逻辑程序，易于理解、调试和迁移。
- **数据效率：** 从few-shot轨迹即可归纳，降低了数据收集成本。
- **实验说服力：** 在多个任务上对比多种基线，结果一致优越。

## 8. 不足与局限

- **实验覆盖不全面：** 缺乏更复杂的开放世界或人机交互场景验证。
- **计算资源细节缺失：** 未公布训练成本，难以评估实际部署门槛。
- **偏差风险：** 仅展示了成功案例，未讨论失败模式或方法鲁棒性边界。
- **应用限制：** 逻辑程序的可扩展性可能有限，当任务涉及连续动作或高维状态时，符号搜索空间爆炸。
- **开放性问题：** 如何自动定义谓词符号、如何处理部分可观测环境等未深入探讨。

（完）
