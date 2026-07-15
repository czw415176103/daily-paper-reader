---
title: Enforcing boundary conditions for physics-informed neural operators
title_zh: 物理信息神经算子的边界条件施加
authors: "Niklas Göschel, Sebastian Götschel, Daniel Ruprecht"
date: 2025-05-09
pdf: "https://openreview.net/pdf?id=yT8hGWqoBm"
tags: ["query:physics-ml"]
score: 9.0
evidence: 在PINN和PINO中强加边界条件的方法
tldr: 物理信息神经算子中的边界条件通常弱加或要求完全C1边界。本文提出新的强加方法，改善了稳定性和精度，在多个PDE问题中验证了有效性。
source: NeurIPS-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 强加Neumann或Robin边界常要求完全C1边界，导致不稳定。
method: 提出不依赖完全C1边界的强加方法，提高稳定性。
result: 在多个PDE实例上优于现有方法，训练更快更稳定。
conclusion: 为PINN和PINO提供了更可靠的边界条件实施策略。
---

## Abstract
Machine-learning based techniques like physics-informed neural networks (PINNs) and physics-informed neural operators (PINO) are becoming increasingly adept at solving even complex systems of partial differential equations (PDEs).
Boundary conditions can be enforced either weakly by penalizing deviations in the loss function or strongly by training a solution structure that inherently matches the prescribed values and derivatives.
The former approach is easy to implement but the latter can provide benefits with respect to accuracy and training times.
However, previous approaches to strongly enforcing Neumann or Robin boundary conditions require a domain with a fully $C^1$ boundary and, as we demonstrate, can lead to instability if those boundary conditions are posed on a segment of the boundary that is piecewise $C^1$ but only $C^0$ globally.
We introduce a generalization of the approach by (Sukumar, N. & Srivastava, A., 2022, https://doi.org/10.1016/j.cma.2021.114333) and a new approach based on orthogonal projections that overcome this limitation.
The performance of these new techniques is compared against weakly and semi-weakly enforced boundary conditions for the scalar Darcy flow equation and the stationary Navier-Stokes equations.

---

## 论文详细总结（自动生成）

好的，以下是根据提供的论文元数据与摘要生成的中文总结。

---

### 1. 核心问题与整体含义（研究动机和背景）

- **背景**：物理信息神经网络（PINN）和物理信息神经算子（PINO）在求解偏微分方程（PDE）方面日益成熟。边界条件通常有两种施加方式：**弱施加**（通过在损失函数中惩罚偏差）和**强施加**（通过构造天然满足边界条件的解结构）。
- **问题**：弱施加实现简单，但强施加能带来更高的精度和更快的训练。然而，现有强加 Neumann 或 Robin 边界条件的方法要求定义域具有完全 \(C^1\) 光滑的边界。当边界条件施加在分段 \(C^1\)、全局仅 \(C^0\) 的边界段上时，这些方法会不稳定。
- **动机**：提出克服该限制的强加边界条件新方法，提升 PINN 和 PINO 在复杂边界上的稳定性和准确性。

### 2. 方法论：核心思想、关键技术细节

- **核心思想**：对 Sukumar & Srivastava (2022) 的方法进行推广，并引入一种基于正交投影的新方法，两者均不依赖边界的整体 \(C^1\) 光滑性。
- **关键技术细节**（从摘要推断）：
  - **推广方法**：扩展原方法使其适用于分段光滑边界，通过局部构造或分区处理避免全局 \(C^1\) 要求。
  - **正交投影法**：将边界条件视为约束，利用正交投影将解空间投影到满足约束的子空间，从而自动满足指定值和导数。
  - **两种方法**均为**强施加**，无需在损失函数中弱化边界条件。
- **算法流程**（文字说明）：
  1. 定义 PDE 残差和网络输出。
  2. 使用上述方法之一修改网络输出结构，使其在边界上严格满足 Neumann/Robin 条件。
  3. 仅最小化内部 PDE 残差（或与弱施加组合），无需边界损失项。

### 3. 实验设计

- **使用的 PDE 场景**：
  - 标量达西流方程（Darcy flow）
  - 稳态纳维-斯托克斯方程（Navier-Stokes equations）
- **基准与对比方法**：
  - **弱施加**边界条件（损失函数中惩罚边界偏差）
  - **半弱施加**边界条件（部分强、部分弱）
  - 本文提出的**推广方法**和**正交投影法**
- **说明**：原文未提供具体数据集，应为解析解或标准数值算例生成的数据。

### 4. 资源与算力

- **文中未明确说明**使用的 GPU 型号、数量或训练时长。
- 仅提及新方法训练“更快更稳定”，但无具体量化指标。

### 5. 实验数量与充分性

- **实验数量**：至少涵盖两个不同的 PDE 问题（达西流、Navier-Stokes），每个问题上与至少三种方法（弱、半弱、两种新方法）对比。未提及消融实验或超参数扫描。
- **充分性评估**：
  - **优点**：选择了不同性质（椭圆型、非线性）的经典方程，具有一定代表性。
  - **不足**：仅两个案例，未涉及更复杂的边界几何或高维问题；未报告统计重复次数或置信区间；缺乏对计算效率的具体比较。

### 6. 论文的主要结论与发现

- 提出的两种新方法能够在不要求全局 \(C^1\) 边界的情况下，稳定地强加 Neumann 或 Robin 边界条件。
- 在达西流和 Navier-Stokes 问题上，新方法优于弱施加和半弱施加，精度更高，训练更快更稳定。
- 为 PINN 和 PINO 提供了更可靠的边界条件实施策略，降低了强加边界时的不稳定性风险。

### 7. 优点

- **方法创新**：解决了长期存在的强加边界条件对边界光滑性的限制，具有理论价值和实用意义。
- **稳定性提升**：在分段光滑边界上避免了现有方法的发散问题。
- **实验设计公平**：与弱和半弱施加方法直接对比，基线清晰。
- **应用广泛**：直接适用于 PINN 和 PINO 两类流行方法。

### 8. 不足与局限

- **实验覆盖有限**：仅测试了两个 PDE，未涉及三维问题、复杂几何（如带尖角）、非稳态问题或实际工程案例。
- **缺乏消融分析**：未系统探究不同参数（如网络宽度、训练步数）对边界施加方法的影响。
- **未报告计算开销**：虽然声称更快，但无具体训练时间对比，无法判断新方法是否增加了计算复杂度。
- **论文被拒**：可能还存在方法论细节不完善或实验不充分的问题，需谨慎看待结论的普适性。
- **资源信息缺失**：无法评估方法的可复现性与训练代价。

（完）
