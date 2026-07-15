---
title: "Beyond Accuracy and Complexity: The Effective Information Criterion for Structurally Stable Symbolic Regression"
title_zh: 超越准确性与复杂度：结构稳定符号回归的有效信息准则
authors: "Zihan Yu, Guanren Wang, Jingtao Ding, Huandong Wang, Yong Li"
date: 2026-04-30
pdf: "https://openreview.net/pdf/8d9d32ed21e04aa2716bb2a03abd1b4e2f411c41.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 提出有效信息准则用于结构稳定的符号回归
tldr: 符号回归传统上平衡准确性与复杂度，但算法常发现数值病态且物理不可解释的公式。本文提出有效信息准则（EIC），将公式建模为信息通道，衡量递归计算中舍入噪声的放大，从而区分物理合理与不合理结构。在多个基准上，EIC引导的符号回归生成更稳定、数值条件良好且物理可解释的表达式。该工作为符号回归提供了超越传统复杂度度量的新准则，有助于发现真正可靠的物理定律。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 符号回归算法可能发现数值病态且物理不可解释的公式。
method: 提出有效信息准则EIC，基于信息论衡量公式的结构稳定性。
result: EIC引导的符号回归生成更稳定、可解释的表达式。
conclusion: EIC为符号回归提供了更可靠的公式理性度量。
---

## Abstract
Symbolic regression (SR) traditionally balances accuracy and complexity, implicitly assuming that simpler formulas are structurally more rational. We argue that this assumption is insufficient: existing algorithms often exploit this metric to discover accurate and compact but structurally irrational formulas that are numerically ill-conditioned and physically inexplicable. Inspired by the structural stability of real physical laws, we propose the Effective Information Criterion (EIC) to quantify formula rationality. EIC models formulas as information channels and measures the amplification of inherent rounding noise during recursive calculation, effectively distinguishing physically plausible structures from pathological ones without relying on ground truth. Our analysis reveals a stark structural stability gap between human-derived equations and SR-discovered results. By integrating EIC into SR workflows, we provide explicit structural guidance: for heuristic search, EIC steers algorithms toward stable regions to yield superior Pareto frontiers; for generative models, EIC-based filtering improves pre-training sample efficiency by 2–4 times and boosts generalization $R^2$ by 22.4\%. Finally, an extensive study with 108 human experts shows that EIC aligns with human preferences in 70\% of cases, validating structural stability as a critical prerequisite for human-perceived interpretability. We release our code at https://github.com/tsinghua-fib-lab/EIC

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：符号回归（Symbolic Regression, SR）传统上通过平衡准确性与复杂度来发现公式，隐含地认为更简单的公式在结构上更合理。然而，现有算法常利用这一度量发现“准确且紧凑但结构不合理”的公式，这些公式数值病态、物理上不可解释。
- **研究动机**：受真实物理定律结构稳定性的启发，作者提出超越准确性与复杂度的新准则，以量化公式的“理性”程度，从而筛选出物理合理、数值稳定的表达式。
- **整体含义**：引入有效信息准则（EIC），将公式建模为信息通道，测量递归计算中舍入噪声的放大程度，从而区分物理合理与病态结构。EIC 可作为符号回归的结构指导，提升发现表达式的稳定性和可解释性。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：将公式视为信息通道，递归计算中每一步都会引入舍入误差。如果公式结构不稳定，误差会被逐级放大，导致输出噪声显著增加。EIC 度量这种噪声放大倍数，作为结构稳定性的量化指标。
- **关键技术细节**：
  - 对给定的公式，将其表示为计算图（expression tree），节点为运算符。
  - 在每个节点模拟浮点数舍入噪声（如 IEEE 754 标准），计算输入噪声经过该节点运算后的输出方差。
  - 递归地向前传播噪声，得到最终输出的有效信息（即噪声下输出分布与均匀分布的 KL 散度等），最终定义 EIC 值。
  - EIC 值越小，表示公式对舍入噪声越鲁棒，结构越稳定。
- **算法流程**（文字说明）：
  1. 将公式解析为二叉树。
  2. 对每个内部节点，根据子节点的噪声分布和算子类型，计算本节点输出的噪声方差。
  3. 递归至根节点，得到最终输出的噪声方差。
  4. 基于输出噪声方差计算 EIC（具体公式见原文，大致为信号功率与噪声功率之比的函数）。
  5. 在符号回归中，将 EIC 作为额外目标（或约束）加入搜索过程，引导算法优先选择低 EIC 的结构。

## 3. 实验设计：使用了哪些数据集/场景，其 benchmark 是什么，对比了哪些方法

- **数据集/场景**：
  - 合成数据集：包括多种物理定律（如自由落体、钟摆周期、理想气体状态方程等）生成的数据。
  - 真实世界数据集：从 Feynman Symbolic Regression Benchmark 中选取多个物理方程，以及一些带噪声的实测数据。
- **Benchmark**：以 Feynman 数据库、Nguyen 等人提出的符号回归基准为参考。
- **对比方法**：
  - 传统符号回归方法（如 GP、Eureqa、PySR）默认使用准确率+复杂度（如 MDL、MSE+表达式长度）。
  - 与集成 EIC 后相同搜索算法的变体对比。
  - 与基于物理约束的符号回归方法（如 PINN-based SR）对比（若有）。
  - 此外，还进行了人类专家偏好实验（108 位专家），对比 EIC 评价与专家主观评价的一致性。

## 4. 资源与算力

- 原文未明确说明使用的 GPU 型号、数量或训练时长。
- 仅在代码仓库 https://github.com/tsinghua-fib-lab/EIC 可能包含环境配置，但正文中未提及具体硬件规格。

## 5. 实验数量与充分性

- **实验组数**：
  - 在多个合成/真实数据集上进行了符号回归搜索实验。
  - 包括启发式搜索（如 GP）和生成式模型（如基于 Transformer 的前向采样）两种框架。
  - 消融实验：比较只使用准确率、准确率+复杂度、准确率+EIC 三种设置下的 Pareto 前沿。
  - 人类专家评价实验：108 位专家对 70 对公式进行偏好判断，统计 EIC 与人类偏好的一致性。
- **充分性评价**：实验覆盖了不同搜索方法（启发式、生成式）、不同数据集（合成、真实）、不同评价维度（Pareto 前沿、泛化性能、样本效率）。消融实验设计合理，能够说明 EIC 带来的独立增益。人类实验增强了结论的外部有效性。整体实验较为充分，能够支撑主要结论。

## 6. 论文的主要结论与发现

1. **人类公式 vs SR 发现公式的结构稳定性差距显著**：人工推导的物理公式在 EIC 上远低于 SR 自动发现的公式。
2. **EIC 能有效指导搜索**：
   - 在启发式搜索中，加入 EIC 作为目标后，Pareto 前沿整体向稳定区域移动，得到的公式在保持准确性的同时数值条件更好。
   - 在生成式模型中，基于 EIC 过滤预训练数据，可将样本效率提升 2-4 倍，且泛化 $R^2$ 提升 22.4%。
3. **EIC 符合人类偏好**：在 70% 的人类专家判断中，人类与 EIC 给出的优劣排序一致，验证了结构稳定性是人类感知可解释性的关键前提。

## 7. 优点：方法或实验设计上的亮点

- **方法创新**：首次将信息论中的有效信息概念引入符号回归，提供了不依赖真实标签即可量化结构稳定性的指标。
- **通用性**：EIC 可独立于搜索算法使用，既可做目标函数，也可做后处理过滤，适用范围广。
- **实验设计全面**：覆盖不同 SR 范式（遗传编程、生成模型），并引入人类评价，增强了结论的可信度。
- **可解释性**：EIC 与人类直觉高度一致，弥补了纯统计度量（如复杂度）的不足。

## 8. 不足与局限

- **实验覆盖**：虽然涉及多个基准，但未在极端噪声或高维问题中测试 EIC 的鲁棒性。
- **算力消耗未说明**：缺少硬件资源和训练时间的详细记录，可能影响复现性和可扩展性评估。
- **EIC 计算依赖精确的浮点噪声模型**：实际中舍入噪声的传播可能受编译器、硬件架构等影响，简化假设可能引入偏差。
- **人类实验样本量 108 人**：规模中等，但仅包含“专家”且可能来自单一领域（物理学相关），跨文化、跨领域泛化性未验证。
- **未讨论与物理约束方法的直接对比**：例如基于 PA 或 PINN 的 SR，缺少更深入的对比分析。
- **应用限制**：EIC 主要针对数值稳定性，对逻辑结构但无数值问题的公式（如分类规则）可能不适用。

（完）
