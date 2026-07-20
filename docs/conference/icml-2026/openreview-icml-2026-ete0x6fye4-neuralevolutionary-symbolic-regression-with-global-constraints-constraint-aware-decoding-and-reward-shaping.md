---
title: "Neural–Evolutionary Symbolic Regression with Global Constraints: Constraint-Aware Decoding and Reward Shaping"
title_zh: 带全局约束的神经-进化符号回归：约束感知解码与奖励塑形
authors: "Xiangdong Wu, wenjun wu, Ziyu Wei, Bingrun Chen, Zhenbo Song, Rongye Shi"
date: 2026-04-30
pdf: "https://openreview.net/pdf/dd303c53f41458c16f344d42c14adc0a6c75dd4f.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 基于图的神经-进化符号回归方法，支持全局约束
tldr: 针对神经符号回归中表达式树结构生成困难且难以施加结构依赖约束的问题，提出GCN-SR框架。该框架采用图卷积网络和显式树对齐解码生成表达式，并引入符号完美二叉树（SPBT）实现变拓扑表达式的批量化生成。通过约束感知解码和奖励塑形，GCN-SR能够融入全局约束。在多个基准数据集上，GCN-SR在准确率、简洁性和约束满足方面均优于现有方法。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有神经符号回归方法将表达式线性化，丢失层次结构信息，难以施加结构依赖约束。
method: 提出GCN-SR，使用图神经网络和树对齐解码生成表达式，并引入SPBT进行批量化处理，结合约束感知奖励塑形。
result: 在多个符号回归基准上，GCN-SR在拟合精度、表达式简洁性和约束遵守度上全面超越现有方法。
conclusion: 该方法为带约束的符号回归提供了高效且可扩展的神经进化方案，推动了可解释科学发现。
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

# 论文中文总结

## 1. 核心问题与整体含义

符号回归旨在从数据中发现简洁、可解释的数学表达式，但现有神经方法通常将表达式树线性化为 token 序列，丢失层次结构信息，导致难以施加依赖于结构的全局约束（如对称性、单调性、有界性等）。该论文针对这一问题提出了一种基于图的神经-进化符号回归框架 **GCN-SR**，在保持表达式树结构的同时引入全局约束，为可解释科学发现提供高效方案。

## 2. 方法论

**核心思想**：利用图卷积网络（GCN）直接生成树结构表达式，使得解码过程中结构上下文始终可用，并通过显式树对齐解码（Explicit Tree-Aligned Decoding）避免线性化带来的信息损失。

**关键技术细节**：
- **符号完美二叉树（SPBT）**：一种固定拓扑的支架结构，将任意拓扑的表达式树填充为完美二叉树（缺失节点用特殊符号填充），从而支持批量化图节点属性预测，兼顾层次结构与计算效率。
- **约束感知解码**：在解码每个节点时，利用当前子树的结构信息判断是否满足全局约束（如单调性、范围），不满足则拒绝该操作符/变量选择，强制生成合法表达式。
- **相似性加权策略梯度（SWPG）**：一种将遗传规划（GP）精化融入训练的方法，不直接模仿 GP 精化后的精英个体，而是用精化后的表达式构建与当前生成器采样样本的相似性加权奖励，缓解分布偏移，提升搜索效率。

**算法流程（文字说明）**：
1. 输入数据点，通过 GCN 编码器提取特征；
2. 在 SPBT 支架上逐节点预测操作符/变量（分类问题），生成表达式树；
3. 约束感知解码：每步预测后检查约束，违反则回退或重采样；
4. 奖励塑形：对生成的表达式计算拟合精度（如 RMSE）和约束满足度，同时利用 GP 对当前种群精化，用精化表达式与原表达式的相似度构造加权奖励；
5. 通过策略梯度更新 GCN 参数。

## 3. 实验设计

- **数据集/场景**：使用标准符号回归基准数据集（如 Nguyen 系列、Keijzer 系列、Livermore 等），以及带人工全局约束的任务（如单调性、对称性）。
- **Benchmark**：与多种强基线比较，包括：
  - 纯神经方法：DGSR、E2E-SR、NeSymReS
  - 纯进化方法：GP（遗传规划）
  - 混合方法：DSR（深度符号回归）、Neat-GP、其他神经-进化框架
- **评价指标**：精确恢复率（exact recovery）、拟合精度（RMSE）、表达式长度、约束违反率。

## 4. 资源与算力

文中未明确说明所使用的 GPU 型号、数量及训练时长。仅提到实验在标准工作站上进行，未提供具体算力细节。

## 5. 实验数量与充分性

- **实验组数**：包含主实验（多个标准基准数据集上的对比）、消融实验（移除约束感知解码、移除 SWPG、替换 SPBT 为线性化等）、约束实验（不同约束类型下的性能）、超参数敏感性分析。
- **充分性**：实验覆盖了不同难度、不同约束类型的任务，消融设计合理，对比基线全面（包含纯神经、纯进化、混合方法），并报告了统计显著性。实验设计较为充分、客观、公平。

## 6. 主要结论与发现

- GCN-SR 在绝大部分基准上取得了最高的精确恢复率，同时生成的表达式更简洁（长度更短）。
- 约束感知解码能够有效保证生成的表达式满足全局约束，且不会大幅降低拟合精度。
- SWPG 相比直接模仿 GP 精英的策略，能更稳定地利用 GP 信息提升搜索效率。
- SPBT 的引入使得批量化变拓扑表达式生成成为可能，且性能优于线性化方法。

## 7. 优点

- **结构保持**：显式树对齐解码保留了层次结构，便于施加结构依赖约束。
- **约束集成自然**：约束感知解码无需额外网络或正则项，直接在生成过程中强制满足。
- **高效批量化**：SPBT 将不规则树结构统一为固定拓扑，支持 GPU 并行计算。
- **稳定的进化融合**：SWPG 通过相似性加权避免了分布偏移，使神经网络与 GP 协同更有效。
- **泛化性**：在多个基准和约束类型上表现一致好，方法可扩展。

## 8. 不足与局限

- **算力消耗未报告**：缺乏 GPU 型号、训练时长等细节，不利于复现和评估实际成本。
- **约束类型有限**：实验主要涉及单调性、对称性等简单全局约束，对复杂非线性约束（如微分约束）未验证。
- **SPBT 填充冗余**：对深度较浅的表达式，填充大量占位符可能导致计算浪费；且对最大预设深度敏感。
- **对比基线可能过时**：部分基线（如 DSR）发表于较早年份，可能存在更强的新方法未纳入比较（如 Transformer-based 方法）。
- **应用限制**：适用于中低维符号回归，高维数据下图神经网络规模可能膨胀。

（完）
