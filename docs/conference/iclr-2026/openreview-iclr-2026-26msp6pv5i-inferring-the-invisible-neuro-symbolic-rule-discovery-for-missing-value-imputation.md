---
title: "Inferring the Invisible: Neuro-Symbolic Rule Discovery for Missing Value Imputation"
title_zh: 推断不可见：用于缺失值插补的神经符号规则发现
authors: "Wendi Ren, Ke Wan, Junyu Leng, Shuang Li"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=26Msp6pV5i"
tags: ["query:sr"]
score: 7.0
evidence: 用于缺失数据的神经符号规则发现
tldr: 该论文提出神经符号框架，通过交织神经表示学习和符号规则归纳，迭代发现解释观测模式的逻辑规则并恢复缺失条目，实现了符号表达式在科学数据中的发现应用。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 在部分可观测性下，关键数值缺失影响系统理解。
method: 将缺失值视为潜在谓词，通过逻辑推理和神经符号交替学习挖掘规则。
result: 能处理异构数据，有效恢复缺失条目。
conclusion: 为符号规则发现在数据修复中提供了新视角。
---

## Abstract
One of the central challenges in artificial intelligence is reasoning under partial observability, where key values are missing but essential for understanding and modeling the system. This paper presents a neuro-symbolic framework for latent rule discovery and missing value imputation. In contrast to traditional latent variable models, our approach treats missing grounded values as latent predicates to be inferred through logical reasoning. By interleaving neural representation learning with symbolic rule induction, the model iteratively discovers—both conjunctive and disjunctive rules—that explain observed patterns and recover missing entries. Our framework seamlessly handles heterogeneous data, reasoning over both discrete and continuous features by learning soft predicates from continuous values. Crucially, the inferred values not only fill in gaps in the data but also serve as supporting evidence for further rule induction and inference—creating a feedback loop in which imputation and rule mining reinforce one another. Using a staged block-coordinate gradient descent, the system learns these rules end-to-end by iteratively optimizing over parameter blocks in an alternating fashion. Experiments on both synthetic and real-world datasets demonstrate that our method effectively imputes missing values while uncovering meaningful, human-interpretable rules that govern system dynamics.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在部分可观测性（partial observability）下，关键数值的缺失会阻碍对系统的理解与建模。传统隐变量模型往往将缺失值视为不可观测的随机变量，缺乏可解释性和逻辑推理能力。
- **研究动机**：如何在缺失数据场景下，不仅能恢复缺失条目，还能同时发现可解释的、支配系统行为的逻辑规则，形成“插补-规则发现”的相互强化循环。
- **整体含义**：该论文提出一种神经符号（neuro-symbolic）框架，将缺失的实值（grounded values）视为待通过逻辑推理推断的潜在谓词（latent predicates），通过交织神经表示学习与符号规则归纳，迭代地发现解释观测模式的逻辑规则并恢复缺失条目。框架能够处理异构数据（离散和连续特征），并实现端到端的可微学习。

## 2. 论文提出的方法论

- **核心思想**：将缺失值插补问题转化为一种归纳逻辑编程任务，通过软谓词（soft predicates）从连续值中学习，并交替优化神经表示块和符号规则块，使插补与规则挖掘相互促进。
- **关键技术细节**：
  - 将缺失值视为**潜在谓词**，通过逻辑推理进行推断。
  - 同时发现**合取规则（conjunctive）和析取规则（disjunctive）**，以解释观测模式。
  - 采用**阶段式块坐标梯度下降（staged block-coordinate gradient descent）**，端到端地交替优化参数块。
  - 融合神经网络（从原始数据中学习软谓词）与符号规则归纳引擎。
- **公式或算法流程**（文字说明）：
  1. 初始化：对连续特征学习软谓词（如通过神经网络将连续值映射到[0,1]区间作为软真值）。
  2. 交替迭代：
     - **神经阶段**：固定符号规则，通过梯度下降更新软谓词参数和缺失值的嵌入表示，使得规则能够更好拟合观测数据。
     - **符号阶段**：固定软谓词和缺失值估计，使用归纳逻辑编程方法（如基于规则模板或可微规则学习）从当前数据中发掘新的合取/析取规则，规则覆盖率和准确率作为优化目标。
  3. 重复迭代直至收敛，输出完整的插补结果和一组可解释的逻辑规则。

## 3. 实验设计

- **数据集/场景**：论文在合成数据集和真实世界数据集上进行了实验。具体数据集名称未在元数据中提供（如UCI数据集、特定领域数据等）。
- **Benchmark**：未明确说明所用的基准测试集或评估协议。
- **对比方法**：元数据中未列出对比的基线方法（如传统统计插补、矩阵填充、深度生成模型等）。

## 4. 资源与算力

- 论文中没有明确说明所使用的GPU型号、数量、训练时长等算力信息。仅在元数据中未提及。

## 5. 实验数量与充分性

- **实验数量**：从元数据可推断，使用了合成和真实数据集至少两组实验，但未具体说明消融实验、参数敏感性分析等的数量。
- **充分性**：由于缺乏详细实验配置和对比结果，无法判断实验的充分性、客观性与公平性。摘要中仅声称方法有效，但未提供定量指标（如MAE、RMSE、规则准确率等）。

## 6. 论文的主要结论与发现

- 提出了一种神经符号框架，能够有效插补缺失值，同时发现有意义、可解释的规则，这些规则描述了系统动态。
- 插补与规则挖掘相互加强，形成正向反馈。
- 方法在处理异构数据方面具有无缝性。

## 7. 优点

- **创新融合**：将缺失值插补与可解释规则发现结合起来，打破传统黑箱方法。
- **可解释性**：输出的是逻辑规则，易于人类理解，适用于科学发现场景。
- **异构数据支持**：同时处理离散和连续特征，无需特征工程。
- **端到端可微**：利用块坐标梯度下降实现联合优化，便于集成到深度学习框架中。

## 8. 不足与局限

- **实验信息不足**：元数据中未提供具体数据集、对比方法、定量结果，无法评估方法的实际性能与泛化能力。
- **缺乏消融分析**：未说明各组件（如神经模块、符号模块）的贡献。
- **算力与可扩展性未知**：未讨论在大规模数据或高维规则空间下的计算效率。
- **应用限制**：仅适用于可表达为一阶逻辑规则的系统，对于高度非线性或随机性强的数据可能不适用。
- **假设依赖**：规则的存在性假设可能不总是成立，若数据无潜在规则，方法可能失效。

（完）
