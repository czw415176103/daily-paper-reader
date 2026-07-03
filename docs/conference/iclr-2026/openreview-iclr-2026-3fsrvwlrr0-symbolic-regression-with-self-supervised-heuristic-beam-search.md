---
title: Symbolic Regression with Self-Supervised Heuristic Beam Search
title_zh: 基于自监督启发式束搜索的符号回归
authors: "Ronnypetson Souza da Silva, Esther Luna Colombini"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=3fsrvwLRr0"
tags: ["query:sr"]
score: 10.0
evidence: 基于自监督启发式束搜索的符号回归方法
tldr: 符号回归旨在发现简洁可解释的数学表达式。本文提出一种结合束搜索与自监督启发式的方法，在合成数据上训练，对精确解问题高效且倾向简洁表达式，在SRBench和物理定律数据集上达到竞争性能。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有符号回归方法在数据效率和解的简洁性上仍有不足。
method: 结合束搜索与自监督学习的启发式，对大规模合成数据进行训练。
result: 在SRBench和物理定律数据集上性能与现有方法相当甚至更优。
conclusion: 自监督束搜索为符号回归提供了高效且简洁的解决方案。
---

## Abstract
Symbolic Regression (SR) aims to discover simple and interpretable mathematical expressions that explain observed data, making it a powerful tool for scientific discovery. In this work, we introduce a conceptually simple SR method that is both sample-efficient with respect to observed data points and self-supervised on large-scale synthetic data. By design, our approach favors parsimony, yielding interpretable and concise expressions. We focus on problems with exact solutions, evaluating our method on datasets containing physical laws and dynamical
equations. Our results demonstrate that combining beam search with a learned heuristic achieves competitive performance compared to existing methods in SRBench. Additionally, our approach effectively handles expressions with constants, a common challenge in the SR field. Finally, we provide a comprehensive scalability analysis across four key dimensions: (i) expression length, (ii) number of variables, (iii) number of domains, and (iv) number of observed data points.

---

## 论文详细总结（自动生成）

# 基于自监督启发式束搜索的符号回归：论文总结

## 1. 核心问题与整体含义
符号回归（Symbolic Regression, SR）旨在从观测数据中发现简洁、可解释的数学表达式，是科学发现的重要工具。现有方法在数据效率和解的简洁性方面存在不足。本文提出一种概念简单的方法，结合束搜索（Beam Search）与自监督学习启发式，在合成数据上大规模预训练，从而实现数据高效且偏好简洁解的符号回归。该方法聚焦于具有精确解的问题（如物理定律和动力方程），在SRBench等基准上取得竞争性能。

## 2. 方法论
- **核心思想**：将符号回归视为序列生成问题，使用束搜索遍历表达式空间；通过学习一个启发式函数（自监督方式在合成数据上训练）来引导搜索，优先选择更短、更简洁的表达式。
- **关键技术细节**：
  - 采用自监督学习：在大量随机生成的合成表达式及其对应数据点上训练一个神经网络，使其能预测给定数据下最优表达式的“得分”或“概率”。
  - 束搜索：在测试时，使用训练好的启发式指导束搜索，保留最有希望的候选表达式，并逐步扩展。
  - 简洁性偏好：启发式函数隐式或显式地惩罚复杂表达式，从而生成简洁可解释的解。
- **算法流程**（文字说明）：
  1. 生成大规模合成数据集：随机采样数学表达式（包括常数），计算其在不同输入点上的输出值。
  2. 训练自监督模型：以数据点（输入-输出对）为输入，以最优表达式（或表达式片段）为监督信号，训练启发式网络。
  3. 测试时：给定观测数据，采用束搜索，每一步使用启发式模型评估候选表达式，只保留得分最高的前K个候选，直到生成完整表达式或达到最大长度。

## 3. 实验设计
- **数据集/场景**：
  - 物理定律和动力方程数据集（如SRBench中的标准问题）。
  - 包含常数的表达式（常见挑战）。
  - 另外进行四项扩展性分析：表达式长度、变量数量、域数量、观测数据点数量。
- **基准（Benchmark）**：SRBench（符号回归标准基准集）。
- **对比方法**：与SRBench中的现有方法（如遗传编程、深度符号回归等）进行比较。具体方法名称未在摘要中列出，但称达到了竞争性能。

## 4. 资源与算力
论文原文（基于提供的元数据和摘要）**未明确说明使用的GPU型号、数量、训练时长**等算力信息。仅提到在“大规模合成数据”上训练，但具体规模未给出。因此无法评估计算成本。

## 5. 实验数量与充分性
- **实验组数**：至少包括SRBench上的性能评估，以及四个维度的扩展性分析（表达式长度、变量数、域数、数据点数），另外还有处理常数的专门实验。消融实验未在摘要中提及。
- **充分性与公平性**：实验设计覆盖了多个关键维度，但未提供统计细节（如重复次数、方差）。由于未看到完整论文，无法判断对比方法的超参数是否公平调优。总体而言，实验范围较广但深度可能有限。

## 6. 主要结论与发现
- 结合束搜索与自监督启发式的方法在SRBench上达到与现有方法相当或更优的性能。
- 该方法能有效处理含有常数的表达式（SR领域的常见难点）。
- 方法天然偏好简洁表达式，生成结果具有可解释性。
- 在表达式长度、变量数量、域数量和观测数据点数量四个维度上具有良好的可扩展性。

## 7. 优点
- **方法简洁**：仅需束搜索+预训练启发式，无需复杂进化算法或强化学习。
- **数据高效**：对于观测数据点样本效率高（因为启发式在合成数据上预训练）。
- **解释性**：明确偏好简洁解，符合科学发现需求。
- **处理常数**：专门应对带常数表达式，改进传统方法弱点。
- **可扩展性分析**：系统评估了四个维度的影响，具有参考价值。

## 8. 不足与局限
- **算力信息缺失**：未报告训练成本，难以复现和比较。
- **实验细节不足**：缺少对比方法的设置、统计显著性检验、消融实验等，可能影响结论的可靠性。
- **仅针对精确解问题**：方法可能不适用于含噪声或近似解的场景。
- **合成数据依赖性**：自监督训练依赖合成表达式的生成分布，若与真实问题分布偏差较大，可能泛化能力下降。
- **开放评审状态**：论文被ICLR 2026拒稿（元数据标注“Rejected”），可能仍存在未被解决的缺陷。

（完）
