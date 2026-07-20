---
title: "Learning to Share: Selective Memory for Efficient Parallel Agentic Systems"
title_zh: 学会共享：高效并行智能体系统的选择性记忆
authors: "Joseph Fioresi, Parth Parag Kulkarni, Ashmal Vayani, Song Wang, Mubarak Shah"
date: 2026-04-30
pdf: "https://openreview.net/pdf/36afa45a025ea9f91750ce099712175131a4547b.pdf"
tags: ["query:cfd-agent"]
score: 6.0
evidence: 并行多智能体系统的选择性记忆共享
tldr: 本文提出Learning to Share（LTS），一种为并行多智能体系统设计的学习型共享记忆机制。不同智能体团队在处理相似子问题时，通过选择性记忆共享避免重复计算，同时控制上下文增长。在多个复杂任务上，LTS显著降低了计算开销，同时保持甚至提升了任务表现，为高效多智能体协作提供了新思路。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 并行多智能体系统中不同团队独立推理相似子问题导致大量重复计算，效率低下。
method: 提出可学习的共享记忆机制，选择性跨团队重用信息并控制上下文增长。
result: 在多个任务中降低了计算成本，同时保持或提升了解决方案质量。
conclusion: 选择性记忆共享是提升并行多智能体系统效率的有效手段。
---

## Abstract
Agentic systems solve complex tasks by coordinating multiple agents that iteratively reason, invoke tools, and exchange intermediate results. To improve robustness and solution quality, recent approaches deploy multiple agent teams running in parallel to explore diverse reasoning trajectories. However, parallel execution comes at a significant computational cost: when different teams independently reason about similar sub-problems or execute analogous steps, they repeatedly perform substantial overlapping computation. To address these limitations, in this paper, we propose Learning to Share (LTS), a learned shared-memory mechanism for parallel agentic frameworks that enables selective cross-team information reuse while controlling context growth. LTS introduces a global memory bank accessible to all teams and a lightweight controller that decides whether intermediate agent steps should be added to memory or not. The controller is trained using stepwise reinforcement learning with usage-aware credit assignment, allowing it to identify information that is globally useful across parallel executions. Experiments on the AssistantBench and GAIA benchmarks show that LTS significantly reduces overall runtime while matching or improving task performance compared to memory-free parallel baselines, demonstrating that learned memory admission is an effective strategy for improving the efficiency of parallel agentic systems.

---

## 论文详细总结（自动生成）

# 论文总结

## 1. 核心问题与整体含义

- **研究动机**：现代智能体系统（Agentic Systems）通过并行运行多个智能体团队来探索不同的推理轨迹，以提高复杂任务求解的鲁棒性和解决方案质量。然而，这种并行执行方式引入了巨大的计算开销——不同团队在独立推理相似子问题或执行类似步骤时，会反复进行大量重叠计算，导致效率低下。
- **核心问题**：如何在不牺牲任务性能的前提下，大幅降低并行多智能体系统因重复计算产生的计算成本。
- **整体含义**：本文提出了一种选择性记忆共享机制，让不同团队能够跨团队复用中间信息，同时控制上下文增长，为构建高效、可扩展的并行智能体系统提供了新思路。

## 2. 方法论

- **核心思想**：引入一个**全局记忆库（global memory bank）**，所有并行团队均可访问；同时设计一个轻量级**控制器（controller）**，动态决策每个中间步骤是否应存入记忆库，从而实现选择性跨团队信息复用，并抑制上下文无限制膨胀。
- **关键技术细节**：
  - 控制器采用**逐步强化学习（stepwise reinforcement learning）**进行训练，并引入**使用意识信用分配（usage-aware credit assignment）**机制，使控制器能识别出对多个团队全局有用的信息，同时忽略局部或冗余信息。
  - 记忆库中的条目可以被后续团队检索并用于指导自身推理，从而避免重复计算。
- **算法流程（文字说明）**：每个智能体团队在推理过程中产生中间步骤（如工具调用结果、推理片段）→ 控制器评估该步骤的全局效用 → 若判定有用则写入全局记忆库 → 后续团队可查询记忆库获取已计算的相关知识 → 经过多轮训练后，控制器学会权衡存储收益与上下文开销。

## 3. 实验设计

- **基准数据集/场景**：在 **AssistantBench** 和 **GAIA** 两个复杂任务基准上评估。
- **对比方法**：主要与**无记忆的并行基线（memory-free parallel baselines）**进行对比，即原始无共享记忆的并行智能体系统。
- **评估指标**：总运行时间（计算效率）以及任务完成质量（如准确率、解决方案质量）。

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。推测本文作为ICML 2026投稿，可能涉及大规模训练，但缺乏公开细节。

## 5. 实验数量与充分性

- **实验数量**：提到了两个主流基准（AssistantBench 和 GAIA）的实验结果；但未描述是否包含多个任务变体、不同并行团队数量、不同记忆大小等消融实验。从短摘要看，可能还有更多消融分析或被截断，但现有信息不完整。
- **充分性判断**：仅从摘要无法判断实验的全面性。两个基准虽具代表性，但缺少对记忆策略超参数敏感性、扩展到大团队规模等的详细分析，其充分性存疑。若论文内部包含更多消融（如不同控制器结构、奖励设计对比），则可能较充分，但需查看全文。

## 6. 主要结论与发现

- 相比无记忆的并行基线，LTS 能够**显著降低总体运行时间**，同时**保持或提升任务性能**。
- 证明了**学习型记忆准入策略**是提升并行智能体系统效率的有效手段——控制器能自主识别跨团队可重用的中间计算，避免重复劳动。

## 7. 优点

- **创新性**：将选择性记忆共享与强化学习相结合，首次系统性地解决并行多智能体中的重复计算问题，避免了简单共享带来的上下文爆炸。
- **实用性**：轻量级控制器开销低，易于集成到现有并行智能体框架中。
- **性能**：在提升效率的同时未牺牲质量，甚至能通过记忆复用带来的全局信息改善结果。

## 8. 不足与局限

- **实验覆盖不够详细**：仅提及两个基准，缺少对更大规模（如百团队级别）或更复杂场景（如异构智能体、动态规划）的验证；未报告统计显著性、方差等。
- **偏差风险**：训练过程依赖逐步强化学习，可能对奖励设计敏感；若使用偏好数据或人为标注，存在主观偏差风险。
- **应用限制**：记忆库的检索机制可能引入额外延迟，且当任务间差异极大时，复用价值可能降低；论文未讨论记忆容量限制及长尾场景。
- **算力与可复现性信息缺失**：未提供训练超参数、硬件配置，不利于其他研究者复现。

（完）
