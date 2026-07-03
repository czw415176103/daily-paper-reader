---
title: Mining Valuable Sub-Expressions for Symbolic Regression
title_zh: 挖掘有价值的子表达式用于符号回归
authors: "Yibo Zhao, Qiang Lu, Baimou Li, Jake Luo"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=HArdYL2hez"
tags: ["query:sr"]
score: 9.0
evidence: 符号回归方法
tldr: 符号回归旨在从数据中发现数学表达式，但传统方法因搜索空间巨大而效率低下。本文提出MSSR框架，通过协作多智能体强化学习和遗传编程，从动态库中智能采样子表达式并组合，显著提升了搜索效率和准确性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 传统符号回归方法逐个构建表达式，忽略了子表达式的重用价值。
method: 采用协作多智能体强化学习结合遗传编程，动态挖掘和重用子表达式。
result: 在多个基准数据集上，MSSR比现有方法更快找到正确表达式。
conclusion: 子表达式重用能有效提升符号回归的搜索效率。
---

## Abstract
Symbolic Regression (SR) aims to discover mathematical expressions from data, but classical methods are hampered by an immense search space. This inefficiency stems from their tendency to construct expressions atom-by-atom using basic operators and variables, overlooking the power of reusing meaningful sub-expressions. To address this challenge, we introduce Mining Sub-Expression Symbolic Regression (MSSR), a novel framework that discovers and leverages valuable sub-expressions to efficiently search for the correct symbolic form. MSSR employs a cooperative multi-agent reinforcement learning framework, augmented with genetic programming, to intelligently sample sub-expressions from a dynamically evolving library, combining them into a mathematical expression. A pruning mechanism based on the coefficient of variation is utilized to remove redundant terms, promoting the discovery of the parsimonious expression. We conduct extensive experiments on the SRBench and fluid dynamics benchmarks. The results demonstrate that, compared to 24 baseline methods, MSSR recovers more ground-truth expressions and achieves a superior balance between predictive accuracy and model simplicity.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究问题**：符号回归（Symbolic Regression, SR）旨在从观测数据中自动发现数学表达式，但传统方法因搜索空间巨大而效率低下。其根本原因在于传统方法采用“原子式”构建方式，即每次从基本运算符和变量中逐个选择操作数来组合表达式，忽视了子表达式的复用价值。
- **研究动机**：如果能挖掘并重用有意义的子表达式，则可以显著缩小搜索空间，提高发现正确表达式的效率。
- **整体含义**：论文提出一种名为 MSSR（Mining Sub-Expression Symbolic Regression）的新框架，通过协作多智能体强化学习与遗传编程相结合，动态地从不断演化的库中智能采样子表达式，并将它们组合成数学表达式，从而提升符号回归的搜索效率和准确性。

## 2. 论文提出的方法论

- **核心思想**：将子表达式视为可复用的“构建块”，通过强化学习智能体动态选择和组合这些子表达式，同时利用遗传编程（GP）的变异与交叉操作维持库的多样性，并采用剪枝机制控制表达式复杂度。
- **关键技术细节**：
  - **协作多智能体强化学习**：多个智能体协同工作，每个智能体负责从候选子表达式库中采样，并组合形成完整表达式。
  - **动态演化的子表达式库**：库中的子表达式并非固定，而是在搜索过程中不断更新（通过GP操作或发现的有价值片段）。
  - **基于变异系数的剪枝机制**：使用变异系数（Coefficient of Variation）作为冗余度衡量标准，移除贡献较小的项，促进简洁表达式（Parsimonious Expression）的生成。
- **算法流程（文字说明）**：
  1. 初始化子表达式库（包含基本运算符和变量）。
  2. 多智能体通过强化学习策略从库中采样子表达式，组合成候选表达式。
  3. 评估候选表达式在数据上的拟合性能（如均方误差）。
  4. 使用遗传编程（交叉、变异）对库进行更新，引入新的子表达式或淘汰低效子表达式。
  5. 基于变异系数对表达式进行剪枝，移除冗余项。
  6. 迭代直至收敛或达到最大代数。

## 3. 实验设计

- **数据集 / 场景**：
  - SRBench（符号回归基准测试）
  - 流体动力学基准（Fluid Dynamics benchmarks）
- **Benchmark**：以恢复真实表达式（ground-truth expression）的准确率、预测精度与模型简洁性的平衡为主要评估指标。
- **对比方法**：与 **24 种基线方法** 进行比较。具体方法列表未在摘要中给出，但涵盖经典的遗传编程、基于强化学习的符号回归方法等。

## 4. 资源与算力

- 论文摘要和元数据中 **未明确说明** 所使用的 GPU 型号、数量、训练时长等算力资源。仅在实验部分提及进行了“大量实验”，但无具体资源细节。因此无法总结具体的算力消耗。如需获得更详细算力信息，需查阅完整论文正文。

## 5. 实验数量与充分性

- **实验数量**：在 SRBench（通常包含多个合成与真实数据集）和流体动力学基准上进行了评估，并与 24 种基线方法对比，覆盖了多种场景。
- **充分性与公平性**：
  - **充分性**：使用了两个代表性基准（SRBench 是符号回归领域常用标准平台），并对比了大量基线方法（24种），实验维度较为丰富。
  - **客观性**：结果报告 MSSR 恢复更多真实表达式，且达到预测精度与简洁性的更优平衡。但未提及是否进行了统计显著性检验或多次重复实验的误差分析。
  - **局限性**：仅给出总体性能提升，未展示消融实验（如无剪枝、无RL单独GP等）来验证各组件的贡献。根据元数据，该论文被 ICLR 2026 拒稿，可能实验设计存在不足。

## 6. 论文的主要结论与发现

- MSSR 相比 24 种基线方法能够在更多基准上恢复真实的数学表达式。
- MSSR 在预测精度与模型简洁性之间达到了更优的平衡（即不仅准确，而且表达式更紧凑）。
- 动态挖掘和重用子表达式可以有效提升符号回归的搜索效率。

## 7. 优点

- **方法亮点**：将多智能体强化学习与遗传编程有机结合，解决了传统 SR 中搜索空间爆炸的问题。
- **子表达式复用思想**：新颖且直观，符合人类发现公式时“举一反三”的认知规律。
- **剪枝机制**：基于变异系数，避免过拟合，促进简洁表达式生成。
- **实验对比充分**：与 24 种基线方法对比，覆盖主流方法，有利于体现性能优势。

## 8. 不足与局限

- **实验报告不完整**：未提供具体数据集名称、训练/测试划分、重复次数、标准差等细节，难以衡量结果的可靠性。
- **缺失消融实验**：没有分解验证多智能体协作、动态库更新、剪枝机制各自的贡献。
- **算力信息缺失**：无法判断方法的实际计算开销是否可接受。
- **应用限制**：仅在两个基准上评估，未涉及高维或噪声较强的真实世界场景，泛化性有待验证。
- **被拒稿暗示**：该论文被 ICLR 2026 拒稿，可能存在的审稿人意见包括贡献不够显著、实验设计不严谨或方法创新性不足。
- **复现难度**：正文细节缺失（如强化学习的具体算法、遗传编程参数设置等），导致他人难以复现。

（完）
