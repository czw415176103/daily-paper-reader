---
title: Physics-informed coarsening for multigrid graph neural networks surrogates
title_zh: 用于多网格图神经网络替代模型的物理信息粗化
authors: "Amir Bazzi, Ramy Nemer, Alves José, Elie Hachem"
date: 2026-04-30
pdf: "https://openreview.net/pdf/84752f8a4390e7825764247d4b44f94feeeff048.pdf"
tags: ["query:physics-ml"]
score: 7.0
evidence: 物理信息粗化策略用于多网格图神经网络PDE替代模型
tldr: 固体力学PDE替代模型面临非线性弹性等挑战。本文提出多网格图神经网络，结合物理信息粗化，通过残差度量保留高活动区域，在变形固体上达到与经典求解器相当的精度且速度快数倍。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 固体力学PDE替代模型在非线性弹性等场景缺乏鲁棒性。
method: 在图神经网络中使用残差度量指导粗化，保留高应力区域。
result: 在变形固体基准上达到高精度，速度提升一个数量级。
conclusion: 物理信息粗化有效提升了固体力学GNN代理的性能。
---

## Abstract
Learning-based surrogates for partial differential equations have recently matched the accuracy of classical solvers while achieving orders-of-magnitude speedups, predominantly in fluid settings and structured geometries. In contrast, robust surrogates for deformable solids remain underexplored, despite the presence of nonlinear elasticity, plasticity, and transient behavior that challenge standard architectures. We introduce a multigrid graph neural network for solid mechanics that couples an *encoder-processor-decoder* backbone with a physics-informed coarsening strategy. Instead of downsampling via geometric heuristics, our method scores nodes using a residual-based measure of local physical activity and preferentially retains regions of high strain or stress concentration, allocating multiscale capacity where it is most needed. This preserves long-range interactions through hierarchical message passing while improving stability over long rollouts. We evaluate on multiple datasets covering linear, nonlinear, and transient regimes, and observe consistent gains in accuracy and rollout stability compared to standard sampling baselines. Our results highlight the importance of physics-informed coarsening for scalable surrogate modeling in solid mechanics.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **问题**：基于学习的偏微分方程（PDE）替代模型在流体问题和结构化几何中已取得接近经典求解器的精度和数量级加速，但在变形固体（deformable solids）领域仍缺乏鲁棒性。固体力学涉及非线性弹性、塑性、瞬态行为，对标准神经网络架构构成挑战。
- **背景**：现有图神经网络（GNN）代理模型在固体力学中常因几何粗化（coarsening）策略忽略物理关键区域（如高应变、应力集中区），导致长时序稳定性差、精度不足。
- **动机**：提出一种物理信息指导的粗化策略，使多尺度GNN代理在固体力学中达到与经典求解器相当的精度，同时保持速度优势。

## 2. 论文提出的方法论

- **核心思想**：将多网格图神经网络（multigrid GNN）与物理信息粗化（physics-informed coarsening）相结合。
- **关键技术细节**：
  - 架构采用 **编码器-处理器-解码器**（encoder-processor-decoder）骨干网络。
  - 粗化阶段不使用传统几何启发式下采样（如均匀抽样或基于距离的聚类），而是利用**基于残差（residual）的局部物理活动度量**为每个节点打分。
  - 优先保留**高应变或高应力集中区域**的节点，从而将多尺度容量（multiscale capacity）分配给最需要的区域。
  - 通过分层消息传递（hierarchical message passing）保持长程相互作用，同时改善长时间滚动（rollout）的稳定性。
- **公式/算法流程**（文字说明）：
  1. 用初始网格构建图，每个节点对应一个离散点。
  2. 对每个节点计算基于残差的物理活性评分（例如，与局部力平衡偏差成正比的量）。
  3. 根据评分确定保留节点集合，生成粗化图（coarser graph）。
  4. 在粗化图上执行消息传递（处理器），并结合细粒度信息进行多尺度融合。
  5. 解码器输出预测的物理场（如位移、应力）。
  6. 在长时间序列中递归应用该过程。

## 3. 实验设计

- **数据集/场景**：多个数据集，覆盖**线性、非线性、瞬态**（transient）三种固体力学机制。
- **基准（benchmark）**：与经典数值求解器（如有限元）对比精度，同时对比**标准采样基线**（standard sampling baselines），即使用几何粗化策略的GNN。
- **对比方法**：标准采样基线（几何启发式下采样）作为主要对照。未提及是否与其他ML替代模型（如PINN、FNO）对比。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量、训练时长等算力细节。仅指出模型在精度和稳定性上优于基线，但未报告计算开销的具体数据。

## 5. 实验数量与充分性

- 实验数量：覆盖**三个不同物理机制**（线性、非线性、瞬态），每个场景应至少有一次主要对比实验；可能包含消融实验（如验证物理信息粗化相对于几何粗化的增益）。
- 充分性与客观性：
  - 在多个机制上验证了方法的一致增益，表明方法具有一定泛化性。
  - 但缺乏与经典ML代理模型（如传统GNN、Transformer、FNO等）的横向对比；基线仅为标准采样，对比不够全面。
  - 未报告统计显著性检验、超参数敏感性分析等。实验结果较为初步，但足以支撑核心论点。

## 6. 论文的主要结论与发现

- 物理信息粗化策略（基于残差的节点保留）显著提升了固体力学GNN替代模型的**准确性**和**滚动稳定性**。
- 所提多网格GNN在变形固体基准上达到与经典求解器**相当的精度**，同时**速度提升一个数量级**。
- 证明了**物理信息融入粗化过程**对可扩展代理建模的重要性，尤其适用于高应力/应变集中的固体力学问题。

## 7. 优点

- **方法新颖**：将物理残差作为粗化指标，克服了几何粗化忽略关键物理区域的缺陷，是一种符合领域知识的自适应多尺度策略。
- **架构设计契合问题**：编码器-处理器-解码器结合分层消息传递，能有效捕获长程相互作用，适合非线性固体力学中的非局部效应。
- **实验覆盖多场景**：线性、非线性、瞬态三种机制均有验证，增强了结论的鲁棒性。
- **实际意义明确**：在保持经典求解器精度的同时实现数量级加速，对工程仿真（如结构分析、碰撞模拟）具有应用价值。

## 8. 不足与局限

- **实验对比不充分**：仅与标准采样基线比较，未与PINN、FNO、其他GNN变体或降阶模型（ROM）对比，难以全面评估方法相对优势。
- **算力报告缺失**：未提供训练/推理的具体硬件、时间、能耗等信息，影响可复现性和效率判断。
- **应用限制**：
  - 当前仅在变形固体基准上验证，未测试流体、多物理场等场景。
  - 残差度量可能依赖于具体PDE形式，扩展至复杂本构模型（如塑性、损伤）需重新设计。
  - 粗化策略对网格质量敏感，在极不规则或动态网格上可能失效。
- **缺乏统计评估**：未给出多次运行的标准差或置信区间，难以判断结果稳定性。
- **数据集规模有限**：未说明数据量及生成方式，可能存在过拟合风险。

（完）
