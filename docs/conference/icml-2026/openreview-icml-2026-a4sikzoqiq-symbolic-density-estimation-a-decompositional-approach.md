---
title: "Symbolic Density Estimation: A Decompositional Approach"
title_zh: 符号密度估计：一种分解方法
authors: "Angelo Rajendram, Xieting Chu, Max Fieg, Aishik Ghosh, Vijay Ganesh"
date: 2026-01-21
pdf: "https://openreview.net/pdf/ae0761184cd12b307ed347b9b9d5edcdeee28bbb.pdf"
tags: ["query:sr"]
score: 8.0
evidence: 符号回归应用于密度估计
tldr: 符号回归在标准回归中表现优异，但在密度估计任务上尚未探索。本文提出AI-Kolmogorov框架，通过聚类/图模型分解、非参数密度估计、支持估计和符号回归四阶段流程，实现对密度函数的符号化表示。在合成混合模型和高能物理等应用中验证了有效性，展示了符号密度估计的潜力。
source: ICML-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 符号回归尚未被有效应用于密度估计任务。
method: 提出AI-Kolmogorov框架，包含问题分解、非参数估计、支持估计和符号回归四阶段。
result: 在合成分布和高能物理数据上成功生成符号密度模型。
conclusion: 扩展了符号回归的应用范围至密度估计。
---

## Abstract
We introduce AI-Kolmogorov, a novel framework for Symbolic Density Estimation (SymDE). Symbolic regression (SR) has been effectively used to produce interpretable models in standard regression settings but its applicability to density estimation tasks has largely been unexplored. To address the SymDE task we introduce a multi-stage pipeline: (i) problem decomposition through clustering and/or probabilistic graphical model structure learning; (ii) nonparametric density estimation; (iii) support estimation; and finally (iv) SR on the density estimate. We demonstrate the efficacy of AI-Kolmogorov on synthetic mixture models, multivariate normal distributions, and three exotic distributions, two of which are motivated by applications in high-energy physics. We show that AI-Kolmogorov can discover underlying distributions or otherwise provide valuable insight into the mathematical expressions describing them.

---

## 论文详细总结（自动生成）

# 论文详细总结：符号密度估计：一种分解方法

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：符号回归（Symbolic Regression, SR）在标准回归任务中已成功生成可解释模型，但其在密度估计任务中的应用尚未被探索。密度估计是统计和机器学习中的重要问题，传统方法多为黑箱模型（如神经网络、核密度估计），缺乏可解释性。
- **核心问题**：如何将符号回归扩展到密度估计领域，生成可解释的符号化密度函数。
- **整体含义**：提出一种名为 **AI-Kolmogorov** 的框架，首次系统性地将符号回归应用于密度估计，并验证其在合成分布和高能物理应用中的有效性，有望为可解释密度建模提供新途径。

## 2. 论文提出的方法论
- **核心思想**：通过多阶段流水线实现符号密度估计（SymDE），将复杂密度估计问题分解为多个子任务，逐步得到符号化表达式。
- **关键技术细节（四阶段流程）**：
  1. **问题分解**：使用聚类（clustering）和/或概率图模型结构学习（probabilistic graphical model structure learning）对原始数据进行分解，将联合分布拆解为多个子分量或条件分布。
  2. **非参数密度估计**：对每个子分量或子结构，采用非参数方法（如核密度估计）得到初步密度估计。
  3. **支持估计（Support Estimation）**：估计密度的支撑集（即定义域），用于后续符号回归的边界约束。
  4. **符号回归**：在密度估计上执行符号回归，搜索最能拟合非参数估计值的符号表达式，最终输出数学公式形式的密度函数。
- **公式或算法流程**：文中未给出具体公式，但流程可概括为：数据 → 分解 → 非参数密度 → 支持估计 → SR → 符号密度函数。

## 3. 实验设计
- **使用的数据集/场景**：
  - 合成混合模型（Synthetic mixture models）
  - 多元正态分布（Multivariate normal distributions）
  - 三种“异类分布”（exotic distributions），其中两种源于高能物理（high-energy physics）应用场景。
- **基准（Benchmark）**：未明确提及对比的基线方法（如传统核密度估计、高斯混合模型等），重点在于展示AI-Kolmogorov能发现底层分布或提供数学洞察。
- **对比方法**：未给出与其他符号回归方法或密度估计方法的直接对比。实验主要是定性展示和证明可行性。

## 4. 资源与算力
- **文中未明确说明**：论文未提及使用的GPU型号、数量、训练时长等算力信息。可能是由于该框架主要依赖符号回归库（如PySR、Eureqa等）以及非参数估计，对算力需求相对较低，但具体细节缺失。

## 5. 实验数量与充分性
- **实验数量**：从摘要和正文描述看，至少包括合成混合模型、多元正态、三种异类分布共约6个实验场景，但未列出详细的消融实验、超参数敏感性分析或不同分解策略对比。
- **充分性与公平性**：
  - **充分性不足**：缺乏与现有密度估计方法（如GMM、核密度、归一化流）的定量对比（如对数似然、KL散度等指标）。
  - **客观性**：实验更多是概念验证（proof-of-concept），展示了符号表达式恢复能力，但未在标准密度估计基准上评估。
  - **偏见风险**：仅选择两种高能物理应用，可能偏向于特定领域，通用性未充分验证。

## 6. 论文的主要结论与发现
- AI-Kolmogorov框架能成功生成符号密度模型，在合成分布和高能物理数据上恢复或发现解释性数学表达式。
- 符号回归可扩展至密度估计任务，为可解释密度建模提供了新的可能性。
- 框架的分解步骤（聚类/图模型）对复杂多模态分布有效，能够简化后续符号回归的难度。

## 7. 优点
- **方法新颖**：首次系统地将符号回归引入密度估计，填补了该交叉领域的空白。
- **可解释性强**：输出为显式数学公式，有利于科学发现和领域知识理解。
- **模块化设计**：四阶段流水线灵活，可替换各模块（如不同聚类算法、SR工具）。
- **实际应用驱动**：高能物理示例展示了在科学数据中的潜在价值。

## 8. 不足与局限
- **实验覆盖不足**：仅测试少量合成和特定领域数据，未在真实通用密度估计基准（如UCI、图像密度）上验证。
- **缺乏定量对比**：未与传统/现代密度估计方法进行性能比较（如似然值、计算效率）。
- **未讨论可扩展性**：未分析高维数据下的性能（如维度灾难对非参数估计和SR的影响）。
- **缺少消融研究**：分解策略、支持估计、SR超参数的影响未单独评估。
- **算力信息缺失**：无法评估方法的成本与可重现性。
- **论文被拒稿**：可能评审认为贡献或验证不够充分，但具体原因未在此处说明。

（完）
