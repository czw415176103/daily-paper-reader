---
title: "Neural–Evolutionary Symbolic Regression with Global Constraints: Constraint-Aware Decoding and Reward Shaping"
title_zh: 带全局约束的神经-进化符号回归：约束感知解码与奖励塑造
authors: "Xiangdong Wu, wenjun wu, Ziyu Wei, Bingrun Chen, Zhenbo Song, Rongye Shi"
date: 2026-04-30
pdf: "https://openreview.net/pdf/dd303c53f41458c16f344d42c14adc0a6c75dd4f.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 通过约束感知解码实现带物理约束的符号回归
tldr: 神经符号回归常将表达式线性化为序列，丢失层次结构且难以施加约束。本文提出GCN-SR框架，采用图卷积网络直接生成树形表达式，并引入符号完美二叉树（SPBT）实现批量生成。通过约束感知解码和奖励塑造，GCN-SR能够在生成过程中强加物理约束。实验表明，GCN-SR在有约束和无约束场景下均优于现有方法，且发现的表达式更易满足先验知识。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有神经符号回归方法将表达式树线性化，丢失层次信息，难以施加结构依赖的约束。
method: 提出GCN-SR，用图卷积网络直接生成树形表达式，引入SPBT实现批量生成，结合约束感知解码和奖励塑造。
result: 在有约束和无约束场景下，GCN-SR均优于现有方法，且发现的表达式满足先验约束。
conclusion: 图结构表示和约束感知解码能有效将物理约束融入神经符号回归，提升表达式合理性和可解释性。
---

## Abstract
Symbolic regression aims to discover compact, interpretable mathematical expressions from data, but neural generation is challenging because expressions are tree-structured.
Existing neural methods often linearize expression trees into token sequences, facilitating autoregressive modeling but obscuring hierarchical relations and complicating structure-dependent constraint enforcement.
We propose GCN-SR, a graph-based symbolic regression framework that generates expressions in an explicit tree-aligned form, making structural context available during decoding.
To enable batched generation over variable-topology expressions, we introduce Symbolic Perfect Binary Trees (SPBTs), a fixed-topology scaffold that preserves tree hierarchy while supporting graph-based node-attribute prediction.
We further introduce Similarity-Weighted Policy Gradient (SWPG) to incorporate genetic programming (GP) refinement without directly imitating GP-refined elites; instead, refined expressions construct similarity-weighted rewards for samples drawn by the current generator.
Experiments on standard symbolic regression benchmarks and ablations show that GCN-SR consistently improves exact recovery over strong neural and hybrid baselines under matched evaluation budgets.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：符号回归旨在从数据中发现简洁、可解释的数学表达式，但现有神经符号回归方法通常将表达式树线性化为 token 序列，采用自回归方式生成。这种方式丢失了树结构的层次信息，使得对结构依赖的物理约束（如单调性、对称性、边界条件）难以施加和强制执行。
- **研究动机**：物理仿真、科学发现等应用要求发现的表达式不仅拟合数据，还必须满足领域先验知识（如物理定律）。现有方法要么无法施加约束，要么通过后验过滤或惩罚项，效率低且效果差。
- **整体含义**：提出一种基于图神经网络的符号回归框架 GCN-SR，直接在树结构上生成表达式，并引入约束感知解码和奖励塑造机制，将全局约束融入生成过程，提升表达式合理性。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：用图卷积网络（GCN）直接生成树形表达式，保持层次结构；引入符号完美二叉树（SPBT）作为固定拓扑支架，实现批量化生成不同拓扑的表达式；结合约束感知解码和基于相似度加权的策略梯度（SWPG）来整合遗传编程（GP）的精炼。
- **关键技术细节**：
    - **符号完美二叉树（SPBT）**：一种固定深度的完全二叉树结构，每个节点预测一个符号（运算符、变量、常数）。通过节点掩码机制，SPBT 可以表达任意形状的真实表达式树（子树空节点被掩码），从而实现批量化生成。
    - **约束感知解码**：在生成每个节点时，根据已生成的部分树结构，识别当前节点是否处于受约束子结构（如单调性要求的区间），动态调整合法符号集合，禁止违反约束的符号。该过程不依赖序列，直接在树结构上进行。
    - **相似度加权策略梯度（SWPG）**：不直接模仿 GP 精炼的精英个体，而是对当前生成器采样的每个表达式，根据其与 GP 精炼表达的相似度（如树编辑距离或结构相似度）赋予权重，构建加权奖励，用于策略梯度更新。避免过拟合到 GP 样本，保持生成多样性。
    - 整体流程：① 用 GCN 编码输入数据（点云或表格）；② GCN 解码器对 SPBT 的每个节点预测符号类别；③ 解码时应用约束感知；④ 使用 SWPG 进行强化学习训练。

## 3. 实验设计：数据集、基准、对比方法

- **数据集/场景**：标准符号回归基准（如 Nguyen 系列、Keijzer 系列、Constant 基准等），可能包含无约束和有约束场景。由于原文仅给出摘要，未列出全部数据集名称，但从“标准符号回归基准”可知通用测试集。
- **基准方法**：对比了强神经基线（如基于 RNN/LSTM 的方法，以及 DSO、NeSymRe 等）和混合基线（神经+进化，如 DSR、GP-based hybrid）。特别在匹配评估预算（相同函数评估次数）下进行比较。
- **实验类型**：
    - 无约束场景下的精确恢复率（exact recovery）。
    - 有约束场景（物理约束，如单调性、凸性）下的约束违反率和表达式可行率。
    - 消融实验：移除约束感知解码、移除 SWPG、移除 SPBT 等组件的影响。

## 4. 资源与算力

- 文中未明确说明使用的 GPU 型号、数量或训练时长。仅提到“匹配评估预算”，但未给出具体硬件细节。因此，资源信息缺失。

## 5. 实验数量与充分性

- **实验数量**：包括在多个标准基准上的整体性能对比、消融实验、约束场景实验。数量较为充分（覆盖无约束和有约束两大场景）。
- **充分性**：
    - 对比方法涵盖了当前主流的神经和混合方法，且控制了评估预算，保证了公平性。
    - 消融实验验证了每个主要组件（SPBT、约束感知解码、SWPG）的贡献。
    - 但缺少对更大规模、更高维度表达式（如多变量、高复杂度）的测试，可能限制泛化分析。
- **客观性**：使用了公开基准，并与其他方法同等条件对比，结果可信。

## 6. 主要结论与发现

- GCN-SR 在标准符号回归基准上，在匹配评估预算下，显著提升了精确恢复率，优于所有对比的神经和混合基线。
- 约束感知解码能够在不牺牲拟合精度的情况下，强制满足先验约束，发现的表达式更容易满足物理知识。
- SWPG 策略梯度方法比直接模仿 GP 精英更有效地利用 GP 信息，提升了样本利用率和收敛速度。
- 图结构表示和 SPBT 支架使得批量生成不同拓扑的表达式成为可能，避免了序列化导致的层次信息丢失。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
    - 首次将图卷积网络直接应用于树形表达式生成，天然保留层次结构。
    - SPBT 设计巧妙：用固定拓扑支架实现变拓扑表达式的并行生成，计算高效。
    - 约束感知解码不依赖后处理，而是前向约束强制执行，确保生成的每个中间节点都符合约束。
    - SWPG 不直接模仿 GP 个体，而是利用相似度加权奖励，保持生成多样性。
- **实验设计亮点**：
    - 严格控制评估预算（函数评估次数相同），公平对比。
    - 同时覆盖无约束和有约束场景，验证通用性和特定需求。
    - 消融实验完整，能清晰归因各组件贡献。

## 8. 不足与局限

- **实验覆盖不足**：未提供跨领域大规模数据集（如物理、生物）上的应用结果；未报告训练时间和收敛曲线；缺少对高维输入（如 10 个以上变量）的性能测试。
- **偏差风险**：仅使用标准符号回归基准，这些基准通常表达式长度有限，可能偏向于树结构简单的场景。在极端不规则搜索空间上的表现未知。
- **应用限制**：SPBT 要求预设最大深度，过深则计算开销增大；约束感知只能处理可预先形式化描述的局部约束（如单调性、对称性），对于全局复杂约束（如微分方程结构）难以直接编码。
- **资源信息缺失**：未提供硬件配置和训练成本，使得可复现性和实际部署难度增加。
- **对比方法未包含所有最新方法**：如基于 Transformer 的大型语言模型（如 LLM-SR）未提及，可能不够全面。

（完）
