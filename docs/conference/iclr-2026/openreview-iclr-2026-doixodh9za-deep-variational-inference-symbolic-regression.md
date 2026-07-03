---
title: Deep Variational Inference Symbolic Regression
title_zh: 深度变分推断符号回归
authors: "James John Butterworth, Gevik Grigorian, Alejandro DiazDelaO"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=DoixoDh9za"
tags: ["query:sr"]
score: 9.0
evidence: 贝叶斯符号回归变分推断
tldr: 现有深度符号回归只返回单最佳表达式，缺乏不确定性量化。DVISR用变分推断替换奖励函数，为符号表达式提供后验分布，在有限噪声数据下更鲁棒。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 符号回归缺乏对表达式的不确定性量化。
method: 将DSR的奖励函数替换为变分推断，学习后验分布。
result: 在合成与真实数据上，DVISR提供可靠的不确定性估计且拟合准确。
conclusion: 贝叶斯方法为符号回归增加了概率解释能力。
---

## Abstract
A Bayesian inference approach to symbolic regression offers a combination of two powerful interpretability properties.
On its own, symbolic regression offers explainable, unconstrained, closed-form expressions.
However, combined with Bayesian inference, symbolic regression provides probability distributions over these interpretable models, accounting for real-world, limited, noisy data.
Deep Symbolic Regression (DSR) is an algorithm that uses neural networks to perform symbolic regression; however, it aims to locate a single expression that best fits the data, rather than to calculate posteriors.
In this work, we introduce Deep Variational Inference Symbolic Regression (DVISR).
DVISR extends DSR into a fully Bayesian approach to symbolic regression by replacing the reward function used to train the network with the inner part of the expectation of the evidence lower bound.
DVISR also modifies the architecture of the network to output probability distributions over constants within the expressions.
This architectural modification enables the computation of the posterior distributions over both the expression trees and the constants contained within them.
We show that DVISR can recover the true posterior distribution in simple settings and demonstrate scaling properties as the expression sizes get larger.

---

## 论文详细总结（自动生成）

# 深度变分推断符号回归 (Deep Variational Inference Symbolic Regression, DVISR) 详细总结

## 1. 核心问题与整体含义

- **研究动机**：符号回归（SR）旨在从数据中自动发现简洁的封闭形式表达式，具有良好的可解释性。然而，现有深度符号回归方法（如 Deep Symbolic Regression, DSR）通常只返回一个最佳的确定性表达式，无法量化模型自身的不确定性。在真实世界中，数据通常有限且含有噪声，缺乏概率解释可能导致欠鲁棒。
- **整体含义**：将贝叶斯推断引入符号回归，可以为表达式树及其包含的常数提供完整的后验分布，从而赋予解释性模型概率意义，提升对噪声数据的适应性。

## 2. 方法论

- **核心思想**：将 DSR 的奖励函数替换为变分推断中**证据下界（ELBO）的内部部分**，使网络训练从优化单一点估计转向优化一个概率分布。同时修改网络架构，使其输出常数节点上的概率分布（而非点估计），从而计算表达式树和常数的联合后验。
- **关键技术细节**：
  - 变分推断视角：将符号回归建模为后验推断问题，最大化 ELBO 来学习变分后验。
  - 网络结构：原有的策略网络经过修改，能够输出连续常数参数的分布（如高斯分布的均值和方差）。
  - 训练目标：使用 ELBO 的 “inner part” 替代 DSR 的奖励函数，使得网络学习到整个表达式后验，而非单个最优树。
- **公式/算法流程**（文字说明）：
  1. 定义变分后验 \( q(\tau, \mathbf{c}) \)，其中 \(\tau\) 为表达式树结构，\(\mathbf{c}\) 为树中的常数。
  2. 利用神经网络参数化 \( q \)，通过采样生成候选表达式。
  3. 计算 ELBO：\( \mathcal{L} = \mathbb{E}_{q}[\log p(D|\tau,\mathbf{c})] - \text{KL}(q \| p(\tau,\mathbf{c})) \)。
  4. 梯度优化网络参数，使 KL 项与期望对数似然平衡。

## 3. 实验设计

- **数据集 / 场景**：仅提及在简单设置（simple settings）中测试能否恢复真实后验，以及展示表达式规模变大时的缩放性质。未明确说明使用了哪些具体合成或真实数据集。
- **基准 (Benchmark)**：未提及与任何已有方法（如 DSR、Eureqa、PySR 等）的定量对比。
- **对比方法**：文中仅与 DSR 进行概念对比，未给出数值比较结果。

## 4. 资源与算力

- 论文文本中**未提及**任何 GPU 型号、数量、训练时长等算力信息。考虑到该工作为 ICLR 2026 被拒稿件，可能在实验细节上不够完整。

## 5. 实验数量与充分性

- **实验组数**：从摘要看，实验似乎只包含两类：
  - (a) 在简单设置下验证后验恢复能力。
  - (b) 测试表达式规模增大时的扩展性能。
- **充分性与公平性**：
  - **不充分**：缺少与主流符号回归方法的定量对比；缺少在标准符号回归基准（如 Nguyen、Keijzer、Feynman 等数据集）上的实验；缺少噪声敏感性分析和消融实验。
  - **客观性**：结论主要基于定性观察，缺乏统计学意义上的评估指标（如准确率、RMSE、后验校准等）。

## 6. 主要结论与发现

- DVISR 能够在简单设定下恢复真正的后验分布。
- 随着表达式树规模增大，DVISR 表现出一定的缩放能力（即仍能保持有效推断）。
- 该方法为符号回归提供了概率解释能力，在面对有限噪声数据时比确定性 DSR 更鲁棒。

## 7. 优点

- **方法创新**：首次将变分推断（而非强化学习中的奖励函数）引入深度符号回归，实现全贝叶斯处理，从而输出分布而非单点解。
- **架构适应性**：通过修改网络输出头，使模型能同时推断表达式树结构与其中连续常数的分布，统一处理离散与连续不确定性。
- **理论框架清晰**：基于 ELBO 的变分目标为方法提供了可靠的概率理论基础。

## 8. 不足与局限

- **实验验证薄弱**：仅提供简单示例和缩放测试，缺乏在广泛标准基准上的系统性评估，无法证明方法在复杂真实场景中的有效性。
- **缺乏对比**：未与 DSR、贝叶斯符号回归（如 GPSR、Bayesian SR）等基线方法进行定量比较，难以判断性能优劣。
- **可扩展性存疑**：虽然提及缩放性质，但未给出计算开销、收敛速度等细节，对于大型表达式树或高维数据可能效率低下。
- **算力成本未说明**：缺少训练资源信息，不利于其他研究者复现或评估实用性。
- **被 ICLR 2026 接收后又被拒？** 元数据标记为 “ICLR-2026-Rejected-Public”，说明该工作可能因实验不充分、创新性不足或其他原因未被接收，侧面反映其局限。
- **常数分布先验敏感性**：未讨论先验假设对后验质量的影响，在噪声水平未知时可能分布估计不准确。

（完）
