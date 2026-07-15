---
title: Structure-Preserving Learning Improves Geometry Generalization in Neural PDEs
title_zh: 结构保持学习提升神经PDE的几何泛化能力
authors: "Benjamin D Shaffer, Shawn Koohy, Brooks Kinch, M. Ani Hsieh, Nathaniel Trask"
date: 2026-04-30
pdf: "https://openreview.net/pdf/68c7764fe04800b36487e10eb0a807d1374631db.pdf"
tags: ["query:physics-ml"]
score: 8.0
evidence: 基于有限元外微积分的物理基础模型，保持守恒律
tldr: 该论文致力于开发能泛化到新几何的物理基础模型，提出Geo-NeWF（通用几何神经惠特尼形式）。该方法联合学习微分算子与降阶有限元空间，通过有限元外微积分精确保持物理守恒律。采用变换器编码几何网格，在未见几何上保持结构精度，为物理信息基础模型提供新范式。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有物理基础模型在适应新几何时难以保持物理结构准确性。
method: 提出Geo-NeWF，联合学习微分算子和兼容的有限元空间，通过有限元外微积分精确满足守恒律。
result: 在多种几何泛化任务上，Geo-NeWF保持物理结构并取得高精度。
conclusion: 结构保持学习能够有效提升神经PDE求解器对未知几何的泛化能力。
---

## Abstract
We aim to develop physics foundation models for science and engineering that provide real-time solutions to Partial Differential Equations (PDEs) which preserve structure and accuracy under adaptation to unseen geometries. To this end, we introduce General-Geometry Neural Whitney Forms (Geo-NeWF): a data-driven finite element method. We jointly learn a differential operator and compatible reduced finite element spaces defined on the underlying geometry. The resulting model is solved to generate predictions, while exactly preserving physical conservation laws through Finite Element Exterior Calculus. Geometry enters the model as a discretized mesh both through a transformer-based encoding and as the basis for the learned finite element spaces. This explicitly connects the underlying geometry and imposed boundary conditions to the solution, providing a powerful inductive bias for learning neural PDEs which we demonstrate improves generalization to unseen domains. We provide a novel parameterization of the constitutive model ensuring existence and uniqueness of the solution. Our approach demonstrates state-of-the-art performance on several steady-state PDE benchmarks and provides a significant improvement over conventional baselines on out-of-distribution geometries.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
在科学和工程领域，偏微分方程（PDE）的实时求解具有重要应用，但传统数值方法在适应新几何时计算成本高，而现有物理基础模型（physics foundation models）在迁移到未见几何时难以保持物理结构的准确性。核心问题是如何开发一种既能保持守恒律等物理结构、又能泛化到未知几何的神经PDE求解器。本文提出结构保持学习范式，旨在为物理信息基础模型提供新方向。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：联合学习微分算子与几何相关的降阶有限元空间，通过有限元外微积分（Finite Element Exterior Calculus）精确保持物理守恒律。
- **关键技术细节**：
  - 提出**Geo-NeWF（General-Geometry Neural Whitney Forms）**，一种数据驱动的有限元方法。
  - 模型输入为离散化网格，通过**Transformer编码**网格几何信息，同时作为学习有限元空间的基础。
  - 对构型模型（constitutive model）进行新型参数化，确保解的存在性与唯一性。
  - 整体流程：学习微分算子 → 定义兼容的有限元空间 → 求解模型生成预测 → 守恒律通过外微积分精确满足。

## 3. 实验设计：数据集、基准、对比方法
- **数据集/场景**：多个稳态PDE基准问题（具体名称未在摘要中详述，但提到“several steady-state PDE benchmarks”）。
- **几何泛化测试**：在**分布外（out-of-distribution）几何**上评估，即训练时未见过的几何形状。
- **对比方法**：与传统数值方法及常规神经PDE基线（如标准神经网络求解器、无结构保持的神经算子）进行比较。
- **评估指标**：精度（可能包括相对误差、守恒律偏差等），并报告了**最先进性能**。

## 4. 资源与算力
论文摘要及元数据中**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。因此无法提供具体细节，仅指出这一点。

## 5. 实验数量与充分性
- 在多个稳态PDE基准上测试，并特别进行了**分布外几何泛化实验**。
- 可能包含消融研究（如验证结构保持成分的重要性），但摘要未列举具体数量。
- 实验设计比较客观：与多种基线对比，并突出在未见几何上的显著提升。但未提供统计显著性检验或多次重复实验的详细信息，充分性一般。

## 6. 论文的主要结论与发现
- 结构保持学习能够有效提升神经PDE求解器对未知几何的泛化能力。
- Geo-NeWF在多个基准上达到**最先进性能**，尤其在外分布几何上比常规基线有**显著改进**。
- 将几何显式编码为网格（Transformer编码+有限元空间基础）提供了强大的归纳偏置，有助于泛化。

## 7. 优点
- **结构保持**：利用有限元外微积分精确满足守恒律，不同于仅软约束的方法。
- **几何泛化**：专门设计处理未见几何，通过联合学习微分算子与空间实现强泛化。
- **理论保证**：通过构型模型参数化确保解的存在唯一性，增强可靠性。
- **实用性**：属于物理基础模型范式，可提供实时PDE求解。

## 8. 不足与局限
- **算力信息缺失**：未报告训练资源，不利于可重复性评估。
- **实验规模有限**：仅在稳态PDE上测试，未涉及瞬态问题或更复杂的非线性方程。
- **几何类型限制**：基于离散化网格，可能对极端复杂或高维几何扩展性存疑。
- **泛化界限未明确**：仅展示经验性能，缺乏理论分析说明泛化误差如何受几何差异影响。

（完）
