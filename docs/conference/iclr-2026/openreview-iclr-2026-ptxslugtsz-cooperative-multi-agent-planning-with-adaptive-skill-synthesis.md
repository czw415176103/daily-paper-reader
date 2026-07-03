---
title: Cooperative Multi-Agent Planning with Adaptive Skill Synthesis
title_zh: 基于自适应技能合成的合作多智能体规划
authors: "Zhiyuan Li, Wenshuai Zhao, Joni Pajarinen"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=pTxSLUgTsz"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 基于视觉语言模型的自适应技能合成实现合作多智能体规划
tldr: 该论文提出COMPASS框架，集成视觉语言模型实现分散式闭环决策，动态生成和细化可解释的代码策略并存储于技能库，显著提升了多智能体系统的技能学习与迁移能力。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 多智能体强化学习面临样本效率和泛化问题。
method: 整合视觉语言模型生成代码策略，并通过实体传播确保鲁棒协调。
result: 在协作任务上表现出更好的可解释性和泛化性。
conclusion: 为多智能体技能学习提供了新范式。
---

## Abstract
Cooperative multi-agent reinforcement learning (MARL) struggles with sample efficiency, interpretability, and generalization. While Large Language Models (LLMs) offer powerful planning capabilities, their application has been hampered by a reliance on text-only inputs and a failure to handle the non-Markovian, partially observable nature of multi-agent tasks. We introduce COMPASS, a multi-agent framework that overcomes these limitations by integrating Vision-Language Models (VLMs) for decentralized, closed-loop decision-making. COMPASS dynamically generates and refines interpretable, code-based strategies stored in a skill library that is bootstrapped from expert demonstrations. To ensure robust coordination, it propagates entity information through a structured multi-hop communication protocol, allowing teams to build a coherent understanding from partial observations. Evaluated on the challenging SMACv2 benchmark, COMPASS significantly outperforms state-of-the-art MARL baselines. Notably, in the symmetric Protoss 5v5 task, COMPASS achieved a 57\% win rate, a 30 percentage point advantage over QMIX (27\%). Project page can be found at https://stellar-entremet-1720bb.netlify.app/.

---

## 论文详细总结（自动生成）

# 论文《Cooperative Multi-Agent Planning with Adaptive Skill Synthesis》中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：多智能体强化学习（MARL）面临样本效率低、可解释性差、泛化困难三大挑战。现有方法多依赖端到端深度强化学习，难以适应部分可观测、非马尔可夫的多智能体协作环境。
- **背景**：大语言模型（LLM）虽具有强大的规划能力，但受限于纯文本输入，无法处理视觉观测；且对多智能体任务中非马尔可夫和部分可观测特性应对不足。
- **整体含义**：提出一种集成视觉语言模型（VLM）的分散式闭环决策框架COMPASS，旨在提升多智能体系统的技能学习与迁移能力，同时保证可解释性和鲁棒协调。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：利用视觉语言模型（VLM）动态生成并细化可解释的代码策略，将策略存入技能库，并通过结构化多跳通信协议传播实体信息，实现团队从局部观测中构建一致理解。
- **关键技术细节**：
  - **技能库初始化**：从专家演示中引导（bootstrapping）构建初始技能库。
  - **代码策略生成与细化**：VLM根据当前观测和任务目标生成Python代码形式的策略，并在执行中根据反馈进行细化。
  - **实体传播与协调**：采用结构化多跳通信协议，智能体间交换实体信息，确保即使仅有局部观测也能形成全局一致的态势理解，从而实现鲁棒协调。
  - **闭环决策**：VLM生成策略后，智能体执行，根据新观测再次调用VLM调整，构成闭环。
- **算法流程（文字说明）**：
  1. 初始化：利用专家演示生成初始技能库（代码策略）。
  2. 每步决策：每个智能体通过视觉观测和通信收集信息，VLM根据当前状态和任务从技能库中检索或生成新代码策略。
  3. 执行与反馈：智能体执行策略，记录结果；若失败或低效，VLM根据反馈细化策略并更新技能库。
  4. 协调：通过多跳通信传播实体信息，确保团队决策一致。

## 3. 实验设计：数据集/场景、Benchmark、对比方法
- **数据集/场景**：使用SMACv2（StarCraft Multi-Agent Challenge v2）基准测试，这是一个具有挑战性的多智能体协作环境。
- **Benchmark**：SMACv2，包含多种不对称和对战场景。
- **对比方法**：与最先进的MARL基线进行对比，包括QMIX（当前主流方法）。具体对比了哪些基线未详细说明，但提及“显著优于SOTA MARL基线”。
- **典型任务**：对称Protoss 5v5任务，COMPASS胜率为57%，而QMIX为27%（提升30个百分点）。

## 4. 资源与算力
- **文中未明确说明**：摘要及元数据中未提及GPU型号、数量及训练时长。但通常这类VLM框架需要较大算力，具体细节未提供。

## 5. 实验数量与充分性
- **实验数量**：摘要仅重点展示了一个典型任务（Protoss 5v5）的对比结果，并声称在多个SMACv2任务上显著超越基线。未列举完整的实验列表（如不同地图、不同智能体数量、消融实验等）。
- **充分性评价**：
  - **充分**：使用了公认的标准基准SMACv2，并展示了与强基线QMIX的显著性能差距，初步证明方法有效性。
  - **不足**：缺少消融实验来验证各组件（如技能库、多跳通信、VLM生成细化）的贡献；未报告方差、多次重复等统计细节；仅提及一个具体数字，其余结果未量化呈现。因此，实验充分性有限，需要更全面的验证。

## 6. 主要结论与发现
- COMPASS框架显著优于现有SOTA MARL方法，尤其在对称Protoss 5v5任务中胜率高出30个百分点。
- 通过集成VLM和结构化通信，COMPASS能有效处理部分可观测、非马尔可夫的多智能体协作，同时生成可解释的代码策略，提升泛化性。
- 技能库的引导式构建和动态细化机制使得技能学习与迁移更为高效。

## 7. 优点：方法或实验设计亮点
- **方法论亮点**：
  - 引入VLM解决传统LLM无法处理视觉输入的问题，适用于视觉观测的MARL环境。
  - 代码策略具有天然可解释性，便于人类理解和调试。
  - 多跳通信协议在保持分散性的同时实现全局信息共享，提升协调鲁棒性。
  - 技能库机制支持知识复用和持续学习，具有迁移潜力。
- **实验设计亮点**：选择了公认难度较高的SMACv2基准，与经典QMIX对比并取得显著优势。

## 8. 不足与局限
- **实验覆盖不足**：仅公开了一个具体数字结果，缺乏完整任务列表、统计显著性、消融研究、泛化到其他环境（如多机器人协作）的验证。
- **偏差风险**：可能只展示了最有利的结果，未报告失败案例或效果不佳的场景。
- **应用限制**：依赖VLM的在线推理和代码生成，实时性和计算开销可能较大，不适用于对延迟敏感或资源受限的应用；技能库的构建与维护需要专家演示，这在实际中可能难以获取。
- **可复现性**：未提供完整代码或训练细节（如超参数、计算资源），影响他人复现。

（完）
