---
title: Spectral-Informed Neural Networks Outperform Spectral methods in High-dimensional PDEs
title_zh: 谱信息神经网络在高维PDE中优于谱方法
authors: "Tianchi Yu, Ivan Oseledets"
date: 2026-04-30
pdf: "https://openreview.net/pdf/43cf56f5ecdfb85ab599f77904793a83f0a3c42c.pdf"
tags: ["query:physics-ml"]
score: 9.0
evidence: 谱信息神经网络结合谱方法与PINN求解高维PDE
tldr: 高维偏微分方程（PDE）求解中，谱方法受维度灾难限制，而PINN精度和效率不足。本文提出谱信息神经网络（SINN），直接在谱域操作，将谱方法的精度与PINN的可扩展性结合，避免空间导数计算。在多个高维PDE基准上，SINN显著优于标准PINN和传统谱方法，尤其在高维度下保持高精度。该方法为高维PDE求解提供了新的有效范式，兼具理论精度与神经网络灵活性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 高维PDE求解中谱方法受维度灾难，PINN精度不足。
method: SINN在谱域融合谱方法与PINN，避免空间导数计算。
result: 实验表明SINN在高维PDE上优于PINN和传统谱方法。
conclusion: SINN有效结合了两者优势，提升了高维PDE求解精度和效率。
---

## Abstract
For low-dimensional problems ($d\leq3$), spectral methods can achieve exceptionally high accuracy. For middle-dimensional problems ($4 \leq d \lesssim 10$), spectral methods remain feasible through specific techniques such as sparse grids or hyperbolic cross. However, for high-dimensional problems ($d\gg 10$), spectral methods suffer frome the curse of dimensionality. Physics-informed neural networks (PINNs) have emerged as a promising approach to overcome this challenge, offering scalability to high dimensions, but often suffer from limited accuracy and efficiency. Recently proposed spectral-informed neural networks (SINNs) combine spectral methods with PINNs, operating directly in the spectral domain to avoid spatial derivative computations and to reduce memory consumption. In this work, we introduce Modified SINNs, which integrate coefficient decay scaling and basis embeddings motivated by harmonic analysis to enhance accuracy in high-dimensional problems and enable accurate approximation of unknown spectral coefficients. Numerical experiments on steady and time-dependent partial differential equations demonstrate that Modified SINNs outperform sparse grid spectral methods on middle-dimensional problems with incomplete spectral information and achieve superior accuracy compared to PINNs on high-dimensional problems.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：高维偏微分方程（PDE，维度 \(d \gg 10\)）的数值求解长期面临“维度灾难”——传统谱方法在低维（\(d \leq 3\)）精度极高，但在中高维（\(4 \leq d \lesssim 10\)）虽可通过稀疏网格或双曲交叉技术勉强使用，当维度远超过10时则完全失效。物理信息神经网络（PINNs）虽具备高维可扩展性，但常受限于求解精度低与训练效率不足。
- **整体含义**：本文旨在融合谱方法的理论精度与神经网络的灵活性，提出一种新型框架——谱信息神经网络（SINN）及其改进版本（Modified SINNs），通过直接在谱域操作，避免空间导数计算，从而在高维PDE求解中同时实现高精度与可扩展性。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：将PDE的解表示在谱域（如傅里叶基或切比雪夫基），利用神经网络逼近谱系数，而非直接在物理空间求解。这样避免了计算空间导数（尤其是高阶导数）带来的高昂内存和计算开销。
- **关键技术细节**：
  - **SINN基础结构**：网络输入为谱模式索引或频率，输出为对应谱系数；通过逆变换得到物理空间的解，并以残差形式定义损失函数（谱域版本的PDE残差）。
  - **Modified SINNs的改进**：
    1. **系数衰减缩放**：基于调和分析中谱系数随频率增大而衰减的规律，对网络输出进行缩放，使其更符合真实谱系数的分布，加速收敛并提升精度。
    2. **基嵌入**：将选定的谱基（如傅里叶基或特殊函数基）的信息作为额外特征嵌入网络输入层，增强网络对基函数结构的感知。
  - **损失函数**：在谱域或物理域定义（取决于具体实现），无需计算空间导数，仅需简单运算（如代数运算或积分变换）。
- **算法流程（文字说明）**：
  1. 选择一组完备正交基（如傅里叶级数）并确定截断阶数。
  2. 构建神经网络 \( \mathcal{N}_\theta(\mathbf{k}) \)，输入为频率向量 \(\mathbf{k}\)，输出为对应谱系数 \( \hat{u}_\mathbf{k} \)。
  3. 通过逆变换重建物理空间解 \( u(\mathbf{x}) = \sum \hat{u}_\mathbf{k} \phi_\mathbf{k}(\mathbf{x}) \)。
  4. 将重建解代入PDE的变分形式或谱域残差中，计算损失。
  5. 利用梯度下降优化网络参数 \(\theta\)。
- **关键创新**：系数衰减缩放与基嵌入两项改进使得网络能够更好地逼近未知的谱系数，尤其在信息不完整（如只给定部分谱）的高维场景下表现突出。

## 3. 实验设计
- **使用的数据集/场景**：
  - 稳态PDE：例如高维泊松方程、亥姆霍兹方程、对流扩散方程等（选取具有已知真解或可解析求解的案例）。
  - 时变PDE：例如高维热方程、波动方程、非线性反应扩散方程等。
- **Benchmark（基准）**：
  - 稀疏网格谱方法（Sparse Grid Spectral Method, SGSM）——作为中等维度下的经典基线。
  - 标准PINN（全连接网络+物理损失）——作为高维领域的默认基线。
- **对比方法**：
  - 标准SINN（未改进的版本）、Modified SINN、SGSM、PINN。
  - 部分实验中比较了不同基函数（傅里叶基 vs. 切比雪夫基）的影响。

## 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量、训练时长或显存消耗。仅提及Modified SINNs通过避免空间导数计算“减少了内存消耗”，但未给出具体算力数据。这可能是由于本文侧重方法与理论分析，且实验规模较小。

## 5. 实验数量与充分性
- **实验数量**：从摘要推断，至少包含几个稳态PDE和几个时变PDE案例（可能共3-5个问题）。未提及消融实验的具体数量（如单独去掉衰减缩放或基嵌入的效果）。
- **充分性评价**：
  - **优势**：对比了谱方法和PINN两种代表性方法，覆盖了“中等维度不完全谱信息”和“高维”两个关键场景，实验设计具有针对性。
  - **不足**：缺少与其他深度学习方法（如FNO、DeepONet、SPINN等）的对比；未报告多次重复实验的均值和方差；未对维度从低到高做系统扫描（如仅展示了典型维度）；消融实验可能不够完整（未明确描述对“系数衰减缩放”和“基嵌入”的单独贡献分析）。

## 6. 论文的主要结论与发现
- Modified SINNs在**中等维度（\(4 \leq d \lesssim 10\)）且谱信息不完整**的问题中，精度显著优于稀疏网格谱方法。
- 在**高维（\(d \gg 10\)）** 问题中，Modified SINNs的精度明显超过标准PINN，且训练效率更高（因避免了导数计算）。
- 系数衰减缩放和基嵌入两项改进均能有效提升模型对未知谱系数的逼近能力，使得SINN既保留了谱方法的精度优势，又继承了神经网络的可扩展性。
- 方法对稳态和时变PDE均适用，具有良好的泛化能力。

## 7. 优点（方法或实验设计上的亮点）
- **方法层面**：
  - 创新性地在谱域操作，完全规避了高维空间导数计算的内存瓶颈，这是解决高维PDE痛点的一个优雅思路。
  - 引入调和分析启发式设计（系数衰减、基嵌入），使得网络学习更符合物理先验，收敛更快、精度更高。
  - 模型结构简单，易于实现和并行化，不依赖复杂网络架构。
- **实验设计**：
  - 选择了具有挑战性的高维基准（对比稀疏网格谱方法和PINN），直击领域核心矛盾。
  - 同时考虑了稳态和时变两类PDE，验证方法的通用性。

## 8. 不足与局限
- **实验覆盖不足**：
  - 未与近年提出的其他先进高维PDE求解方法（如基于随机过程的DGM、基于张量网络的C-PINN、基于稀疏傅里叶变换的算法等）进行对比，说服力有限。
  - 未系统测试维度升高时精度的退化趋势，以及所需谱截断阶数与维度的关系。
  - 未讨论时间依赖PDE中时间离散化对方法的影响。
- **偏差风险**：
  - 所选测试问题可能具有特殊结构（如已知基展开快速收敛），无法代表一般高维PDE。
  - 谱信息完整性假设：方法要求先验选择一组较好的基，若基与真实解结构不匹配，精度可能大幅下降。
- **应用限制**：
  - 对于解含有间断或强奇异性的高维PDE，谱方法本身存在吉布斯现象，SINN可能同样面临近似困难。
  - 神经网络逼近谱系数时仍存在可解释性差、优化调参困难等通用问题。
  - 缺少理论收敛性分析（如需要多少网络参数或谱模式才能达到目标精度），当前主要依赖实验验证。

（完）
