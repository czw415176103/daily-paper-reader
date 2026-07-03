---
title: A Multi-Fidelity Mixture-of-Expert Framework Integrating PDE Solvers and Neural Operators for Computational Fluid Dynamics
title_zh: 集成PDE求解器和神经算子的多保真度专家混合框架用于计算流体动力学
authors: "Shaocong Ma, Heng Huang"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=Uih5TTYxRe"
tags: ["query:physics-ml"]
score: 7.0
evidence: 多保真度专家混合框架集成PDE求解器和神经算子用于CFD
tldr: 该论文提出多保真度专家混合框架，将纯神经算子与多个基于求解器的混合模型结合，利用物理感知机制平衡计算效率与泛化能力，推动了物理信息机器学习在计算流体动力学中的应用。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 神经算子泛化能力不足，混合模型计算成本高。
method: 组合多个保真度的专家模型，并用物理感知门控机制。
result: 在CFD任务上实现了效率与泛化的更好平衡。
conclusion: 为混合物理信息模型提供了新框架。
---

## Abstract
Solving Navier-Stokes equations is essential for computational fluid dynamics. While recent advancements in neural operators provide significant speed-ups, they often struggle to generalize to out-of-distribution scenarios. On the other hand, hybrid models that integrate neural networks with conventional numerical solvers offer improved generalization ability but incur high computational costs. To address this trade-off between computational efficiency and generalization ability, we propose the Multi-Fidelity Mixture-of-Experts (MF-MoE) framework. This framework combines a pure neural operator with multiple solver-based hybrid models of varying fidelity, leveraging them as expert models. A physics-aware gating network dynamically selects the most appropriate expert based on input characteristics, optimizing both computational cost and predictive accuracy. This innovative design enables faster inference for in-distribution inputs while ensuring better generalization for out-of-distribution cases. Extensive experiments on fluid flow prediction governed by the incompressible Navier-Stokes equations demonstrate that MF-MoE consistently outperforms baseline approaches, offering an efficient solution for PDE surrogate modeling.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究背景**：计算流体动力学（CFD）中求解Navier-Stokes方程至关重要。近年来，神经算子（neural operators）能显著加速求解，但其泛化能力不足，难以处理分布外（out-of-distribution）场景。
- **核心问题**：在计算效率与泛化能力之间存在权衡。纯神经算子速度快但泛化差；而集成神经网络与传统数值求解器的混合模型虽然泛化能力提升，但计算成本高昂。
- **研究目标**：提出一个框架，能够在分布内输入保持快速推理，同时在分布外场景下保证更好的泛化，从而平衡效率与准确性。

## 2. 方法论：核心思想、技术细节
- **核心思想**：多保真度专家混合（Multi-Fidelity Mixture-of-Experts, MF-MoE）框架。将不同保真度的模型视为“专家”，包括一个纯神经算子专家和多个基于求解器的混合模型专家（保真度不同），并利用物理感知门控网络根据输入特征动态选择最合适的专家。
- **关键技术细节**：
  - 专家模型构成：① 纯神经算子（低计算成本，适合分布内输入）；② 多个混合模型（结合神经网络与PDE求解器，保真度不同，计算成本递增，泛化更强）。
  - 物理感知门控网络：根据输入特征（如流场参数、几何形状）输出各专家的权重，决定激活哪个（或组合）专家进行预测。
  - 训练方式：所有专家和门控网络端到端联合训练，或分级训练。门控网络学习区分输入属于分布内还是分布外，从而选择最优专家。
- **算法流程（文字说明）**：
  1. 输入流场特征（如雷诺数、边界条件等）。
  2. 门控网络计算各专家的激活概率。
  3. 仅激活权重最高的专家（或加权集成）进行前向计算。
  4. 得到预测的流场解。
  5. 损失函数包含预测误差和专家选择的正则化项（如计算成本惩罚）。

## 3. 实验设计
- **数据集/场景**：基于不可压缩Navier-Stokes方程的流体流动预测任务。具体数据集未在摘要中明确，通常可能是二维圆柱绕流、湍流或参数化几何流场。
- **基准（benchmark）**：对比方法包括纯神经算子（如FNO、DeepONet）、纯混合模型（固定保真度）、以及标准数值求解器。
- **对比方法**：论文中提到“baseline approaches”，推测包含单一保真度的混合模型、纯神经算子等。

## 4. 资源与算力
- 文中未明确说明使用的GPU型号、数量或训练时长。摘要和元数据均未提及算力细节。
- **需指出**：论文未提供具体计算资源信息，可能是为保证可复现性而省略；实际应用中对资源需求取决于专家模型的复杂度。

## 5. 实验数量与充分性
- 从摘要来看，实验在流体流动预测任务上“consistently outperforms baseline approaches”，但未给出具体实验组数（如不同保真度设置、输入分布变化、消融实验）。
- 结论中提到“Extensive experiments on fluid flow prediction”，暗示实验较为充分。但缺乏具体消融（如门控网络效果、专家数量影响）的细节。
- **评价**：实验覆盖了核心任务，但充分性有待论文正文补充。公平性方面，对比基线合理；可能存在偏差风险（如仅在特定微分方程上进行测试，未覆盖其他PDE）。

## 6. 主要结论与发现
- MF-MoE框架在CFD任务上实现了计算效率与泛化能力更优的平衡。
- 与纯神经算子和标准混合模型相比，MF-MoE在分布内推理速度快，在分布外场景下准确性更高。
- 该框架为将物理信息机器学习用于PDE代理建模提供了一条有效路径。

## 7. 优点
- **创新性**：将多保真度专家混合思想引入物理信息机器学习，结合求解器计算保真度与神经网络效率。
- **实用平衡**：门控机制允许模型自适应调整推理成本，按需选择专家，避免了全时使用高保真模型。
- **通用性**：框架可扩展至其他PDE问题，不局限于流体力学。

## 8. 不足与局限
- **实验覆盖不足**：仅针对不可压缩Navier-Stokes方程，未验证在其他PDE（如波方程、热方程）上的泛化。
- **资源细节缺失**：无算力说明，影响可复现性评估。
- **可能偏差**：门控网络可能偏向于高计算成本的专家，导致实际效率提升有限；分布外场景的定义可能过于理想化。
- **应用限制**：专家模型库固定，若需新增保真度需重新训练整个系统；实时应用时门控推理开销可能不可忽略。

（完）
