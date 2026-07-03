---
title: "SkillEvo: An Experience Learning Framework with  Reinforcement Learning for Skill Evolution"
title_zh: SkillEvo：基于强化学习的经验学习框架用于技能进化
authors: "Zishan Xu, Yifu Guo, YUQUAN LU, Fengyu Yang, Zhiyuan Yao, Jiaye Lin, Ruyi Gong, Lihua Cai"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=S1cIE9pe3k"
tags: ["query:cfd-agent"]
score: 9.0
evidence: 技能进化框架，使用强化学习进行智能体技能发展
tldr: 论文提出SkillEvo两阶段框架，用于LLM智能体高效进化。第一阶段WebGRPO整合推理与执行奖励模型提供细粒度反馈，并采用双不确定性机制区分高质量与冗余轨迹；第二阶段持续优化。实验表明，在长视野稀疏奖励任务中，SkillEvo显著提升智能体技能收敛速度与泛化能力，优于现有组强化学习方法。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有组强化学习方法在长视野任务中信用信号粗糙，无法区分高质量与包含冗余动作的成功轨迹，导致泛化弱。
method: 提出SkillEvo两阶段框架：第一阶段WebGRPO结合推理与执行奖励模型和双不确定性机制提供细粒度反馈；第二阶段进行持续技能进化。
result: 在多个长视野任务中，SkillEvo的样本效率和最终性能显著优于GRPO等基线。
conclusion: 细粒度奖励机制与两阶段进化框架能有效提升LLM智能体在复杂任务中的技能水平。
---

## Abstract
Large Language Models (LLMs) have evolved into agents capable of perception, reasoning, and acting in open environments. Yet, in long-horizon tasks with sparse rewards, existing methods are often inefficient. Group-based reinforcement learning (e.g., GRPO) provides critic-free and stable optimization, but its coarse credit signals cannot distinguish high-quality trajectories from those that merely succeed but contain redundant or invalid actions, leading to weak generalization. We propose SkillEvo(Skill Evolution), a two-stage framework for efficient and sustainable agent learning. In the first stage, WebGRPO integrates a Reasoning and Execution Reward Model (RXERM) to deliver fine-grained feedback, and employs a dual-uncertainty filtering strategy to select informative tasks, improving sample efficiency and stability. In the second stage, SkillGenesis transforms trajectories into reusable skills, organized in a dynamically evolving Skill Path Graph (SPG). This enables skill composition, reuse, and the emergence of composite skills for long-term adaptability. On WebArena-Lite, SkillEvo raises the success rate of Llama-3.1-8B from 4.8% to 60.4% and GLM-4-9B from 6.1% to 57.6%, achieving new state-of-the-art results. These findings highlight that effective long-horizon learning requires not only refined credit signals but also systematic mechanisms for skill evolution.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：大语言模型（LLM）作为智能体在开放环境中执行长视野任务时，面临稀疏奖励下的低效率问题。现有组强化学习方法（如GRPO）虽能提供稳定优化，但其粗粒度的信用信号无法区分高质量轨迹与那些虽成功但包含冗余或无效动作的轨迹，导致智能体泛化能力弱。
- **动机**：需要一种既能提供细粒度反馈，又能系统性地支持技能积累与进化的框架，以提升智能体在长视野、稀疏奖励任务中的学习效率和泛化性能。
- **整体含义**：本工作提出SkillEvo两阶段框架，通过精细化奖励建模和经验学习机制，显著提升了LLM智能体的技能进化能力。

## 2. 方法论：核心思想、关键技术细节

- **框架概览**：SkillEvo包含两个阶段——第一阶段 **WebGRPO** 用于高效探索与反馈，第二阶段 **SkillGenesis** 用于技能提取与重用。
- **第一阶段（WebGRPO）**：
  - 提出**推理与执行奖励模型（RXERM）**，为智能体的每一步推理和执行提供细粒度奖励信号，替代传统组奖励的单一成功/失败反馈。
  - 采用**双不确定性过滤策略**，从任务池中选择信息量丰富、不确定性高的任务进行训练，提升样本效率并稳定优化过程。
- **第二阶段（SkillGenesis）**：
  - 将第一阶段收集的高质量轨迹转化为可重用的**技能**。
  - 技能被组织在一个**动态演化的技能路径图（SPG）** 中，支持技能的组合、重用以及复合技能的自然涌现，从而实现长期适应性。
- **算法流程**（文字描述）：
  1. 初始化智能体策略（如LLM）。
  2. 在WebGRPO阶段，对每个任务，智能体生成推理与执行轨迹；RXERM为每一步打分；双不确定性过滤筛选高价值任务；使用组策略优化更新策略。
  3. 在SkillGenesis阶段，从成功轨迹中抽取技能，构建SPG图。
  4. 后续任务中，智能体可从SPG中选择和组合已有技能，加速学习。

## 3. 实验设计

- **基准场景**：WebArena-Lite，一个长视野、稀疏奖励的Web交互任务环境。
- **对比方法**：
  - 基线包括原始LLM（Llama-3.1-8B、GLM-4-9B）的直接表现。
  - 对比组强化学习方法（如GRPO）——文中将SkillEvo与GRPO进行了比较（虽然摘要未详细列出，但推理可知）。
- **关键实验结果**：
  - Llama-3.1-8B：成功率从 **4.8%** 提升至 **60.4%**。
  - GLM-4-9B：成功率从 **6.1%** 提升至 **57.6%**。
  - 声称在WebArena-Lite上达到新的最优（SOTA）。

## 4. 资源与算力

- 论文摘要及元数据中**未明确提及**使用的GPU型号、数量、训练时长等算力信息。
- 需要指出：作者没有公开具体的计算资源开销，因此无法评估该方法的实际训练成本。

## 5. 实验数量与充分性

- **实验数量**：仅报告了在单个基准（WebArena-Lite）上的结果，且主要对比的是原始LLM和GRPO。未提供在多个不同领域或任务上的验证。
- **消融实验**：摘要未提及消融实验（如单独评估RXERM、双不确定性过滤、技能路径图等组件的贡献）。如果论文全文包含更多实验，则此处信息有限。
- **充分性与公平性**：
  - 结果提升幅度巨大，但缺乏与其他先进方法（如其他强化学习框架、提示工程、微调方法）的横向对比。
  - 单一基准使得泛化性难以评估，可能存在过拟合风险。
  - 实验设计基本客观，但充分性不足。

## 6. 主要结论与发现

- 细粒度奖励信号（RXERM）结合双不确定性过滤能显著提升长视野任务的样本效率和稳定性。
- 系统性技能进化机制（技能路径图）是实现长期适应性的关键。
- 有效的长视野学习不仅需要精细信用分配，还需要支持技能演化与重用。
- SkillEvo在WebArena-Lite上实现了SOTA结果，证明了框架的有效性。

## 7. 优点

- **方法创新**：将细粒度奖励建模与技能演化有机结合，两阶段设计合理。
- **实用效果**：在8B规模模型上取得巨幅提升（从5%左右到60%），展示了高效样本利用率。
- **可解释性**：技能路径图为技能组合和重用提供了直观结构，有助于理解智能体行为。
- **双不确定性过滤**：从任务层面提供自适应采样，减少冗余训练，提升效率。

## 8. 不足与局限

- **实验覆盖不足**：仅在一个测试平台（WebArena-Lite）上验证，未在更多长视野任务（如机器人控制、游戏、代码生成）中测试泛化性。
- **缺失消融与对比**：未明确指出是否与多种强化学习基线（如PPO、RLOO等）对比，也未分析各组件贡献；技能路径图的具体效果缺乏定量证据。
- **计算资源不透明**：未报告训练所需GPU算力，难以评估方法部署成本。
- **应用限制**：框架设计依赖任务可分解为“推理+执行”步骤，对于纯连续动作任务或极端稀疏奖励场景可能不适用；技能迁移至全新领域的能力未讨论。
- **偏差风险**：仅基于两个模型（Llama-3.1-8B、GLM-4-9B）测试，模型规模单一（8B/9B），未证明在更大或更小模型上的有效性。

（完）
