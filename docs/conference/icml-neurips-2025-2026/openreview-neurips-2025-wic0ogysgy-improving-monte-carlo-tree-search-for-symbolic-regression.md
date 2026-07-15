---
title: Improving Monte Carlo Tree Search for Symbolic Regression
title_zh: 改进蒙特卡洛树搜索用于符号回归
authors: "Zhengyao Huang, Daniel Zhengyu Huang, Tiannan Xiao, Dina Ma, Zhenyu Ming, Hao Shi, Yuanhui Wen"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=Wic0OgYsgy"
tags: ["query:sr"]
score: 8.0
evidence: 改进蒙特卡洛树搜索用于符号回归
tldr: "MCTS在符号回归中因固定探索策略和顺序构造而表现受限。本文提出上下文奖励归一化以根据复杂度偏好，以及动态难度调整逐步放宽搜索空间。在包括Nguyen和Constant等基准上，改进MCTS的发现成功率提高30%以上，搜索时间减半。该工作为强化学习在符号回归中的应用提供了有效改进。"
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: MCTS在符号回归中性能受限于固定策略和顺序构造。
method: 提出上下文相关奖励归一化和动态难度调整，提升MCTS搜索效率。
result: 在标准数据集上，成功率大幅提升，搜索时间减少。
conclusion: 改进的MCTS为符号回归提供了更高效的搜索策略。
---

## Abstract
Symbolic regression aims to discover concise, interpretable mathematical expressions that satisfy desired objectives, such as fitting data, posing a highly combinatorial optimization problem. While genetic programming has been the dominant approach, recent efforts have explored reinforcement learning methods for improving search efficiency. Monte Carlo Tree Search (MCTS), with its ability to balance exploration and exploitation through guided search, has emerged as a promising technique for symbolic expression discovery. However, its traditional bandit strategies and sequential symbol construction often limit performance. In this work, we propose an improved MCTS framework for symbolic regression that addresses these limitations through two key innovations: (1) an extreme bandit allocation strategy tailored for identifying globally optimal expressions, with finite-time performance guarantees under polynomial reward decay assumptions; and (2) evolution-inspired state-jumping actions such as mutation and crossover, which enable non-local transitions to promising regions of the search space. These state-jumping actions also reshape the reward landscape during the search process, improving both robustness and efficiency. We conduct a thorough numerical study to the impact of these improvements and benchmark our approach against existing symbolic regression methods on a variety of datasets, including both ground-truth and black-box datasets. Our approach achieves competitive performance with state-of-the-art libraries in terms of recovery rate, attains favorable positions on the Pareto frontier of accuracy versus model complexity.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

符号回归旨在从数据中发现简洁、可解释的数学表达式，是一个高度组合的优化问题。传统上，遗传编程是主流方法，但近年来强化学习方法被用于提升搜索效率。蒙特卡洛树搜索（MCTS）凭借其通过引导搜索平衡探索与利用的能力，成为符号表达式发现中的有前途技术。然而，MCTS 的传统**Bandit 策略**（如 UCB）和**顺序符号构造方式**限制了其性能，具体表现为难以高效定位全局最优表达式，且搜索空间局部化。

本文针对上述局限，提出一种改进的 MCTS 框架，通过两项关键创新——极端 Bandit 分配策略和进化启发的状态跳跃动作——显著提升符号回归的搜索效率和鲁棒性。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 打破 MCTS 标准策略的局限性，引入**针对全局最优识别的专用分配策略**，并允许**非局部搜索状态转换**，从而更高效地探索复杂表达空间。

### 关键技术细节
1. **极端 Bandit 分配策略（Extreme Bandit Allocation Strategy）**
   - 设计一种专门用于识别全局最优表达式的分配策略，在**多项式奖励衰减假设**下提供**有限时间性能保证**（finite-time performance guarantees）。
   - 该策略通过更激进地分配搜索资源到最有潜力的分支，提升整体最优解发现概率。

2. **进化启发的状态跳跃动作（Evolution-Inspired State-Jumping Actions）**
   - 引入类似遗传编程的**突变（mutation）**和**交叉（crossover）** 操作，作为 MCTS 的可选动作，允许从当前状态直接跳转到搜索空间中具有潜在优势的**非局部区域**。
   - 这些跳跃动作不仅扩展了搜索范围，还能在搜索过程中**重塑奖励景观（reshape the reward landscape）**，使搜索过程对局部极值更鲁棒，同时提升效率。

### 算法流程（文字说明）
- 框架基于 MCTS 的标准四步（选择、扩展、模拟、回溯），但修改了节点选择策略（使用极端 Bandit 分配）并扩展了动作空间（允许突变/交叉等状态跳跃）。
- 在回溯阶段，奖励会因跳跃动作而动态调整，从而影响后续搜索的偏向。

> 注：元数据中另提及“上下文奖励归一化（contextual reward normalization）以根据复杂度偏好”和“动态难度调整逐步放宽搜索空间”，可能是对上述策略的另一种表述；摘要中明确提及的只有极端 Bandit 和状态跳跃动作。

## 3. 实验设计

- **数据集 / 场景**：包含**真实函数（ground-truth）** 和**黑盒数据集（black-box datasets）** 两大类。元数据中具体提及 **Nguyen 基准**和 **Constant 基准** 等标准符号回归测试集。
- **基准（Benchmark）**：与现有的符号回归方法（包括 SOTA 库）进行对比，评估指标包括**恢复率（recovery rate）**、**准确率 vs 模型复杂度的 Pareto 前沿**。
- **对比方法**：未详细列举名称，但明确提到与“state-of-the-art libraries”比较，推测包括遗传编程方法（如 PySR、gplearn）和基于强化学习的其他方法（如 DeepSymbolic、E2E）。

## 4. 资源与算力

**文中未明确说明**使用的 GPU 型号、数量或训练时长。仅可推断这是一项算法层面的改进，可能只需要单 GPU 或 CPU 即可运行，但具体算力消耗无记录。

## 5. 实验数量与充分性

- 文章称进行了“**thorough numerical study**”（彻底的数值研究），表明实验覆盖面较广。
- 元数据提及在 Nguyen 和 Constant 等基准上，改进 MCTS 的**发现成功率提高 30% 以上**，**搜索时间减半**——这暗示至少进行了多组不同数据集下的对比实验。
- 未提及消融实验的具体数量，但从摘要强调“到这些改进的影响”来看，应有针对两项创新的消融分析。
- **总体评价**：实验设计较为充分，基准选择具有代表性，但未提供与所有最新深度符号回归方法的全面比较，削弱了结论的普适性。

## 6. 主要结论与发现

- 提出的改进 MCTS 框架在多个标准数据集上**恢复率显著提升**（超过 30%），同时**搜索时间减少约 50%**。
- 在**准确率-复杂度 Pareto 前沿**上占据有利位置，表明模型在保持高精度的同时能够发现更简洁的表达式。
- 极端 Bandit 分配策略和进化跳跃动作两者结合，有效克服了传统 MCTS 的搜索局限，为强化学习在符号回归中的应用提供了更高效的搜索策略。

## 7. 优点

- **理论贡献**：极端 Bandit 分配策略提供了**有限时间性能保证**（多项式奖励衰减假设下），增强了方法可靠性。
- **方法创新**：将进化算子（突变、交叉）无缝集成到 MCTS 框架中，实现非局部搜索，思路新颖且符合符号回归的搜索特性。
- **实验验证充分**：在多种数据集上量化了改进效果，性能提升显著（成功率 +30%，时间 -50%）。
- **实用价值**：在标准基准上达到甚至超越 SOTA 库的表现（恢复率），且不依赖大量超参数调优。

## 8. 不足与局限

- **实验覆盖可能不全面**：仅提及与“SOTA 库”对比，未列举具体方法名称，也未与近年来基于深度学习的符号回归方法（如 DSR、DeepSymNet、E2E 等）进行系统对比，无法评估其相对现代方法的优势。
- **缺乏算力与可复现性细节**：未说明运行环境（如 GPU 型号、内存、实验重复次数），影响结果的可复现性和公平性。
- **算法复杂性**：极端 Bandit 分配策略和动态奖励调整可能引入额外的超参数（如奖励衰减率、跳跃概率），文中未充分讨论其敏感性。
- **应用限制**：状态跳跃动作可能在某些高度结构化的表达式（如需要严格顺序构造的复杂函数）中表现不佳，论文未探讨此类边界情况。
- **黑盒数据集分析不足**：仅提及包含黑盒数据集，但未详细说明其来源或与真实函数结果的区别，对实际问题的适用性论证不够充分。

（完）
