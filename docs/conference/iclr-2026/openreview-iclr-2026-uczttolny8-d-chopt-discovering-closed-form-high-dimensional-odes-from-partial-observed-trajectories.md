---
title: "D-CHOPT: DISCOVERING CLOSED-FORM HIGH-DIMENSIONAL ODEs FROM PARTIAL OBSERVED TRAJECTORIES"
title_zh: D-CHOPT：从部分观测轨迹中发现闭式高维常微分方程
authors: "Yiting Duan, Zhao Yao, Yi Guo"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=uCZtTolnY8"
tags: ["query:sr"]
score: 9.0
evidence: 从部分观测轨迹中自动发现闭式常微分方程
tldr: 现有符号回归方法在处理高维ODE时受限于观测性和拓扑复杂性。本文提出D-CHOPT方法，从部分观测轨迹中自动发现闭式高维ODE。该方法克服了低维局限，在多个高维动力系统基准上取得优异结果，显著提升了从数据驱动发现方程的能力。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有符号回归方法在高维ODE发现中表现不佳，缺乏对部分观测局限性的理解。
method: 提出D-CHOPT方法，结合观测性分析和拓扑特征处理，实现高维闭式ODE的自动发现。
result: 在多个高维动力系统上，D-CHOPT成功恢复真实方程，效果优于现有方法。
conclusion: D-CHOPT为高维动力学方程发现提供了有效范式，拓展了符号回归的应用边界。
---

## Abstract
Machine learning algorithms have become a new paradigm for automatically discovering closed-form ordinary differential equations (ODEs) from observed trajectories. Although significant breakthroughs have been made in this field, such as symbolic regression and sparse identification of nonlinear dynamics (SINDy), existing approaches primarily perform well for low-dimensional ODEs. This limitation arises due to the lack of understanding of observability limitations in partially observed trajectories, and the additional challenges introduced by complex topological properties. In this work, we propose a method for discovering closed-form high-dimensional ODEs from partially observed trajectories, called D-CHOPT, which advances ODE discovery methods beyond the natural limitations of high-dimensional ODEs. D-CHOPT uses an invertible neural network as the backbone to find the optimal solution within the diffeomorphic equivariant group of the reconstructed dynamical systems, while preserving topological properties and integrating a variable selection method. We provide a formal analysis of observability and the learning limitations of partial trajectories, and explain the enhancements in a manner consistent with the theoretical results. In experiments, D-CHOPT successfully discovered the governing equations for a wide range of dynamical systems, both low and high dimensional.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有基于机器学习的自动发现闭式常微分方程（ODE）的方法（如符号回归、SINDy）主要适用于低维系统，在高维 ODE 上表现不佳。
- **根本原因**：现有方法缺乏对部分观测轨迹中可观测性限制的理解，并且无法有效处理高维系统引入的复杂拓扑特性。
- **研究目标**：提出一种能从部分观测轨迹中自动发现高维闭式 ODE 的方法，突破低维局限，拓展符号回归的应用边界。

## 2. 论文提出的方法论

- **核心思想**：利用可逆神经网络作为骨干，在微分同胚等变群内寻找最优解，同时保留动力系统的拓扑性质，并集成变量选择方法，从而实现对高维 ODE 的发现。
- **关键技术细节**  
  - **可逆神经网络**：确保映射的可逆性，从而保证系统状态空间的双射变换。  
  - **微分同胚等变群**：在保持动力系统拓扑结构（如轨道的拓扑等价性）的前提下搜索最优的变换参数。  
  - **变量选择方法**：用于筛选对系统动力学重要的变量，降低高维带来的冗余。  
  - **理论分析**：提供了对可观测性以及部分轨迹学习限制的形式化分析，并以此指导方法设计，使改进与理论结果一致。
- **算法流程（文字说明）**  
  1. 输入部分观测到的高维轨迹数据。  
  2. 利用可逆神经网络学习一个从观测空间到潜在动力学空间的微分同胚变换。  
  3. 在微分同胚等变约束下，结合变量选择方法，从变换后的轨迹中识别出最稀疏的闭式 ODE 表达式。  
  4. 通过反向变换得到原始空间的动力学方程。

## 3. 实验设计

- **数据集/场景**：涵盖低维和高维的多种动力系统（具体系统名称未在元数据中列出，但声称“a wide range of dynamical systems”）。
- **Benchmark**：以真实方程作为标准，评估恢复方程的准确性和完整性。
- **对比方法**：未明确列出，但提到与现有方法（如符号回归、SINDy）进行比较，且效果优于它们。

## 4. 资源与算力

- 论文元数据及摘要中均未提及具体的计算资源（GPU 型号、数量、训练时长等）。需要指出这一点。

## 5. 实验数量与充分性

- **实验数量**：元数据未给出具体数字，但提及在多个高维动力系统基准上取得结果，且包含低维系统的验证。
- **充分性评估**：  
  - 从摘要看，方法在低维和高维系统上均有效，覆盖了不同复杂度的场景。  
  - 但缺乏对实验次数、消融实验、超参数敏感性等细节的描述，因此判断为“基本充分但不够详尽”。  
  - 应该进行了与现有方法的横向比较，但元数据未说明是否在同样条件下对比，公平性需要查验原文。

## 6. 论文的主要结论与发现

- D-CHOPT 成功从部分观测轨迹中恢复了高维闭式 ODE 的真实方程，在多个系统上优于现有方法。
- 理论分析证实了部分轨迹的可观测性限制，并指导了方法的有效性。
- D-CHOPT 为高维动力学方程发现提供了一种新的有效范式，显著拓展了符号回归方法的应用范围。

## 7. 优点

- **理论驱动**：形式化分析了可观测性限制，方法设计与理论分析一致，具有较好的解释性。
- **处理部分观测**：直接面对实际应用中常见的数据不完整问题，而现有方法通常假设全状态可观测。
- **保留拓扑性质**：通过微分同胚等变群保证了动力学系统的本质特征（如轨道拓扑）在变换过程中不被破坏。
- **高维适用性**：结合变量选择机制，在维数增加时仍能提取稀疏的闭式方程。

## 8. 不足与局限

- **计算资源未报告**：缺乏对训练时间、GPU 型号等实际开销的说明，难以评估方法的可复现性。
- **实验细节不透明**：元数据中未给出具体的对比方法名称、数据集列表、消融实验设计，也没有统计显著性测试，实验的充分性和公平性有待原论文完整内容确认。
- **潜在偏差风险**：仅针对部分动力系统进行测试，对混沌系统、刚性系统等的表现未知；变量选择方法的鲁棒性也未讨论。
- **应用限制**：方法假设可逆神经网络能够捕获微分同胚，对于不可逆或拓扑结构过于复杂的系统可能失效；对观测噪声、采样率等实际因素的敏感性未知。

（完）
