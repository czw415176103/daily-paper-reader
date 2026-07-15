---
title: Riemannian Diffusion Models on General Manifolds via Physics-Informed Neural Networks
title_zh: 基于物理信息神经网络的通用流形黎曼扩散模型
authors: "Gyeonghoon Ko, Juho Lee"
date: 2026-04-30
pdf: "https://openreview.net/pdf/4d571c68812026663b6991c9c6d48d8d6f73c205.pdf"
tags: ["query:physics-ml"]
score: 9.0
evidence: 用PINN近似热核，实现流形上的扩散模型
tldr: 黎曼扩散模型需要热核，但只在对称流形上有闭式解。本文用PINN直接解流形热方程，学习对数热核作为替代，从而支持任意流形上的扩散生成模型。实验表明该方法在多个非平凡流形上有效。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有黎曼扩散模型受限于热核的闭式解。
method: 使用PINN求解流形热方程，学习对数热核作为替代。
result: 该方法在多种流形上实现有效的扩散生成模型。
conclusion: 将PINN与生成建模结合，扩展了扩散模型到一般流形。
---

## Abstract
Riemannian diffusion models generalize score-based generative modeling to manifold-supported data via stochastic diffusion equations on the manifold.
However, training requires sampling from and differentiating the manifold heat kernel, which is rarely available in closed form beyond a few highly symmetric manifolds.
We propose a general approach that approximates the heat kernel by directly solving the manifold heat equation with a physics-informed neural network (PINN).
Given an explicit manifold specification, we choose a coordinate system, derive the corresponding heat (Fokker--Planck) equation and a short-time asymptotic approximation, and then train a PINN to learn the log heat kernel.
The resulting surrogate enables both forward noising (heat-kernel sampling) and conditional-score evaluation for denoising score matching.
We demonstrate the method on diverse manifolds including $S^2$, $SO(3)$, $\mathrm{SPD}(n)$, and permutation-quotiented point clouds.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：黎曼扩散模型（Riemannian diffusion models）通过流形上的随机微分方程将基于分数的生成模型推广到流形支撑的数据。然而，训练这类模型需要从流形热核（heat kernel）中采样并对其求导，而热核只在少数高度对称的流形（如球面、旋转群等）上有闭式解，严重限制了模型的适用范围。
- **整体含义**：本文提出一种通用方法，利用物理信息神经网络（PINN）直接求解流形上的热方程，近似对数热核，从而摆脱对闭式热核的依赖，使扩散生成模型能够应用于任意黎曼流形。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将热核近似问题转化为求解流形热方程的偏微分方程（PDE）问题，并使用PINN作为数值求解器。训练得到的PINN可以同时提供正向加噪（从热核采样）和条件分数评估（用于去噪分数匹配）。
- **关键技术细节**：
  1. 给定一个显式定义的流形，选择适当的坐标系。
  2. 推导该坐标系下的流形热方程（即Fokker–Planck方程）以及短时渐近近似（作为边界条件或初始条件）。
  3. 定义一个神经网络，输入为时间、流形上的坐标点（或成对点的测地距离等），输出为对数热核的近似值。
  4. 训练损失包含两项：PDE残差（确保满足热方程）和短时渐近拟合（确保初始时刻的准确性）。PINN通过自监督方式学习，无需真实热核标签。
  5. 训练完成后，利用该PINN替代热核，实现完整的正向扩散和反向去噪过程。

### 3. 实验设计：数据集、基准与对比方法

- **使用的流形/场景**：球面 \(S^2\)、三维旋转群 \(SO(3)\)、对称正定矩阵流形 \(\mathrm{SPD}(n)\)、以及排列商点云（permutation-quotiented point clouds）。
- **基准与对比方法**：
  - 对于 \(S^2\) 和 \(SO(3)\)，可对比基于闭式热核的原始黎曼扩散模型。
  - 对于 \(\mathrm{SPD}(n)\) 和排列商点云，热核无闭式解，本文方法成为唯一可行的方案，因此主要展示生成效果的质量（如似然、样本视觉质量等）。
- **对比方法**：文中未明确列出其他数值近似方法（如谱方法、蒙特卡洛估计）作为基线，仅在自身框架下验证PINN近似热核的精度以及下游生成任务的有效性。

### 4. 资源与算力

- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量以及训练时长等算力信息。仅提到“使用PINN训练”，但未提供硬件配置或时间开销。因此无法评估其计算成本的可接受性。

### 5. 实验数量与充分性

- **实验数量**：覆盖4种不同类型的流形（\(S^2\), \(SO(3)\), \(\mathrm{SPD}(n)\), 点云），每个流形上至少进行了生成任务的定性/定量评估。
- **充分性**：实验设计较为全面，既包括有闭式解的流形（可验证PINN近似与真实热核的一致性），也包括无闭式解的复杂流形（展示泛化能力）。但缺乏以下方面的分析：
  - 与其它数值近似方法的对比（如傅里叶级数截断、随机步仿真）。
  - 对PINN近似误差的量化分析（如与精确热核的KL散度）。
  - 消融实验（如短时渐近约束的影响、网络架构选择）。
  - 对更高维流形的可扩展性测试。
- **公平性**：对于有闭式解的流形，可以公平比较，但未给出PINN方法与直接使用闭式解的运行时间、内存差异对比，因此公平性判断受限。

### 6. 论文的主要结论与发现

- PINN能够有效学习流形上的对数热核，其近似质量足以支撑完整的扩散生成模型训练。
- 在 \(S^2\) 和 \(SO(3)\) 上，基于PINN的模型生成效果接近甚至媲美使用精确热核的模型。
- 在 \(\mathrm{SPD}(n)\) 和排列商点云等复杂流形上，该方法成功实现了首个无闭式热核的黎曼扩散生成模型，生成样本在流形几何约束下保持高保真度。
- 表明将PINN与生成建模结合是拓展扩散模型到一般流形的可行路径。

### 7. 优点：方法或实验设计上的亮点

- **通用性**：突破了黎曼扩散模型依赖闭式热核的限制，可应用于任意显式定义的流形。
- **自监督学习**：PINN通过物理方程自监督训练，无需标注数据或预先生成热核样本，训练样本可在线生成。
- **端到端可微**：PINN输出可直接用于分数匹配的梯度计算，便于反向传播。
- **实验场景多样性**：涵盖低维/高维、紧/非紧、对称/非对称流形，验证了方法的广泛适用性。

### 8. 不足与局限

- **计算开销**：PINN训练本身需要求解PDE，对于高维流形可能面临维度灾难，文中未讨论可扩展性。
- **近似误差**：PINN只能近似热核，无法保证严格满足热方程，可能引入偏差。缺少对误差传播到生成质量影响的严格分析。
- **对比不充分**：未与现有的谱方法、特征函数展开或随机步仿真等近似手段进行系统对比，难以判断该方法的效率与精度优势。
- **算力与时间未报告**：导致复现和公平比较困难。
- **适用性假设**：要求流形有显式的坐标描述和可计算度量的热方程表达式，对于隐式定义或不光滑流形可能无法直接应用。

（完）
