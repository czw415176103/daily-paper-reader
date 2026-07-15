---
title: Neural Superposition Networks
title_zh: 神经网络叠加网络
authors: "Atiyo Ghosh, Nicolò Toscano, Jongyeong Lee, Hyukgeun Cha, Jun-Ho Lee, Jung Jun Park, Yunjun Choi, Seong-Hyok Sean Kim, Antonio Andrea Gentile"
date: 2025-05-10
pdf: "https://openreview.net/pdf?id=aWXrVm07Zl"
tags: ["query:physics-ml"]
score: 9.0
evidence: 通过基函数叠加将PDE解流形嵌入网络架构
tldr: 针对PINN依赖损失正则化且训练不稳定的问题，Neural Superposition Networks通过将输出表达为求解目标PDE的解析基函数的叠加，从架构上强制满足PDE约束，无需内部残差损失项。在拉普拉斯、热传导等线性PDE上，该方法训练更稳定且收敛有保证，为物理约束神经网络提供了新范式。
source: NeurIPS-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 传统PINN通过损失正则化施加PDE约束，导致训练复杂且数值不稳定。
method: 提出Neural Superposition Networks，网络输出为求解线性PDE的解析基函数的可学习叠加。
result: 在多个线性PDE基准上，该架构无需内点残差项即可精确满足PDE，训练稳定且收敛快。
conclusion: 通过架构设计内在满足PDE约束为物理信息学习提供了新思路。
---

## Abstract
We introduce _Neural Superposition Networks_, a class of physics-constrained neural architectures that exactly satisfy given partial differential equations (PDEs) by construction. In contrast to traditional physics-informed neural networks (PINNs), which enforce PDE constraints via loss regularization, our approach embeds the solution manifold directly into the architecture by expressing the output as a superposition of analytical basis functions that solve the target PDE. This eliminates the need for interior residual loss terms, simplifies training to a single-objective optimization on boundary conditions, and improves numerical stability. 
We show that for linear PDEs—including Laplace, heat, and incompressible flow constraints—this architectural bias leads to provably convergent approximations. Using maximum principles and classical convergence theory, we establish uniform boundary-to-interior convergence guarantees. For nonlinear PDEs such as Burgers’ equation, we demonstrate that partial structural constraints can still be enforced via transformations (e.g., Cole–Hopf), yielding improved inductive bias over standard PINNs. The resulting networks combine the expressiveness of deep learning with the convergence guarantees of Galerkin and spectral methods. Our framework offers a theoretically grounded and computationally efficient alternative to residual-based training for PDE-constrained problems..

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **传统PINN的局限**：物理信息神经网络（PINNs）通过损失函数中的残差项来正则化PDE约束，导致训练复杂、数值不稳定，且需要精细调节超参数。
- **核心需求**：如何在神经网络架构中内在强制满足PDE约束，避免依赖损失正则化，同时保持收敛性和表达能力。
- **整体含义**：提出一种新的物理约束神经网络范式——Neural Superposition Networks，将求解PDE的解析基函数叠加嵌入网络输出层，从架构上保证PDE等式精确成立，从而简化训练目标（仅需边界条件损失），提高稳定性和收敛保证。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：将网络输出表示为目标线性PDE的解析基函数的可学习线性叠加，使得网络输出自动满足PDE，无需内部残差损失项。
- **关键技术细节**：
  - 对于线性PDE（如Laplace方程、热传导方程、不可压缩流动约束），利用最大原理和经典收敛理论，证明边界到内部的均匀收敛保证。
  - 对于非线性PDE（如Burgers方程），通过变换（如Cole-Hopf变换）将非线性问题转化为线性问题，再应用基函数叠加，从而部分施加结构约束。
  - 训练过程简化为仅优化边界条件上的单一目标函数（例如边界损失）。
  - 结合了深度学习的表达能力与Galerkin/谱方法的收敛性保证。
- **算法流程**（文字说明）：  
  ① 根据目标PDE的线性算子，选取一组解析基函数（如特征函数、调和函数等）；  
  ② 神经网络输出层将这组基函数通过可学习的权重进行加权叠加；  
  ③ 仅使用边界条件数据（或初值）计算损失，反向传播更新权重；  
  ④ 由于叠加本身满足PDE，无需在内部点施加残差损失。

## 3. 实验设计：使用的数据集/场景、benchmark、对比方法
- **数据集/场景**：摘要提及的线性PDE基准包括Laplace方程、热传导方程、不可压缩流动约束；非线性PDE基准为Burgers方程（通过Cole-Hopf变换处理）。
- **Benchmark**：未明确列出具体数值数据集或标准测试集，但应包含常见PDE验证案例（如热传导板、泊松方程等）。
- **对比方法**：主要与传统PINNs（基于损失正则化）进行比较，评估训练稳定性、收敛速度、近似精度。

## 4. 资源与算力
- **文中未明确说明**：摘要及元数据均未提及使用的GPU型号、数量、训练时长等算力信息。因此无法总结具体资源消耗。

## 5. 实验数量与充分性
- **实验覆盖**：至少涵盖了3种线性PDE和1种非线性PDE（Burgers方程），但未说明每个问题的具体实验次数、参数变化或消融实验。
- **充分性评价**：由于仅从摘要获知，缺乏对实验设计的详细描述（如收敛曲线、误差量化、超参数调优、多维度对比等），无法判断实验是否充分、公平。该论文为NeurIPS拒稿，可能因实验不够全面或对比不够详实。

## 6. 论文的主要结论与发现
- **主要结论**：Neural Superposition Networks通过架构设计内在满足PDE约束，在多个线性PDE上训练更稳定、收敛更快，且无需内部残差项，提供了比传统PINN更高效、理论更牢固的替代方案。
- **发现**：对于非线性PDE，通过适当的变换仍可部分利用该思想，获得优于标准PINN的归纳偏置。

## 7. 优点：方法或实验设计上的亮点
- **架构设计的创新性**：将解析基函数叠加作为网络输出，从数学上保证PDE成立，避免了PINN的损失权衡问题。
- **训练简化**：单目标优化（仅边界损失），无需调整内点残差权重，训练更稳定。
- **理论保证**：对线性PDE提供边界到内部的均匀收敛性证明，利用最大原理和经典收敛理论，具有Galerkin/谱方法类似的收敛保障。
- **扩展性**：通过变换可处理某些非线性PDE，增强了通用性。

## 8. 不足与局限
- **适用范围受限**：目前主要针对线性PDE或可通过变换线性化的非线性PDE，一般非线性PDE（如复杂Navier-Stokes）可能无法直接应用。
- **实验细节缺失**：未提供具体数据集、误差指标、与其他方法（如PINNs、Deep Galerkin方法）的定量比较表格，难以评估实际改进程度。
- **算力与效率报告不足**：未说明训练时间、模型参数规模或计算成本，无法判断是否比PINN更经济。
- **非线性处理依赖特定变换**：对Burgers方程使用Cole-Hopf变换虽有效，但并非所有非线性PDE都有解析变换，限制了该方法在一般非线性问题上的直接推广。
- **存在拒稿风险**：被NeurIPS-2025拒绝，可能意味着实验充分性或新颖性尚未达到顶会标准。

（完）
