---
title: Physics-informed machine learning with domain decomposition and global dynamics for three-dimensional intersecting flows
title_zh: 物理信息机器学习结合领域分解和全局动力学用于三维交叉流
authors: Leslie K Hwang
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=rOuRpOA6pm"
tags: ["query:physics-ml"]
score: 9.0
evidence: 多域PINN用于三维交叉流，采用领域分解
tldr: 针对物理信息神经网络（PINN）在三维非矩形几何和复杂流体动力学中可扩展性和精度下降的问题，提出了多域PINN（MDPINN）框架。该框架将目标域分解为子域，并引入全局动力学耦合，有效处理了高纵横比交叉流道中的非线性流动。实验表明，MDPINN在三维交叉流模拟中显著优于标准PINN，为实际工程中的复杂几何流体问题提供了可行方案。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: PINN在三维非矩形几何中可扩展性和精度不足，难以应用于实际系统。
method: 提出多域PINN，包含领域分解和全局动力学耦合以处理复杂几何流动。
result: 在三维交叉流模拟中，MDPINN精度和收敛性大幅优于标准PINN。
conclusion: 多域分解有效提升了PINN在复杂几何流体问题中的实用性。
---

## Abstract
Physics-informed neural networks (PINNs) have emerged as a promising framework to develop complex scientific surrogate models, yet their scalability and accuracy often degrade in non-canonical geometries, such as non-rectangular domains or three-dimensional (3D) domains with high aspect ratios. These limitations hinder the broader adoption of vanilla PINNs in real-world, practical systems. In this work, we introduce a multi-domain PINN (MDPINN) framework designed to address the scalability and generalization challenges inherent in 3D non-rectangular domains governed by nonlinear fluid dynamics. The target domain consists of intersecting 3D fluid channels with a high aspect ratio, inducing complex flow features such as deflections, mixing, and recirculations. Our approach is grounded in two key innovations: 1) domain decomposition, which partitions the channel volumes into multiple cubic-like subdomains, each modeled by an individual PINN, 2) enforcement of global dynamics (MDPINN-GD), which ensures that the total mass flow rate entering the domain equals that exiting. These innovations reduce the complexity of the problem imposed on individual PINNs and guide effective network optimization toward physically consistent solutions throughout the domain. We demonstrate that our method achieves: 1) 74.8\% accuracy improvement over a single-network PINN, and 2) 52.9\% accuracy improvement over MDPINN that do not enforce global mass conservation. Furthermore, the MDPINN-GD framework exhibits accurate prediction even in highly complex regions-such as the channel intersecting zone and the outlet zone characterized by intense flow mixing and large velocity gradients-achieving maximum normalized mean absolute errors below 14.9\% for velocity predictions compared to simulation results. This work establishes a path towards scalable, physically grounded surrogate modeling approach that is extensible to multiphysics and high-dimensional scientific problems.

---

## 论文详细总结（自动生成）

# 论文《Physics-informed machine learning with domain decomposition and global dynamics for three-dimensional intersecting flows》详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：物理信息神经网络（PINN）在非标准几何（如非矩形区域或高纵横比三维域）中的可扩展性和精度会显著下降，限制了其在真实复杂工程系统（如三维交叉流道）中的应用。
- **研究动机**：三维交叉流道（例如流道相交、高纵横比区域）中会产生偏转、混合、回流等复杂流动特征，标准单网络PINN难以准确模拟此类非线性流体动力学问题。
- **整体含义**：论文旨在通过领域分解和全局动力学约束，提升PINN在复杂几何流体问题中的实用性和可扩展性，为多物理场和高维科学问题提供替代建模路径。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：将整体目标域分解为多个子域，每个子域独立由一个小型PINN建模，同时引入全局动力学耦合（即质量流率守恒），从而降低单网络复杂度，并引导优化朝向物理一致解。
- **关键技术细节**：
  - **多域PINN（MDPINN）框架**：将三维交叉流道划分为多个近似立方体的子域，每个子域分配一个独立的PINN网络。
  - **全局动力学（MDPINN-GD）**：额外强制施加总入口质量流率等于总出口质量流率的约束，作为软约束或硬约束加入损失函数。
  - **没有给出具体公式**（因为只有摘要），但可以推断损失函数包含：Navier-Stokes方程残差、边界条件、子域间连续性（如速度/压力匹配）以及全局质量守恒项。
- **算法流程（文字说明）**：
  1. 将复杂三维几何（交叉流道）人工分解为多个简单立方体状子域。
  2. 为每个子域初始化一个单独的PINN网络（权重独立）。
  3. 损失函数包含各子域的NS残差、边界条件、子域间界面连续性（可能通过重叠或交界点匹配）以及全局质量流率守恒项。
  4. 使用自动微分和优化器同时训练所有子网络，或者交替训练。
  5. 最终通过组合各子域输出获得全局解。

## 3. 实验设计：使用数据集/场景、benchmark、对比方法

- **场景**：三维交叉流体通道（高纵横比、相交区域），包含偏转、混合、回流等复杂流动特征。没有指定的数据集，而是基于数值模拟（如CFD）生成的参考解。
- **基准（benchmark）**：与高精度CFD模拟结果对比（计算归一化平均绝对误差）。
- **对比方法**：
  - 标准单网络PINN（vanilla PINN）
  - 未施加全局质量守恒的多域PINN（MDPINN without GD）
  - 提出的MDPINN-GD（完整方法）
- **结果**：
  - MDPINN-GD相比标准PINN精度提升74.8%。
  - MDPINN-GD相比未加全局守恒的MDPINN精度提升52.9%。
  - 在复杂区域（交叉区、出口区）速度预测的最大归一化平均绝对误差低于14.9%。

## 4. 资源与算力

- **论文未明确说明**使用的GPU型号、数量或训练时长。仅能根据常识推断：训练多个子域PINN可能需要GPU加速，但具体细节缺失。

## 5. 实验数量与充分性

- **实验数量**：至少进行了三组对比实验：标准PINN、无全局守恒MDPINN、完整MDPINN-GD。此外，还可能包含不同网格划分策略的消融实验（摘要未详细列出）。
- **充分性评估**：仅提供了一组成果指标（如精度提升百分比），但缺乏多组超参数调优、不同几何复杂度、不同流动Re数等扩展实验。从摘要看，实验覆盖有限，未讨论对高Re数或湍流的适用性。对比方法仅包含两个基线，缺少与其他领域分解方法（如XPINN、cPINN）的对比。因此，实验设计在对比全面性和泛化性方面不够充分，但重点验证了核心创新点的有效性。

## 6. 论文的主要结论与发现

- 多域分解（MDPINN）能显著提升PINN在三维非矩形几何中的精度和收敛性。
- 施加全局质量守恒约束（MDPINN-GD）进一步提升了物理一致性，尤其在高复杂区域（交叉区、出口区）表现优异。
- 该方法可扩展至多物理场和高维科学问题，为复杂几何流体仿真提供了一种可扩展的代理建模途径。

## 7. 优点：方法或实验设计上的亮点

- **创新性**：将领域分解与全局动力学约束结合，有效缓解了非矩形几何下PINN的可扩展性问题。
- **效果显著**：在三维交叉流中实现接近15%以内的误差，且相比标准PINN提升74.8%，数值改进大。
- **物理可解释性**：全局质量守恒是基本物理定律，强制约束有助于网络学习合理流动。
- **实用性**：针对真实工程场景（交叉流道），具有潜在工业应用价值。

## 8. 不足与局限

- **实验覆盖不全**：仅使用单一几何构型（交叉流道），未验证其他复杂几何（如弯曲管道、多入口出口）或不同流动条件（如湍流、非稳态）。
- **缺乏对比**：未与其他领域分解PINN变体（如XPINN、cPINN、hp-VPINN）对比，无法确立本方法的相对优势。
- **算力和训练细节缺失**：未报告GPU资源、训练时间、网络架构大小等，不利于复现和评估实际成本。
- **可能偏差风险**：结果仅基于某一数值模拟数据，未与实验测量数据对比，其泛化至真实物理系统有待验证。
- **应用限制**：方法依赖人工领域分解，对于极端复杂几何可能需要大量手动干预，可扩展性仍有待改善。

（完）
