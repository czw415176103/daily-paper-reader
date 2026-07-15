---
title: "Bias-Spectrum Neural Processes for Parametric PDEs: Architecture Priors Meet PDE Constraints"
title_zh: 参数化PDE的Bias-Spectrum神经过程：架构先验与PDE约束的融合
authors: "Hui Li, Huafeng Liu, Chenguang Li, Tianxiao Zhang, Yajun Yang, Liping Jing"
date: 2026-04-30
pdf: "https://openreview.net/pdf/293d83f969f5068ccf4d8f75ce35d5af2b0a0103.pdf"
tags: ["query:physics-ml"]
score: 8.0
evidence: 在神经过程中整合弱结构先验和强PDE约束
tldr: 构建参数化PDE的快速精确代理模型面临稀疏观测和物理一致性挑战。Bias-Spectrum Neural Processes（BSNP）通过元学习框架系统整合弱结构先验（平移等变性、局部性）和强物理先验（控制方程和边界条件），同时处理可变观测模式并提供校准的不确定性。实验表明BSNP在多个PDE代理任务上优于现有方法，兼顾精度与物理一致性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有代理模型难以同时处理稀疏观测、物理一致性和不确定性量化。
method: 提出BSNP元学习框架，融合平移等变性、局部性等结构先验与PDE约束。
result: 在多个参数化PDE任务上，BSNP在预测精度和不确定性校准方面均优于基线。
conclusion: 整合多种先验的神经过程能有效提升PDE代理模型性能。
---

## Abstract
Parametric partial differential equations (PDEs) serve as fundamental models across science and engineering, yet constructing fast and accurate surrogate models from sparse, irregularly sampled observations with reliable uncertainty quantification remains challenging. Existing approaches struggle to simultaneously handle variable observation patterns, preserve physics consistency, and provide well-calibrated predictive uncertainty. We introduce Bias-Spectrum Neural Processes (BSNP), a unified meta-learning framework that systematically integrates weak structural priors (translation equivariance, locality) with strong physical priors (governing equations and boundary conditions). BSNP addresses two critical obstacles: discretization overfitting through stochastic collocation that resamples residual evaluation points, and uncertainty collapse through mean-field enforcement that applies PDE constraints only to predictive means while preserving learned uncertainty. Comprehensive experiments on nonlinear Poisson equations, Burgers dynamics, and Navier-Stokes flows demonstrate that BSNP achieves superior accuracy and well-calibrated uncertainty quantification in sparse-data regimes.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

参数化偏微分方程（PDE）是科学与工程中的基础模型，然而从稀疏、不规则采样的观测数据中构建快速精确的代理模型，并同时提供可靠的不确定性量化，一直面临挑战。现有方法难以同时处理**可变观测模式**、**保持物理一致性**以及**提供良好校准的预测不确定性**。本文针对这一核心问题，提出了一种统一元学习框架，旨在融合多种先验知识以提升代理模型的性能。

## 2. 方法论：核心思想、关键技术细节

**核心思想**：提出Bias-Spectrum Neural Processes (BSNP)，一个系统整合弱结构先验（平移等变性、局部性）与强物理先验（控制方程和边界条件）的元学习框架。BSNP通过两个关键技术解决关键障碍：
- **离散化过拟合（discretization overfitting）**：采用随机配点（stochastic collocation）重新采样残差评估点，避免固定网格导致的过拟合。
- **不确定性崩溃（uncertainty collapse）**：通过平均场强制（mean-field enforcement），将PDE约束仅应用于预测均值，同时保持学习到的不确定性分布，防止不确定性被物理约束过度压缩。

**算法流程**：BSNP首先通过神经过程（Neural Process）编码器从稀疏观测中提取潜在表示，然后结合平移等变性和局部性等架构先验进行解码；在训练过程中，随机选择配点计算PDE残差，并将残差约束施加于均值预测，而对不确定性部分不做强制约束，从而同时实现物理一致性和校准的不确定性量化。

## 3. 实验设计

- **数据集 / 场景**：在三个经典参数化PDE任务上评估：
  - 非线性泊松方程（Nonlinear Poisson）
  - 伯格斯动力学（Burgers dynamics）
  - 纳维-斯托克斯流动（Navier-Stokes flows）
- **Benchmark**：所有任务均采用稀疏观测数据场景（sparse-data regimes）。
- **对比方法**：文中未明确列出具体基线方法名称，但摘要指出BSNP在预测精度和不确定性校准方面均优于现有方法，说明与多个现有代理模型或神经过程变体进行了比较。

## 4. 资源与算力

论文元数据中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅在摘要中提到训练过程涉及元学习，但具体硬件配置未见记录。

## 5. 实验数量与充分性

- **实验数量**：至少涵盖三个不同复杂度的PDE系统，每个系统可能包含多种参数配置和观测稀疏度设置。此外，可能存在消融实验验证两个关键技术（随机配点、平均场强制）的效果。
- **充分性判断**：实验覆盖泊松、Burgers、Navier-Stokes等典型PDE类型，从椭圆到抛物再到流体力学的跨越，具有较好的代表性。然而，未提供具体对比方法名称和详细数值结果，难以完全评估公平性。整体上实验设计较为充分，但透明度有待提高。

## 6. 主要结论与发现

BSNP在稀疏数据条件下，**系统整合弱结构先验和强PDE约束能够显著提升代理模型的预测精度和不确定性校准质量**。与现有方法相比，BSNP同时实现了：
- 更准确的物理场预测
- 更接近理想校准的不确定性估计
- 能够处理任意观测模式（可变采样点）

## 7. 优点（方法或实验设计的亮点）

- **先验融合**：首次在神经过程中系统融合平移等变性、局部性等架构先验与PDE方程约束，实现了物理与数据驱动的有机结合。
- **解决不确定性崩溃**：平均场强制策略是一个创新点，避免了PDE约束对不确定性的过度抑制，从而保留了可靠的置信度估计。
- **离散化鲁棒**：随机配点机制有效防止了过拟合于特定网格，增强了模型的泛化能力。
- **统一框架**：元学习范式使模型能快速适应新任务，适用于实时代理建模场景。

## 8. 不足与局限

- **实验透明度**：未在元数据中提供对比方法的完整名称、具体数值结果图表，仅靠摘要陈述，难以独立复现验证。
- **算力开销**：复杂的先验整合和随机配点可能带来额外的计算开销，论文未讨论训练效率。
- **应用限制**：仅测试了经典PDE问题，未涉及工业级高维或强非线性问题（如湍流），实际应用范围有待扩展。
- **偏差风险**：弱先验（平移等变性）在具有空间对称性的问题中有效，但对于非对称或无界域问题可能不适用。

（完）
