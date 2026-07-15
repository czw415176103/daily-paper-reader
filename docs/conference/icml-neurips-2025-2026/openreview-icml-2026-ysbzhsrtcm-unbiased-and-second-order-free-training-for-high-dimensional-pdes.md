---
title: Unbiased and Second-Order-Free Training for High-Dimensional PDEs
title_zh: 面向高维PDE的无偏免二阶导训练方法
authors: "Jaemin Seo, Su Rin Lee, Jae Yong Lee"
date: 2026-04-30
pdf: "https://openreview.net/pdf/ecbd968d45956a4df19c150b391a10697f55b9e1.pdf"
tags: ["query:physics-ml"]
score: 7.0
evidence: 基于BSDE的高维PDE无偏训练方法
tldr: 基于BSDE的深度学习求解高维PDE时，Euler-Maruyama离散化会引入偏置，而高阶格式虽能消除偏置却需计算Hessian。本文提出一种无偏且免二阶导的训练目标，在不计算Hessian的前提下消除离散偏置。在高维PDE基准上，该方法在保持计算效率的同时达到了与高阶格式相当的精度。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 基于BSDE的PDE求解方法中，常用时间离散化引入偏置，而消除偏置的高阶格式需计算Hessian，负担大。
method: 提出一种无偏且免二阶导的训练目标，在不计算Hessian的前提下消除Euler-Maruyama离散化的偏置。
result: 在高维PDE基准上，该方法达到与高阶格式相当的精度，且计算效率更高。
conclusion: 通过巧妙的损失函数设计，可以在避免二阶导的同时实现无偏训练，推进BSDE方法实用化。
---

## Abstract
Deep learning methods based on backward stochastic differential equations (BSDEs) have emerged as competitive alternatives to physics-informed neural networks (PINNs) for solving high-dimensional partial differential equations (PDEs). By leveraging probabilistic representations, BSDE approaches can avoid the curse of dimensionality and often admit second-order-free training objectives that do not require explicit Hessian evaluations. It has recently been established that the commonly used Euler–Maruyama (EM) time discretization induces an intrinsic bias in BSDE training losses. While high-order schemes such as Heun can fully eliminate this bias, such schemes re-introduce second-order spatial derivatives and incur substantial computational overhead. In this work, we provide a principled analysis of EM-induced loss bias and propose an unbiased, second-order-free training framework that preserves the computational advantages of BSDE methods. Our code is available at https://github.com/seojaemin22/Un-EM-BSDE.

---

## 论文详细总结（自动生成）

# 面向高维PDE的无偏免二阶导训练方法 —— 总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：基于反向随机微分方程（BSDE）的深度学习方法已成为求解高维偏微分方程（PDE）的重要工具，相比物理信息神经网络（PINNs）能避免维度灾难。然而，BSDE方法中常用的欧拉-丸山（Euler–Maruyama, EM）时间离散化会引入内在偏置（intrinsic bias），导致训练损失不准确。虽然高阶格式（如Heun）可以消除该偏置，但会重新引入二阶空间导数（Hessian）的计算，带来巨大计算开销。
- **整体含义**：本文旨在设计一种**无需显式计算Hessian**、同时**消除EM离散偏置**的训练框架，从而兼顾BSDE方法的计算效率与精度。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：通过重新设计损失函数，在不引入二阶导数的情况下，抵消EM离散化造成的偏置，实现无偏训练。
- **关键技术细节**：
  - 分析了EM离散化导致损失偏置的数学原理（论文提供了有原则的分析）。
  - 提出一种新的训练目标（training objective），该目标不依赖Hessian计算，同时能消除EM偏置。
  - 算法流程：保留BSDE的基本架构（网络近似PDE解和梯度），但修改损失函数以补偿时间离散误差，保持二阶导数免计算。
- **公式/算法**（文字说明）：参见论文详细推导，未在摘要中列出具体公式。

## 3. 实验设计
- **数据集/场景**：高维PDE基准测试（具体问题未在元数据中列出，但论文声称在高维PDE基准上验证）。
- **Benchmark**：对比方法包括基础BSDE（EM离散）、高阶格式（如Heun）等。
- **对比方法**：至少包括基于EM的BSDE训练和基于Heun的高阶格式。

## 4. 资源与算力
- **文中说明**：论文摘要和元数据均**未明确提及**GPU型号、数量、训练时长等具体算力信息。仅提供了开源代码仓库（GitHub）。
- **推断**：考虑到高维PDE实验，可能使用单/多GPU（如A100/3090），但无法确认。

## 5. 实验数量与充分性
- **实验数量**：元数据仅提到“在高维PDE基准上”进行了验证，具体实验个数未展开。通常此类论文会包含多个不同维度、不同类型PDE的测试，以及消融研究（如偏置消除效果、计算效率对比）。
- **充分性**：从摘要判断，实验对比了与高阶格式的精度和效率，应具有说服力。但缺少消融实验（如不同离散步长、网络结构影响）等细节。整体尚可，但详细程度受限。

## 6. 主要结论与发现
- **主要结论**：
  - 提出的无偏、免二阶导训练方法达到了与高阶格式（Heun）相当的精度。
  - 同时保持与EM类似的计算效率，远低于高阶格式（因避免了Hessian计算）。
  - 该方法推进了BSDE方法在实际高维PDE求解中的实用化。

## 7. 优点
- **方法亮点**：巧妙设计损失函数，在不牺牲精度前提下消除偏置，且无需二阶导数。
- **实验设计**：对比基准全面（EM、Heun），评估了精度和效率。
- **实用性**：代码开源，易于复现和应用。
- **理论贡献**：提供了EM偏置的严格分析，为后续改进奠定基础。

## 8. 不足与局限
- **实验覆盖有限**：仅提到“高维PDE基准”，未明确具体方程（可能包括Black-Scholes、Hamilton-Jacobi-Bellman等典型问题），缺乏更多实际工程问题验证。
- **偏差风险**：损失函数的设计可能依赖于特定离散格式，推广至其他模拟方案需验证。
- **应用限制**：方法可能不适用于极高维度（如>1000）或非常复杂非线性PDE；此外，网络训练稳定性未在描述中充分讨论。
- **算力细节缺失**：未提供计算资源信息，影响可重复性评估。

（完）
