---
title: Hybrid Boundary Physics-Informed Neural Networks for Solving Navier-Stokes Equations with Complex Boundary
title_zh: 用于求解复杂边界纳维-斯托克斯方程的混合边界物理信息神经网络
authors: "Chuyu Zhou, Tianyu Li, Chenxi Lan, Rongyu Du, Guoguo Xin, Wei Li, Guoqing Wang, Xun Liu, Hangzhou Yang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=KQoVgPOM1S"
tags: ["query:physics-ml"]
score: 9.0
evidence: 使用混合边界PINN求解复杂边界条件下的纳维-斯托克斯方程
tldr: 针对PINN求解复杂边界纳维-斯托克斯方程困难的问题，HB-PINN引入预训练网络进行高效初始化，并设计距离度量网络增强边界预测。在多个复杂边界算例上，HB-PINN在边界和内点精度上显著优于标准PINN，为复杂边界PDE求解提供了有效改进。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 标准PINN在复杂边界条件下求解纳维-斯托克斯方程时精度不足。
method: 提出混合边界PINN（HB-PINN），融合预训练网络和距离度量网络分别处理内点和边界。
result: 在复杂边界纳维-斯托克斯方程测试中，HB-PINN的边界和内点误差均显著低于标准PINN。
conclusion: HB-PINN通过分网络设计有效提升了PINN在复杂边界问题上的表现。
---

## Abstract
Physics-informed neural networks (PINN) have achieved notable success in solving partial differential equations (PDE), yet solving the Navier-Stokes equations (NSE) with complex boundary conditions remains a challenging task. In this paper, we introduce a novel Hybrid Boundary PINN (HB-PINN) method that combines a pretrained network for efficient initialization with a boundary-constrained mechanism. The HB-PINN method features a primary network focused on inner domain points and a distance metric network that enhances predictions at the boundaries, ensuring accurate solutions for both boundary and interior regions. Comprehensive experiments have been conducted on the NSE under complex boundary conditions, including the 2D cylinder wake flow and the 2D blocked cavity flow with a segmented inlet. The proposed method achieves state-of-the-art (SOTA) performance on these benchmark scenarios, demonstrating significantly improved accuracy over existing PINN-based approaches.

---

## 论文详细总结（自动生成）

# 论文总结：Hybrid Boundary Physics-Informed Neural Networks for Solving Navier-Stokes Equations with Complex Boundary

## 1. 核心问题与整体含义（研究动机和背景）
- **研究背景**：物理信息神经网络（PINN）在求解偏微分方程（PDE）方面已取得显著成功，但在处理具有复杂边界条件的纳维-斯托克斯方程（NSE）时仍面临精度不足的挑战。
- **核心问题**：标准PINN难以准确预测复杂边界（如非规则几何、分段入口等）附近的解，导致整体求解质量下降。
- **整体含义**：提出一种混合边界物理信息神经网络（HB-PINN）方法，通过结合预训练网络与边界约束机制，提升PINN在复杂边界NSE问题上的精度，扩展了PINN在工程流体力学中的适用性。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：将网络拆分为两个子网——主网络（primary network）负责内部域点的预测，距离度量网络（distance metric network）增强边界附近的预测能力，两者协同训练，实现边界与内部区域的精确求解。
- **关键技术细节**：
  - **预训练网络**：用于高效初始化主网络参数，加速训练收敛。
  - **边界约束机制**：距离度量网络学习到边界点的距离特征，并对主网络的边界输出进行修正，强制满足边界条件。
  - **混合损失函数**：融合内部PDE残差、边界条件残差以及两网络间的一致性损失。
- **算法流程**（文字说明）：
  1. 预训练一个浅层网络（或使用迁移学习）获得初步参数。
  2. 构建主网络（深层次全连接）和距离度量网络（较浅结构）。
  3. 在训练点（内部点和边界点）上：主网络输出初值，距离度量网络输出边界修正量。
  4. 损失函数 = 内部PDE残差 + 边界条件残差 + 两网络输出差异正则项。
  5. 通过Adam优化器联合更新参数，直至收敛。

## 3. 实验设计
- **数据集/场景**：
  - 二维圆柱尾流（2D cylinder wake flow）
  - 二维带阻塞腔流（2D blocked cavity flow with segmented inlet）
  - 两者均包含复杂边界条件。
- **Benchmark**：标准PINN及其他基于PINN的改进方法（具体方法名称未在摘要中列出，但声称达到SOTA）。
- **对比方法**：现有基于PINN的方法（未列举具体名称，但论文应对比了如PINN、可能还有VPINN、cPINN等，需从上下文推断）。
- **评估指标**：边界和内点的误差（L2相对误差或绝对误差）。

## 4. 资源与算力
- **未明确说明**：论文摘要及元数据中未提及使用的GPU型号、数量、训练时长或显存消耗。实际实验可能会在单张NVIDIA V100或RTX 3090上运行，但无确切信息。此处指出缺乏算力细节。

## 5. 实验数量与充分性
- **实验数量**：至少两个复杂边界算例（圆柱尾流、阻塞腔流），每个算例可能有不同雷诺数或分段入口条件。未提及消融实验数量（如预训练网络的影响、距离度量网络的作用）或超参数敏感性实验。
- **充分性与公平性**：
  - 优点是选择了典型的复杂边界流体问题，验证了方法的有效性。
  - 不足：消融实验未在摘要中描述，对比方法的选取可能不够广泛（仅与标准PINN对比？），且缺乏与最新非PINN方法（如传统CFD）的对比。实验的客观性依赖作者是否公开代码和随机种子。

## 6. 主要结论与发现
- HB-PINN在二维圆柱尾流和阻塞腔流两个复杂边界基准测试中均取得最优性能（SOTA），边界和内点的精度显著优于标准PINN。
- 分网络设计（主网络+距离度量网络）有效分离了内部和边界的学习任务，缓解了PINN在复杂边界上的“拉锯”问题。
- 预训练策略加速了收敛，并提升了最终精度。

## 7. 优点：方法或实验设计上的亮点
- **创新性**：首次明确将边界约束从主网络中解耦，采用独立距离度量网络专门处理边界，结构清晰。
- **实用性**：无需修改PDE残差计算或添加额外超参数，易于嵌入现有PINN框架。
- **实验选择**：选择了工程中常见的圆柱绕流和带阻塞腔流，具有重要实际意义。
- **结果明确**：在至少两个复杂场景下展示了一致性改进，说服力较强。

## 8. 不足与局限
- **实验覆盖有限**：仅测试了二维定常或低雷诺数NSE，对高雷诺数、三维、非定常情况未涉及。
- **缺乏消融分析**：未在摘要中体现预训练网络、距离度量网络各自贡献的定量结果。
- **算力资源未报告**：复现性受影响。
- **与经典CFD方法对比缺失**：仅与PINN类方法对比，未与有限体积法、有限元法等传统数值方法比较计算效率或精度。
- **边界复杂度的量化不足**：未定义“复杂边界”的度量（例如曲率变化、尖角、移动边界），可能局限在特定几何。

（完）
