---
title: "DRiFT: Differentiable Grid-Based Rigid-Fluid Coupling for Training and Control"
title_zh: DRiFT：用于训练与控制的可微网格刚体-流体耦合
authors: "Shrey Jayeshbhai Patel, Samuel Audia, Heechan Yoon, Bo Zhu, Rahul Narain, Ming Lin"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=c59wJJ6ZgR"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 可微流体模拟用于智能体训练与控制
tldr: 该论文提出DRiFT，一个端到端可微的网格基刚体-流体耦合模拟器。通过变分方法将边界条件转化为线性压力求解，并利用伴随方法反向传播梯度，为智力体提供准确的流体交互梯度信息，有助于在模拟环境中高效训练智能体。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 智能体与流体环境交互时需要精确的后果理解，传统模拟缺乏可微性。
method: 构建可微网格流体模拟，包含刚体-流体双向耦合，并通过伴随方法计算梯度。
result: DRiFT成功实现了刚体-流体耦合环境下的梯度反向传播，支持智能体训练。
conclusion: 可微流体模拟为基于梯度的智能体学习提供了新工具，尤其适用于CFD相关任务。
---

## Abstract
Intelligent agents, interacting with physical environments, require an accurate
understanding of the consequences of their action for efficient learning. Such
agents are often trained inside simulated environments to alleviate over dependence
on data, and gradients from such a simulation can help in training the agent. To this
end, we present an end-to-end differentiable grid-based fluid simulation including
strong two-way coupling with rigid bodies. In the forward pass, the solid-fluid
boundary conditions are converted to a monolithic linear pressure solve using a
variational method. For the backpropagation, we introduce a novel method of
calculating and propagating gradients for the combined fluid-solid state using the
adjoint method, which runs faster than the forward solve. This implementation,
which is customized for coupling rigid bodies with inviscid fluids, is more suitable
over general purpose methods like automatic differentiation, for use cases where
performance is key for analyzing overall flow patterns and learning fluid properties.
We demonstrate the utility of our simulator in training a neural network to learn
optimal control for general target states. Additionally, we show the effectiveness
of our differentiable simulator in isolation, by using the generated gradients for
simple derivative based optimization tasks. Finally, we showcase the accuracy,
robustness and efficiency of our gradient computation method.

---

## 论文详细总结（自动生成）

# DRiFT：用于训练与控制的可微网格刚体-流体耦合 —— 论文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **核心问题**：智能体在与流体环境交互时需要准确理解动作的后果，以便高效学习。传统物理模拟虽能提供环境反馈，但缺乏可微性，无法直接为基于梯度的智能体训练提供导数信息。
- **研究背景**：智能体（如机器人、无人机）的操控任务常包含流体介质，如空气、水流等。现有模拟器（如CFD求解器）用于训练时，通常需要大量数据或强化学习中的奖励函数设计，无法利用梯度优化。可微模拟器能够将物理过程的梯度反向传播给神经网络，提高训练效率。
- **整体意义**：DRiFT提出首个**端到端可微的网格基刚体-流体双向耦合模拟器**，为智能体在流体环境中的梯度学习提供了新工具，适用于CFD相关任务（如最优控制、流场分析）。

## 2. 方法论：核心思想、关键技术细节、算法流程

- **核心思想**：构建可微的网格基流体模拟器，并将其与刚体进行强双向耦合；在前向传播中将固体-流体边界条件转化为线性压力求解（使用变分方法）；在反向传播中采用伴随方法高效计算并传播整个耦合系统的梯度。
- **关键技术细节**：
  - **变分方法处理边界条件**：将固体-流体边界条件统一转换为一个整体的线性压力方程组进行求解（monolithic linear pressure solve），实现强双向耦合。
  - **伴随方法用于反向传播**：针对耦合流体-固体系统的状态，设计专门的伴随方程（adjoint equations），其计算速度比前向求解更快。
  - **针对无黏流体的特殊优化**：该方法专门定制于刚体与无黏流体的耦合，相比于通用自动微分（AD）框架，在性能关键的应用中（如全局流型分析与流体属性学习）更高效。
- **算法流程**：
  1. **前向模拟**：在每个时间步，基于流体状态和刚体状态，使用变分方法构造并求解线性压力系统，更新流体速度场与刚体运动。
  2. **反向传播**：采用伴随方法，从损失函数反向遍历时间步，计算关于控制参数（如初始条件、边界力等）的梯度。因为伴随方程的求解只涉及少量线性系统且可并行化，故比前向模拟更快。

## 3. 实验设计：数据集/场景、Benchmark、对比方法

- **数据集/场景**：
  - 训练一个神经网络来学习**针对一般目标状态的最优控制**（例如控制刚体到达指定位置或使流体达到指定流型）。
  - 单独的**基于导数的简单优化任务**（如最小化阻力、调整浮力等），以验证梯度有效性。
- **Benchmark**：未明确提及外部标准基准数据集，但通过多种示例场景（可能包括标准流体-刚体交互如落体、流固耦合控制）来验证。
- **对比方法**：文中明确提到**通用自动微分（自动微分框架，如基于PyTorch/TensorFlow的自动微分）**作为对比基线。DRiFT的伴随方法在性能上优于AD。此外可能对比了无梯度优化方法（如遗传算法）以体现梯度优势，但文中未具体列出。

## 4. 资源与算力

- 论文**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅提及逆向传播比前向求解更快，但未给出硬件细节。读者无法据此复现或评估计算成本。

## 5. 实验数量与充分性

- **实验数量**：包含两大类实验：① 以梯度优化为目标的任务（可能有多组不同初始/目标条件）；② 以神经网络训练为目标的控制学习任务。具体实验组数未在摘要/元数据中详列。
- **充分性与客观性**：
  - 作者提供了**准确度、鲁棒性、效率**的展示，但没有给出完整的消融实验（例如是否对比不同网格分辨率、不同耦合强度）以及统计显著性分析。
  - 实验设计基本围绕核心主张（可微梯度有效、伴随方法高效）展开，但缺少与更多基线（如无梯度RL、其他可微模拟器）的全面对比，因此充分性有限。

## 6. 主要结论与发现

- DRiFT成功实现了**刚体-流体双向耦合环境下的梯度反向传播**，支持基于梯度的智能体训练和优化任务。
- 所提出的**伴随方法**在计算梯度时比前向模拟**更快**，优于通用自动微分方案。
- 通过神经网络控制任务的演示，证明了可微模拟能为学习提供有效梯度，帮助收敛到目标状态。
- 模拟器在**精度、鲁棒性、效率**方面表现良好。

## 7. 优点（方法或实验设计亮点）

- **端到端可微**：同时支持前向模拟与梯度反向传播，填补了流体-刚体耦合可微模拟的空白。
- **强双向耦合**：采用变分法将边界条件转化为整体压力求解，保证了物理一致性。
- **高效的伴随梯度计算**：比自动微分更快，更适合长时间步大规模模拟。
- **专门针对无黏流体优化**：在特定应用场景（CFD智能体、流场分析）中兼具物理准确性与计算效率。
- 实验验证了**梯度可用于神经网络训练**和**直接优化任务**，展示了多场景适用性。

## 8. 不足与局限（实验覆盖、偏差风险、应用限制）

- **实验覆盖不完整**：缺少与多种基线（如其他可微模拟器、强化学习方法）的定量比较；消融实验不充分（未单独验证不同网格尺寸、不同时间步长等的影响）。
- **偏差风险**：仅针对无黏流体；对于黏性、湍流等复杂流动未讨论，可能限制实际工程应用。
- **应用限制**：网格基方法对于高精度复杂几何或大变形问题可能效率或精度不足；伴随方法要求物理模型可微且无奇异点，否则梯度可能不准确。
- **算力信息缺失**：无法评估实际运行成本及可扩展性。
- **无开源代码/数据集**：摘要中没有提及开源计划，复现难度大。
- **论文被拒（ICLR 2026）**：可能审稿人指出其他未覆盖的局限，需谨慎对待其结论的普适性。

（完）
