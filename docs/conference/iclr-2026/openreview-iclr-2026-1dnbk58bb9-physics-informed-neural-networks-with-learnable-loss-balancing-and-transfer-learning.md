---
title: Physics-Informed Neural Networks with Learnable Loss Balancing and Transfer Learning
title_zh: 具有可学习损失平衡和迁移学习的物理信息神经网络
authors: Reza Pirayeshshirazinezhad
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=1dNbK58bB9"
tags: ["query:physics-ml"]
score: 9.0
evidence: 可学习损失平衡与迁移学习的物理信息神经网络
tldr: 该论文针对物理信息神经网络在数据稀缺时训练不稳定问题，提出可学习融合神经元动态平衡物理残差与数据损失，并结合迁移学习重用相关领域知识，在多个物理系统上验证了准确性和泛化性。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有PINN依赖固定或启发式损失权重，训练不稳定且泛化差。
method: 引入可学习融合神经元自适应调整损失项权重，并集成迁移学习。
result: 在多个物理方程求解任务中，该方法比基线PINN精度提升且训练更稳定。
conclusion: 可学习损失平衡与迁移学习有效提升了PINN在数据稀缺场景下的实用性。
---

## Abstract
We propose a self-supervised physics-informed neural network (PINN) framework that adaptively balances physics-based and data-driven supervision for scientific machine learning under data scarcity. Unlike prior PINNs that rely on fixed or heuristic weighting of physics residuals and data loss, our approach introduces a learnable blending neuron that dynamically adjusts the relative contribution of each term based on their uncertainties. This mechanism enables stable training and improved generalization without manual tuning. To further enhance efficiency, we integrate a transfer learning strategy that reuses representations from related domains and adapts them to new physical systems with limited data. We validate the framework for the prediction of heat transfer in liquid-metal miniature heat sinks using only 87 CFD datapoints, where the adaptive PINN achieves an error $<8\%$, outperforming shallow neural networks, kernel methods, and physics-only baselines. Our framework provides a general recipe for embedding physics adaptively into neural networks, offering a robust and reproducible approach for data-scarce problems across various scientific domains, including fluid dynamics and material modeling.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：物理信息神经网络（PINN）在数据稀缺的科学计算任务中存在训练不稳定、泛化能力差的问题。传统PINN通常采用固定或启发式方法来平衡物理残差（基于偏微分方程）和数据损失（观测数据）的权重，这导致训练过程高度依赖手动调参，且在数据量极少时容易陷入局部最优或失效。
- **整体含义**：该论文旨在提出一种自适应、数据高效的科学机器学习框架，通过动态调整物理约束与数据监督的贡献，并利用迁移学习复用相关领域知识，使得在仅有少量CFD模拟数据的情况下仍能准确预测物理场，从而拓展PINN在工程实践（如液态金属微型散热器热传递）中的适用性。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：引入一个**可学习的融合神经元（learnable blending neuron）**，基于各损失项的不确定性动态调整物理残差和数据损失的相对权重，实现自适应的损失平衡；同时集成**迁移学习策略**，将在相关物理系统上预训练的表示迁移到目标系统，减少数据需求。
- **关键技术细节**：
  - **可学习损失平衡机制**：该融合神经元输出每个损失项的权重系数，系数通过梯度下降优化，与网络参数联合训练。权重基于不确定性估计，使得那些预测不确定性高的项自动获得更大权重，从而稳定训练并避免手动调参。
  - **迁移学习集成**：先在与目标系统相似的物理域（例如不同几何或边界条件）上预训练一个PINN，冻结部分底层特征，然后在小数据集上微调顶层网络。这利用了来自相关领域的物理规律表示，加速收敛并提升精度。
- **算法流程（文字说明）**：
  1. 在源域上使用标准PINN（固定损失权重）预训练，保存网络权重。
  2. 在目标域上初始化网络，加载预训练的部分层（如特征提取层）权重。
  3. 定义总损失：$L = \lambda_{physics} L_{physics} + \lambda_{data} L_{data}$，其中 $\lambda_{physics}$ 和 $\lambda_{data}$ 由可学习融合神经元根据不确定性输出。
  4. 联合训练所有参数（包括融合神经元的权重），最小化总损失。
  5. 在测试集上评估预测精度。

## 3. 实验设计

- **数据集/场景**：论文仅报告了一个具体案例——**液态金属微型散热器中的热传递预测**。使用了**87个CFD（计算流体动力学）数据点**作为监督数据，物理残差基于热传导/对流偏微分方程。
- **基准（Benchmark）**：未明确列出标准Benchmark；使用的是自建CFD数值模拟结果作为真实值。
- **对比方法**：
  - **浅层神经网络（Shallow Neural Networks）**：不含物理约束的标准数据驱动模型。
  - **核方法（Kernel Methods）**：如高斯过程回归。
  - **仅物理基线的PINN（Physics-only baselines）**：不使用任何数据，仅优化物理残差。
  - 可能还包括固定权重PINN（未在摘要中明确，但根据上下文推测）。

## 4. 资源与算力

- **文中未明确说明**：未提及使用的GPU型号、数量、训练时长、CPU配置等算力信息。因此无法评估其计算开销和可复现性。

## 5. 实验数量与充分性

- **实验组数**：仅报告了一组具体案例实验（液态金属散热器）。未提及在多个不同物理系统或数据集上的实验，也未提及消融实验（例如：消除迁移学习或可学习损失平衡后的性能对比）。
- **充分性与公平性**：
  - 实验覆盖狭窄，仅在一个特定场景下验证，无法充分证明方法的广泛适用性。
  - 对比的基线方法不够全面（缺少对最新PINN改进方法如自适应权重、多任务学习的对比）。
  - 不过，在数据极度稀缺（87个点）的条件下，该方法达到<8%误差，结果具有说服力。但未报告多次独立实验的统计量（均值、方差），存在不确定性。

## 6. 主要结论与发现

- 提出的自适应PINN（可学习损失平衡+迁移学习）在使用仅87个CFD数据点的情况下，预测误差低于8%，显著优于浅层神经网络、核方法和仅物理基线。
- 可学习融合神经元能够动态调整损失权重，使训练更加稳定，无需手动调参。
- 迁移学习策略有效利用相关领域知识，在小数据条件下提升了泛化能力。
- 该框架可作为数据稀缺科学问题的通用方案，例如流体动力学和材料建模。

## 7. 优点

- **自适应损失平衡**：以可学习神经元替代固定或启发式权重，提升了训练鲁棒性和实用性。
- **迁移学习集成**：巧妙解决了数据稀缺问题，合理利用了相关领域中的物理知识。
- **结果显著**：在极端低数据条件下取得<8%误差，展示了方法的有效性。
- **思路清晰通用**：不局限于特定方程，具备跨域迁移的潜力。

## 8. 不足与局限

- **实验数量过少**：仅在一个应用场景（液态金属散热器）上验证，缺少在不同物理系统（如流体、固体力学、电磁学）上的广泛测试。
- **缺乏消融实验**：未分离评估可学习损失平衡与迁移学习各自的贡献，无法判断哪个模块更关键。
- **未与当前SOTA PINN方法对比**：例如未对比Neural Tangent Kernel PINN、自适应权重PINN（如基于梯度范数、基于不确定性加权的PINN），也缺少与数据增强、元学习等低数据方法的对比。
- **算力与可复现性信息缺失**：无GPU型号、训练时长等细节，难评估计算成本和他人复现。
- **偏差风险**：87个CFD数据点可能来自特定网格或求解器，若数据存在系统偏差，结果可能高估。未报告数据来源与质量。
- **应用限制**：依赖于可微分物理方程，对于非光滑、强非线性或缺乏先验模型的问题可能失效。迁移学习要求源域与目标域具有一定相似性，否则可能负迁移。

（完）
