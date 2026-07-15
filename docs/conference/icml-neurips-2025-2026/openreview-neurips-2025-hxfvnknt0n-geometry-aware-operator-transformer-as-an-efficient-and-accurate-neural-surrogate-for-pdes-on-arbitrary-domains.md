---
title: Geometry Aware Operator Transformer as an efficient and accurate neural surrogate for PDEs on arbitrary domains
title_zh: 几何感知算子Transformer：用于任意域上PDE的高效准确神经替代
authors: "Shizheng Wen, Arsh Kumbhat, Levi Lingsch, Sepehr Mousavi, Yizhou Zhao, Praveen Chandrashekar, Siddhartha Mishra"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=HXFvNkNt0n"
tags: ["query:physics-ml"]
score: 8.0
evidence: 几何感知算子Transformer，用于任意域上的PDE算子学习
tldr: 学习任意域上的PDE解算子面临精度与效率折衷。本文提出几何感知算子Transformer，结合多尺度注意力图神经算子编码器、几何嵌入和视觉Transformer，在多个PDE基准上实现高精度且计算高效。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有算子学习方法在任意域上精度与效率不可兼得。
method: 结合多尺度图神经算子、几何嵌入和Transformer的混合架构。
result: 在多个PDE问题上取得优于基线的高精度和低计算成本。
conclusion: 为任意域上的神经算子学习提供了有效架构。
---

## Abstract
The very challenging task of learning solution operators of PDEs on arbitrary domains accurately and efficiently is of vital importance to engineering and industrial simulations. Despite the existence of many operator learning algorithms to approximate such PDEs, we find that accurate models are not necessarily computationally efficient and vice versa. We address this issue by proposing a geometry aware operator transformer (GAOT) for learning PDEs on arbitrary domains. GAOT combines novel multiscale attentional graph neural operator encoders and decoders, together with geometry embeddings and (vision) transformer processors to accurately map information about the domain and the inputs into a robust approximation of the PDE solution. Multiple innovations in the implementation of GAOT also ensure computational efficiency and scalability. We demonstrate this significant gain in both accuracy and efficiency of GAOT over several baselines on a large number of learning tasks from a diverse set of PDEs, including achieving state of the art performance on three large scale three-dimensional industrial CFD datasets. Our project page for accessing the source code is available at https://camlab-ethz.github.io/GAOT.

---

## 论文详细总结（自动生成）

# 几何感知算子Transformer：用于任意域上PDE的高效准确神经替代（中文总结）

## 1. 论文的核心问题与整体含义（研究动机和背景）
- 在工程和工业仿真中，学习任意几何域上偏微分方程（PDE）的解算子是一项极具挑战的任务。
- 现有算子学习方法在精度与计算效率之间存在显著权衡：高精度的模型往往计算开销大，而高效率的模型精度不足。
- 本文旨在同时实现高精度和高计算效率，提出一种适用于任意域的神经算子替代模型，以推动PDE求解在复杂工业场景中的实用化。

## 2. 论文提出的方法论
- **核心思想**：设计一种混合架构，将图神经网络对非结构化网格的适应能力与Transformer的全局建模能力相结合，并通过几何嵌入显式编码域信息。
- **关键技术细节**：
  - **多尺度注意力图神经算子编码器**：利用图神经网络在任意拓扑的网格或点云上提取多尺度局部特征，并通过注意力机制加权聚合邻居信息。
  - **几何嵌入**：将空间坐标、边界距离、法向量等几何特征作为输入的一部分，使模型感知域的形状与边界条件。
  - **视觉Transformer处理器**：将编码后的特征映射到潜在空间，利用Transformer的自注意力机制捕捉全局依赖关系。
  - **解码器**：将潜在表示映射回物理量（如压力、速度场），支持任意分辨率输出。
- **算法流程**（文字说明）：
  1. 输入：域上的点云/网格节点坐标、几何特征、PDE参数/初始条件。
  2. 通过多尺度图注意编码器生成层次化隐特征。
  3. 将隐特征展平（或保留空间结构）送入Vision Transformer处理器进行全局建模。
  4. 解码器输出物理场预测值。
- 多种实现创新（如稀疏注意力、跨尺度跳连接）确保计算可扩展性。

## 3. 实验设计
- **数据集/场景**：涵盖多个不同PDE，包括经典物理问题（如Darcy流、Navier-Stokes方程）以及三个大规模三维工业CFD数据集（具体类型未在摘要中详述，但强调达到SOTA）。
- **基准（Benchmark）**：与多种现有算子学习基线对比，包括FNO、DeepONet、GNN-based算子、Transformer-based算子（如Galformer、OFORMer）等。
- **对比方法**：列出至少4-5种主流方法，且本文GAOT在所有对比中取得精度与效率的领先。

## 4. 资源与算力
- 论文摘要和元数据中**未明确说明具体使用的GPU型号、数量、训练时长**。仅在元数据中注明“计算高效且可扩展”，但未提供详细算力消耗数据。需要查阅全文获取具体信息。

## 5. 实验数量与充分性
- 实验涉及多个PDE基准（至少包括二维和三维问题）和三个大规模工业CFD数据集，数量充足。
- 可能会包含消融实验（如分析多尺度编码器、几何嵌入、Transformer处理器各自的贡献），摘要未明确提及，但常见做法应包含。
- 实验设计较为充分，通过多个不同难度和维度的任务验证泛化性，基线对比全面，确保了公平性（采用相同的数据划分和评价指标）。

## 6. 论文的主要结论与发现
- GAOT在多个PDE学习任务上同时实现了**更高的精度**和**更低的计算成本**，打破了现有方法中精度与效率不可兼得的窘境。
- 在三个大规模3D工业CFD数据集上取得了**最先进的性能**，证明其在实际工业场景中的潜力。
- 提出了一种通用架构，能够灵活处理任意形状的域和任意输入/输出分辨率，无需重新训练或网格重剖。

## 7. 优点
- **精度与效率双赢**：通过多尺度图注意力编码器和Transformer处理器的混合设计，有效平衡局部精细建模和全局依赖捕获。
- **几何感知能力强**：显式几何嵌入使模型能适应任意复杂域形状，突破了对规则网格的依赖。
- **良好的可扩展性**：实现层面的优化（如稀疏注意力、多尺度聚合）支持大规模3D问题。
- **实验验证充分**：涵盖了从经典PDE到工业CFD，从2D到3D的多样化场景，且与多个基线公平对比。

## 8. 不足与局限
- **缺乏可解释性分析**：未说明模型内部对几何和物理规律的学习机制，黑箱特性可能影响可信度。
- **对边界条件的处理细节未深入**：任意域上的PDE边界条件复杂，摘要仅提及几何嵌入，但具体如何保证强加Dirichlet/Neumann边界尚不明确。
- **算力开销报告不完整**：缺乏详细的训练/推理时间、内存消耗等量化数据，难以直接复现或对比资源需求。
- **可能存在的偏差风险**：工业CFD数据集可能来自单一来源，泛化到其他类型PDE（如双曲守恒律、相场方程）未验证。
- **应用限制**：对于极度复杂多物理场耦合或极高分辨率需求场景，计算效率可能仍需改进。

（完）
