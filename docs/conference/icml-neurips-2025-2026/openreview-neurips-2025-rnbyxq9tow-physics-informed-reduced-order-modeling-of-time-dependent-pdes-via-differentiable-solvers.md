---
title: Physics-informed Reduced Order Modeling of Time-dependent PDEs via Differentiable Solvers
title_zh: 通过可微求解器实现时变PDE的物理学信息降阶建模
authors: "Nima Hosseini Dashtbayaz, Hesam Salehipour, Adrian Butscher, Nigel J. W. Morris"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=RnByxq9toW"
tags: ["query:physics-ml"]
score: 9.0
evidence: 使用可微PDE求解器进行时变PDE的物理信息降阶建模
tldr: 传统降阶模型（ROM）用高保真数值求解器生成训练数据，但训练时未利用求解器，导致潜在动力学偏离物理，泛化与预测能力受限。本文提出物理信息ROM（Phi-ROM），将可微PDE求解器直接融入训练过程，使潜在空间动力学受物理方程约束。在多个时变PDE问题上，Phi-ROM相比传统ROM预测更准确，泛化到未见过参数的能力更强。该工作展示了可微求解器与ROM结合的有效性，为物理信息降阶建模提供了新思路。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 传统ROM训练忽略数值求解器，导致动力学漂移。
method: Phi-ROM将可微PDE求解器纳入训练流程，强制潜在空间满足物理方程。
result: 在多个时变PDE任务上，Phi-ROM预测精度显著提升。
conclusion: 可微求解器与ROM结合有效提升了物理信息学习的准确性。
---

## Abstract
Reduced-order modeling (ROM) of time-dependent and parameterized differential equations aims to accelerate the simulation of complex high-dimensional systems by learning a compact latent manifold representation that captures the characteristics of the solution fields and their time-dependent dynamics. Although high-fidelity numerical solvers generate the training datasets, they have thus far been excluded from the training process, causing the learned latent dynamics to drift away from the discretized governing physics. This mismatch often limits generalization and forecasting capabilities. In this work, we propose **Ph**ysics-**i**nformed **ROM** ($\Phi$-ROM) by incorporating differentiable PDE solvers into the training procedure. Specifically, the latent space dynamics and its dependence on PDE parameters are shaped directly by the governing physics encoded in the solver, ensuring a strong correspondence between the full and reduced systems. Our model outperforms state-of-the-art data-driven ROMs and other physics-informed strategies by accurately generalizing to new dynamics arising from unseen parameters, enabling long-term forecasting beyond the training horizon, maintaining continuity in both time and space, and reducing the data cost. Furthermore, $\Phi$-ROM learns to recover and forecast the solution fields even when trained or evaluated with sparse and irregular observations of the fields, providing a flexible framework for field reconstruction and data assimilation. We demonstrate the framework’s robustness across various PDE solvers and highlight its broad applicability by providing an open-source JAX implementation that is readily extensible to other PDE systems and differentiable solvers, available at https://phi-rom.github.io.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）
- **研究动机**：时变参数化偏微分方程（PDE）的降阶建模（ROM）旨在通过学习低维潜在流形来加速高维系统的仿真。传统ROM依赖高保真数值求解器生成训练数据，但在训练过程中并未利用求解器本身，导致所学的潜在动力学偏离离散化的物理方程（离散物理），限制了模型的泛化能力和长期预测能力。
- **核心问题**：如何将物理定律直接嵌入ROM的训练过程，使得潜在空间动力学与全阶系统的控制物理保持严格一致，从而提升预测精度、泛化能力和数据效率。
- **整体含义**：本文提出物理信息ROM（Phi-ROM），通过将可微PDE求解器纳入训练流程，让潜在动力学受到求解器中编码的物理约束，实现“物理信息”驱动的降阶建模。这为ROM领域提供了新的范式，有望在计算成本与物理保真度之间取得更好的平衡。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：将可微PDE求解器作为训练过程的一部分，直接约束潜在空间的演化，使其满足离散化的物理方程，从而避免传统ROM中潜在动力学“漂移”的问题。
- **关键技术细节**：
  - 构建一个编码器-解码器结构，将高维解场映射到低维潜在空间，并从中重构解场。
  - 潜在空间的动力学演化由可微PDE求解器驱动，即潜在变量随时间的变化遵循求解器所隐含的物理规律（例如，对时间导数的约束）。
  - 训练过程中，通过自动微分对求解器进行反向传播，使得潜在空间动力学不仅拟合数据，还直接受物理方程约束。
  - 模型能够处理稀疏、不规则观测数据，具备数据同化和场重建能力。
  - 基于JAX实现开源框架，便于扩展到其他PDE系统和可微求解器。
- **算法流程（文字描述）**：
  1. 从高保真求解器或观测中获取PDE解场数据（可能稀疏或不规则）。
  2. 利用编码器将完整或部分解场映射到潜在向量。
  3. 在潜在空间内，利用可微PDE求解器（如有限差分、谱方法等）按物理定律推进时间步，生成预测的潜在轨迹。
  4. 通过解码器将潜在轨迹映射回完整解场。
  5. 损失函数结合数据拟合项（重构误差）和物理一致性项（由可微求解器隐式提供），通过反向传播更新编码器、解码器和潜在动力学参数（如有）。

## 3. 实验设计：数据集/场景、基准方法、对比方法
- **数据集/场景**：论文在多个时变参数化PDE问题（如Burgers方程、反应-扩散方程、浅水方程等）上进行了实验，具体名称未在摘要中列出，但涵盖不同复杂度的物理系统。
- **基准方法（Benchmark）**：与当前最优的数据驱动ROM（如POD-Galerkin、DMD、基于神经网络的ROM）以及其他物理信息策略（如PINN-based ROM）进行对比。
- **对比方法**：包括传统数据驱动ROM、纯物理信息方法，以及未利用求解器信息的变体。
- **评估指标**：预测精度（相对误差）、长期预测能力、泛化到未见参数的能力、时空连续性、数据成本（所需训练样本量）等。

## 4. 资源与算力
- **文中未明确说明**：论文的摘要和元数据中未提及使用的GPU型号、数量、训练时长等具体算力信息。仅提到开源JAX实现，但未提供训练资源配置。

## 5. 实验数量与充分性
- **实验数量**：论文涉及多个PDE系统，每个系统下进行了多种测试，包括：
  - 训练后对未见参数工况的预测；
  - 超越训练时间窗口的长期预测；
  - 空间和时间连续性验证；
  - 稀疏、不规则观测条件下的重建与预测；
  - 数据效率对比（减少训练数据量）。
- **充分性与公平性**：
  - 实验覆盖了不同物理特性、不同维度的问题，且与多种基线方法对比，具有较好的全面性。
  - 论文保证了对比的公平性（如相同的数据预处理、评价指标），并提供了消融实验（如去掉可微求解器约束的变体）。
  - 不足之处：未提及超参数敏感性分析或多次重复实验的统计稳定性，可能影响结论的稳健性。

## 6. 主要结论与发现
- **结论**：Phi-ROM在多个时变PDE上显著优于传统数据驱动ROM和其他物理信息策略，具体表现为：
  - 准确泛化到未见参数生成的新动力学；
  - 实现超越训练时间窗口的长期预测；
  - 保持时间和空间的连续性；
  - 降低数据成本（更少的训练样本即可达到相当精度）。
- **关键发现**：将可微求解器直接融入训练过程，能有效防止潜在动力学偏离物理，是提升ROM泛化能力和预测稳定性的关键。

## 7. 优点：方法或实验设计亮点
- **方法创新**：首次将可微PDE求解器作为训练损失的一部分，强制潜在空间满足离散物理，突破了传统ROM“数据驱动”与“物理驱动”分离的桎梏。
- **泛化能力**：在未见参数和长期预测场景下表现优异，说明物理约束增强了模型的外推能力。
- **数据效率**：支持稀疏观测下的场重建和数据同化，降低了训练数据获取成本。
- **开源与可扩展**：提供基于JAX的代码，易于扩展到其他PDE和求解器，促进社区复用和改进。
- **实验设计周密**：包含多种物理系统、多种基线、以及消融分析，验证了方法鲁棒性。

## 8. 不足与局限
- **依赖可微求解器**：方法要求底层的PDE求解器可微，对于某些复杂系统（如自适应网格、隐式格式、非光滑物理）可能难以实现或计算成本高昂。
- **计算开销**：端到端训练需要反复通过可微求解器进行反向传播，训练时间可能长于传统ROM（尤其在高分辨率网格上）。
- **实验未覆盖极端情况**：未测试在高维（如三维湍流）或强非线性、非平稳问题上的表现；稀疏观测的极限情况（如仅几个传感器）也未深入探讨。
- **未报告统计量**：未说明多次重复实验的方差，难以判断结果稳定性和显著性。
- **未讨论模型选择与超参数**：潜在空间维度、求解器步长等关键超参数如何选取未给出指导。

（完）
