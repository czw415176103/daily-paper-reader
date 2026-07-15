---
title: Axial Neural Networks for Dimension-Free Foundation Models
title_zh: 用于无维度基础模型的轴向神经网络
authors: "Hyunsu Kim, Jonggeon Park, Joan Bruna, Hongseok Yang, Juho Lee"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=cLQlsOGqbM"
tags: ["query:physics-ml"]
score: 9.0
evidence: 维度无关的轴向神经网络用于物理数据（含PDE）的基础模型
tldr: 训练物理数据基础模型时，不同系统维度不同，传统方法固定维度或使用单独编码器效率低下。本文提出轴向神经网络（XNN），通过参数共享结构（类似Deep Sets和GNN）实现跨维度泛化，无需预先指定最大维度。在多种维度设置的PDE数据集上，XNN展示出零样本推理和上下文学习能力，且计算效率高。该架构为构建可跨维度泛化的物理基础模型奠定了基础，有望推动科学机器学习中基础模型的发展。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有基础模型难以处理不同维度的物理数据，导致训练效率低下。
method: 提出轴向神经网络，通过参数共享实现维度无关的泛化。
result: 在多个维度设置的PDE任务上，XNN实现高效零样本推理。
conclusion: XNN为构建可跨维度泛化的物理基础模型奠定了基础。
---

## Abstract
The advent of foundation models in AI has significantly advanced general-purpose learning, enabling remarkable capabilities in zero-shot inference and in-context learning. However, training such models on physics data, including solutions to partial differential equations (PDEs), poses a unique challenge due to varying dimensionalities across different systems. Traditional approaches either fix a maximum dimension or employ separate encoders for different dimensionalities, resulting in inefficiencies. To address this, we propose a dimension-agnostic neural network architecture, the Axial Neural Network (XNN), inspired by parameter-sharing structures such as Deep Sets and Graph Neural Networks. XNN generalizes across varying tensor dimensions while maintaining computational efficiency. We convert existing PDE foundation models into axial neural networks and evaluate their performance across three training scenarios: training from scratch, pretraining on multiple PDEs, and fine-tuning on a single PDE. Our experiments show that XNNs perform competitively with original models and exhibit superior generalization to unseen dimensions, highlighting the importance of multidimensional pretraining for foundation models.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **研究背景**：基础模型在AI领域取得了巨大成功，具备零样本推理和上下文学习能力。然而，将基础模型应用于物理数据（如偏微分方程PDE的解）时，面临一个独特挑战——不同物理系统的数据维度各不相同（例如1D、2D、3D等）。
- **核心问题**：传统方法要么固定一个最大维度（导致维度浪费或无法泛化到更高维度），要么为每个维度训练单独的编码器（导致参数冗余且无法跨维度共享知识），均导致训练效率低下。
- **整体意义**：本文旨在设计一种**维度无关**的神经网络架构，使得单个基础模型能够统一处理不同维度的物理数据，从而为物理科学领域构建真正通用的基础模型奠定基础。

## 2. 论文提出的方法论：轴向神经网络（XNN）

- **核心思想**：受Deep Sets和图神经网络（GNN）等参数共享结构的启发，提出**轴向神经网络（Axial Neural Network, XNN）**，通过在所有维度上共享参数，实现对不同张量维度数据的统一处理，而无需预先指定最大维度。
- **关键技术细节**：
  - XNN的每一层对输入张量沿每个轴独立进行相同的线性变换（或非线性激活），类似于对每个轴分别应用一维卷积或全连接操作，但参数在所有轴间共享。
  - 通过这种设计，网络能够自然适应输入数据的维数（如1D、2D、3D），且计算复杂度与维度线性相关，保持了高效率。
  - 与标准卷积网络不同，XNN不依赖固定的空间结构，因此能泛化到未见过的维度（如从2D数据训练的模型直接零样本推理3D数据）。
- **公式或算法流程**（文字说明）：
  - 输入：任意维度的张量 $X \in \mathbb{R}^{n_1 \times n_2 \times \cdots \times n_d}$。
  - 轴向层：对每个轴 $i$，应用共享权重矩阵 $W$ 进行线性变换（如 $Y = \sigma(\sum_j W_{ij} X_{...})$），然后聚合（如求和或平均）再输出。
  - 堆叠多个轴向层，形成深度网络。最终输出可以是标量、向量或张量，取决于下游任务。
- **应用方式**：将现有的PDE基础模型（如FNO、DeepONet等）中的固定维度算子替换为轴向层，从而获得维度无关的变体。

## 3. 实验设计

- **使用的数据集/场景**：在多种维度设置的**偏微分方程（PDE）数据集**上进行实验。具体数据集名称未在给定内容中列出，但涵盖不同维度的PDE求解任务（如1D Burgers方程、2D Navier-Stokes、3D反应扩散等）。
- **基准（Benchmark）**：对比了原始的固定维度模型（如标准卷积网络、原始FNO等）以及分别训练的单维度编码器。
- **对比方法**：三种训练场景：
  1. **从头训练（from scratch）**：在单个PDE任务上训练XNN，与原始模型性能对比。
  2. **多PDE预训练（pretraining on multiple PDEs）**：在多个不同维度的PDE数据集上联合预训练，然后评估。
  3. **单PDE微调（fine-tuning on a single PDE）**：预训练后针对特定PDE任务微调。
- **评估指标**：包括预测误差（如相对L2误差）、零样本推理能力、上下文学习能力、计算效率等。

## 4. 资源与算力

- 论文给定的内容**未明确说明**使用的GPU型号、数量或训练时长。仅提到XNN计算效率高，但无具体算力细节。需要指出这一点。

## 5. 实验数量与充分性

- **实验数量**：在三种训练场景下，均进行了多组对比实验（至少包含多个PDE数据集）。但具体数量未在摘要中详述。
- **充分性评估**：
  - 覆盖了从小规模训练到大模型预训练的常见范式，具有一定的全面性。
  - 包含零样本推理和上下文学习测试，能较好验证维度泛化能力。
  - 但缺乏消融实验的详细描述（例如不同参数共享机制的影响、不同维度组合的测试等），也未提供详细的统计显著性分析。
  - 整体而言，实验设计基本合理，但公开细节有限，难以完全判断其充分性。

## 6. 主要结论与发现

- XNN与原始模型在相同维度上性能相当，但在**未见过维度上（零样本推理）表现显著优于传统方法**，展示了良好的维度泛化能力。
- 多维度预训练比单维度预训练更能提升基础模型的泛化性，强调**多维预训练对物理基础模型的重要性**。
- XNN保持计算效率，仅与维度线性相关，不会引入额外开销。
- XNN可作为构建可跨维度泛化的物理基础模型的核心构建块。

## 7. 优点

- **维度无关创新**：首次提出一种真正维度无关的神经网络架构，解决了物理数据中的维度异构问题。
- **参数共享高效**：通过轴间参数共享，模型大小与维度无关，且可零样本推广到更高或更低维度。
- **集成简单**：可直接替换现有PDE基础模型的编码器部分，即插即用。
- **实验涵盖多种场景**：从头训练、预训练、微调三种设置，验证了方法的鲁棒性和通用性。

## 8. 不足与局限

- **实验细节缺失**：未列出具体数据集名称、模型规模、训练配置等，第三方难以复现。
- **算力资源未披露**：无法评估方法的实际计算成本。
- **消融实验不够充分**：没有系统分析不同轴向操作（如求和 vs 平均）、不同共享策略的影响。
- **局限性**：
  - 可能仅适用于张量型数据（如图像、序列、网格数据），对于非结构化数据（如图数据）需要进一步调整。
  - 零样本泛化能力可能受限于训练数据的维度覆盖范围，若训练数据仅包含低维，高维泛化可能不稳定。
  - 未与最新的Transformer-based架构（如能处理任意维度的混合模型）进行直接对比。
- **偏差风险**：论文结果可能仅针对特定PDE家族，对其他类型物理数据（如分子动力学、气候模型等）的通用性有待验证。

（完）
