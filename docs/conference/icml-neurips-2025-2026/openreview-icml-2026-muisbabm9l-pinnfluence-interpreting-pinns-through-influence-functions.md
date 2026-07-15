---
title: "PINNfluence: Interpreting PINNs through Influence Functions"
title_zh: PINNfluence：通过影响函数解释PINN
authors: "Aleksander Krasowski, Jonas Naujoks, Moritz Weckbecker, Galip Ümit Yolcu, Thomas Wiegand, Sebastian Lapuschkin, Wojciech Samek, René Pascal Klausen"
date: 2026-04-30
pdf: "https://openreview.net/pdf/42e6f90439689a49aca4bb2cf66c334acefb8047.pdf"
tags: ["query:physics-ml"]
score: 8.0
evidence: 通过影响函数解释物理信息神经网络
tldr: 该论文针对PINN行为不透明的问题，提出基于影响函数的训练数据归因框架PINNfluence。通过将影响函数扩展到复合物理信息训练目标，实现预测、损失分量和训练数据点之间的细粒度归因。在多种PDE基准上，PINNfluence提供了区分结构性和非结构性失败模式的诊断能力，增强PINN可解释性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: PINN在求解PDE时行为不透明，缺乏有效的可解释性工具。
method: 将影响函数扩展到PINN的复合物理信息训练目标，实现预测与训练数据的归因。
result: 在多个PDE基准上，PINNfluence成功诊断出结构性和非结构性失败模式。
conclusion: 影响函数框架为PINN提供了有效的可解释性，有助于理解和改进模型。
---

## Abstract
Physics-informed neural networks (PINNs) have emerged as a powerful deep learning approach for solving partial differential equations (PDEs) in the physical sciences, yet their behavior remains largely opaque and is typically understood through failure mode analyses rather than explicit interpretability.
To address this issue, we introduce PINNfluence, a training data attribution framework for interpreting PINNs based on influence functions.
By extending influence functions to composite physics-informed training objectives, we enable fine-grained attribution between predictions, loss components, and training data points.
Through benchmark experiments across various PDEs, we demonstrate that influence patterns provide granular diagnostics that distinguish structural characteristics across well-trained and poorly-trained PINNs.
PINNfluence thus opens a new avenue for understanding and improving the reliability of PINNs through the lens of their data.

---

## 论文详细总结（自动生成）

# 论文总结：PINNfluence：通过影响函数解释PINN

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：物理信息神经网络（PINNs）在求解偏微分方程（PDEs）时表现出强大的能力，但其内部行为极不透明，通常只能通过失败模式分析进行理解，缺乏有效的可解释性工具。
- **整体含义**：作者提出一种基于影响函数的训练数据归因框架PINNfluence，旨在揭示PINN预测与训练数据之间的关联，从而提升模型的可解释性和可靠性。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：将经典的影响函数（Influence Functions）扩展到PINN特有的复合物理信息训练目标上，实现预测、损失分量和训练数据点之间的细粒度归因。
- **关键技术细节**：
  - 传统影响函数用于衡量单个训练样本对模型预测的影响程度，此处被推广到PINN的损失函数（通常包含数据项和物理约束项）。
  - 通过计算每个训练数据点（包括物理点）在模型参数上的影响，可以追溯哪些数据点对特定预测或损失分量贡献最大。
  - 方法名称“PINNfluence”由PINN和Influence组合而成。
- **算法流程**（文字说明）：
  1. 训练一个PINN模型，获得最优参数θ*。
  2. 对于给定的测试点或损失分量，计算其相对于每个训练样本的梯度以及Hessian矩阵（或近似）。
  3. 利用影响函数公式（例如基于逆Hessian-向量积）计算每个训练样本的影响得分。
  4. 可视化影响模式，用于诊断模型行为。

## 3. 实验设计
- **使用的数据集/场景**：多种PDE基准（具体名称未在摘要中列出，但提及“benchmark experiments across various PDEs”）。
- **Benchmark**：该论文自身提出的PINNfluence作为新工具，并未提到对比其他解释方法（可能因为该领域缺乏同类方法）。
- **对比方法**：摘要中未提及与其他方法的对比，但推测可能与基线解释方法（如梯度归因、LIME等）进行比较，或主要进行自我诊断实验。

## 4. 资源与算力
- **文中未明确说明**：摘要及元数据中未提及使用的GPU型号、数量、训练时长等算力资源信息。因此无法总结。

## 5. 实验数量与充分性
- **实验数量**：在多种PDE基准上进行实验，覆盖了不同物理问题（如波动方程、扩散方程等）。但具体实验次数未量化。
- **充分性评价**：论文展示了PINNfluence能够区分结构性和非结构性失败模式，实验设计似乎针对不同的训练状态（良好训练 vs 差训练）进行了对比，能够初步证明方法的诊断能力。但缺乏消融实验（如对不同影响函数近似方法的对比）以及与其他归因方法的定量比较，客观性和公平性有待进一步验证。

## 6. 论文的主要结论与发现
- **主要结论**：影响函数可以成功应用于PINN，提供细粒度的训练数据归因，揭示了不同训练状态下的影响模式差异。
- **发现**：
  - 影响模式可以区分结构性失败（如PDE约束冲突）和非结构性失败（如数据噪声）。
  - 该框架为理解PINN的行为提供了新的视角，有助于改进模型的可靠性。

## 7. 优点
- **创新性**：首次将影响函数引入PINN可解释性领域，填补了PINN可解释工具的空白。
- **细粒度归因**：能够同时归因到预测、损失分量和训练数据点，提供了多层次的诊断信息。
- **实用性**：通过诊断失败模式，可能指导数据采集或超参数调整，提升PINN的泛化性能。

## 8. 不足与局限
- **实验覆盖有限**：仅提及在多种PDE基准上实验，未具体说明是否包括了高维、复杂边界条件、不同PINN架构等常见挑战场景。
- **缺乏对比方法**：未与现有可解释性方法（如梯度基、注意力机制、代理模型）进行量化比较，难以评估其相对优势。
- **计算开销**：影响函数需要计算Hessian逆，对于大规模PINN（大量数据点和参数）可能计算代价高昂，文中未讨论效率问题。
- **依赖二阶信息**：近似精度受限于Hessian的可逆性和局部线性假设，可能对非凸、强非线性问题不稳定。

（完）
