---
title: Recursive Structure Discovery as an Inductive Bias for Symbolic Regression
title_zh: 递归结构发现作为符号回归的归纳偏置
authors: "Wassim Tenachi, Rodrigo Ibata, Alejandro M. Illescas Giménez, Yashar Hezaveh, Laurence Perreault-Levasseur, Pierre-Luc Bacon"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=0YVqQY1Udx"
tags: ["query:sr"]
score: 9.0
evidence: 使用递归结构发现的符号回归方法
tldr: 符号回归搜索空间巨大，但许多科学目标具有简单结构（如可加性）。本文提出递归结构发现步骤，通过神经网络导数检测可分离性，将函数分解为层次化子函数作为结构先验。该方法可附加到任何符号回归后端，显著降低搜索复杂度并提高可解释性。实验表明其有效性强。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 符号回归搜索空间巨大，而许多科学目标具有简单可分离结构未被利用。
method: 用二阶更新的小神经网络估计导数，递归检测可分离性并分解函数为子函数。
result: 该方法降低搜索复杂度，提高可解释性，并可附加到任何符号回归后端。
conclusion: 递归结构发现是一种有效且通用的符号回归归纳偏置。
---

## Abstract
Symbolic regression (SR) can recover analytic laws from data, but its search space is enormous. Many scientific targets are structurally simple, for example additively or multiplicatively separable, yet most SR pipelines do not exploit this. We introduce a recursive structure discovery step that tests for separability using accurate derivatives from a small neural model trained with second-order updates. The method decomposes $y=f(\mathbf{x})$ into a hierarchy of simpler subfunctions, which we feed to SR as a structure prior. This plug-in reduces search complexity, improves interpretability, and can attach to any SR backend; here we pair it with a deep RL generator. This substantially reduces search complexity, improves interpretability, and remains robust to noise, maintaining reliable separability detection under challenging conditions. On SRBench (Feynman, 120 equations), the structure-aware pipeline achieves state-of-the-art exact recovery, outperforming separability-only, pure RL, and prior hybrid baselines.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：符号回归（Symbolic Regression, SR）旨在从数据中自动恢复解析定律，但其搜索空间极为庞大。许多科学目标函数具有天然的简单结构，例如可加性（additive）或可乘性（multiplicative）分离，然而现有大多数符号回归方法并未利用这一先验知识。
- **研究动机**：通过利用函数结构的可分离性作为归纳偏置（inductive bias），可以大大缩小搜索空间，提升恢复效率和可解释性。
- **整体含义**：提出一种递归结构发现（Recursive Structure Discovery）步骤，作为插件附加到任何符号回归后端，从而降低搜索复杂度、提高恢复精度，并在标准基准上达到最先进性能。

## 2. 方法论：核心思想、关键技术细节、算法流程
- **核心思想**：将函数 \( y = f(\mathbf{x}) \) 递归地分解为更简单的子函数层次结构。分解的依据是通过对函数可分离性的检测（例如当偏导数或高阶导数满足一定条件时，函数可拆分为独立子函数之和或积）。
- **关键技术细节**：
  - 使用一个**小规模神经网络**，并通过**二阶更新**（second-order updates）训练，以精确估计函数的导数。
  - 基于导数估计，递归检测函数是否可分离（如加性可分离：\( f(x_1, x_2) = g(x_1) + h(x_2) \)）。
  - 一旦检测到可分离性，将原函数分解为多个低维子函数，然后对每个子函数递归应用该过程。
  - 最终得到一颗层次化的分解树，表示函数的**结构先验**。
- **算法流程**（文字描述）：
  1. 使用二阶优化的小型神经网络拟合给定数据点，得到可微的代理模型。
  2. 计算代理模型的一阶/二阶偏导数，利用可分离性判据（如偏导数的条件独立性）进行测试。
  3. 若可分离，将输入变量分组，构建子函数并递归回到步骤1。
  4. 若不可分离，则将当前函数作为不可约单元交给符号回归后端。
  5. 将分解后的结构先验提供给任一符号回归后端（本文与深度强化学习生成器配对），以引导搜索过程。
- **插件性质**：该方法可附加到任何符号回归后端，不依赖特定底层算法。

## 3. 实验设计
- **数据集/场景**：
  - 主要基准：**SRBench**（Feynman 方程集合），包含 120 个经典物理方程（Feynman 数据集）。
  - 实验场景包括干净数据和含噪声数据，以测试鲁棒性。
- **基准方法**：
  - 仅使用可分离性（ablation）的基线。
  - 纯深度强化学习（Pure RL）符号回归基线。
  - 此前最佳的混合方法（Prior hybrid baselines）。
- **对比方式**：精确恢复率（exact recovery）作为主要指标，即成功恢复出与真实解析表达式完全一致的概率。

## 4. 资源与算力
- **文中未明确说明**所使用的具体 GPU 型号、数量、训练时长等算力细节。
- 仅提及使用“二阶更新训练小神经网络”，暗示可能需要额外计算资源，但未量化。
- 点评：缺乏算力报告，难以评估方法的计算开销和可复现性。

## 5. 实验数量与充分性
- **实验数量**：主要实验在 120 个 Feynman 方程上进行，涵盖多种方程类型。
- **充分性评估**：
  - 优势：使用了广泛认可的 SRBench 基准，覆盖数量较大（120个方程），并对比了多个基线，结果具有说服力。
  - 不足：未提供消融实验（如不同神经网络大小、不同后端的影响）、未说明在非 Feynman 数据集上的表现、未详细展示噪声鲁棒性的定量结果。整体实验数量尚可，但充分性有待加强（如缺少跨领域泛化测试）。

## 6. 主要结论与发现
- 提出的**结构感知符号回归管道**（Structure-aware SR pipeline）在 SRBench 上达到了 **state-of-the-art 的精确恢复率**，显著优于仅可分离性基线、纯 RL 基线以及先前混合基线。
- 该方法**显著降低了搜索复杂度**，提高了解释性，并且对噪声具有鲁棒性，能够在挑战性条件下依然可靠地检测可分离性。
- 证明了递归结构发现是一种有效的通用归纳偏置，可自然地集成到多种符号回归框架中。

## 7. 优点
- **插件式设计**：可无缝附加到任意符号回归后端，具有良好的通用性和易用性。
- **利用导数信息**：通过二阶优化训练神经网络获得精确导数，实现了对可分离性的鲁棒自动检测，无需人为预设结构。
- **提高可解释性**：分解后的函数层次结构天然可解释，与科学发现流程一致。
- **搜索复杂度降低**：将原始难题逐步分解为低维子问题，大幅缩小搜索空间。
- **噪声鲁棒性**：在含噪数据上依然保持可靠分离性检测，适用实际场景。

## 8. 不足与局限
- **实验覆盖有限**：仅使用了 Feynman 方程集，缺乏对更多领域（如生物学、经济学）或更复杂结构（如非可分离型）的验证。
- **算力和时间成本未报告**：无法评估方法在实际部署中的计算代价，尤其二阶更新小神经网络的训练成本可能较高。
- **依赖可分离性先验**：对于完全不可分离的函数，该方法的收益有限，甚至可能增加计算开销；未讨论对不可分离函数的处理效果。
- **消融实验不完整**：未深入分析神经网络大小、二阶更新参数、分解阈值等超参数的影响，也未提供不同后端（如遗传编程）的对比结果。
- **应用限制**：方法假设函数可层次化分解为低维子函数，对于高维且耦合紧密的问题可能效果不佳。

（完）
