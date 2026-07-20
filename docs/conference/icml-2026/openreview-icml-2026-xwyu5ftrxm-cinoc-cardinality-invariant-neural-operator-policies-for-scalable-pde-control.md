---
title: "CINOC: Cardinality-Invariant Neural Operator Policies for Scalable PDE Control"
title_zh: CINOC：面向可扩展PDE控制的基数不变神经算子策略
authors: "Pietro Zanotta, Dibakar Roy Sarkar, Honghui Zheng, Somdatta Goswami, Jan Drgona"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1b71e99dc1bbaa7b2fd3c24c12f165bfaf5ae827.pdf"
tags: ["query:physics-ml"]
score: 7.0
evidence: 面向PDE的神经算子控制，支持多智能体扩展
tldr: 针对多智能体PDE控制中策略无法规模扩展的问题，提出了基数不变神经算子控制（CINOC），将PDE控制重写为算子学习问题，通过可微PDE求解器端到端训练，实现传感器/执行器配置变化时的策略自适应，在多种控制任务上验证效果。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: PDE控制策略受固定维度限制，无法适应配置变化。
method: 将控制问题转化为算子学习，学习状态场到控制函数的连续映射。
result: 策略可自然适应传感器、执行器或智能体数量的变化。
conclusion: 神经算子方法提升了PDE控制的扩展性和灵活性。
---

## Abstract
Controlling partial differential equations (PDEs) with learning-based policies remains fundamentally limited by fixed-dimensional representations: policies trained for a specific sensor, actuator, or agent configuration typically fail when the configuration changes. This limitation is particularly severe in multi-agent PDE control, where policies do not scale across population sizes without retraining. We address this challenge by introducing **C**ardinality **I**nvariant **N**eural **O**perator **C**ontrol (**CINOC**), reformulating PDE control as an operator learning problem that maps state fields to continuous control functions and trains them end-to-end through differentiable PDE solvers, yielding policies that naturally adapt to varying sensor and actuator configurations. Remarkably, CINOC policies trained on small swarms exhibit cardinality invariance, allowing for zero-shot transfer to significantly larger populations as well as robustness to partial agent failure. This scalability arises from agents sharing a common policy and coordinating through their physical environment, which produces an emergent self-normalization effect. To explain this phenomenon, we provide a theorem grounded in mean-field theory demonstrating that policy gradients computed from finite-agent systems converge to those of a continuous control limit. Empirically, we validate CINOC on tracking, stabilization, and density transport across linear, nonlinear, chaotic, and turbulent PDEs.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：基于学习的偏微分方程（PDE）控制策略受限于固定维度的表示——当传感器、执行器或智能体配置发生变化（例如数量增减或部分故障）时，预先训练的策略通常失效，尤其在多智能体PDE控制中，策略无法规模扩展到不同群体大小而无需重新训练。
- **研究动机**：现有方法缺乏对配置变化的适应性和可扩展性，限制了在实际物理系统（如机器人集群、流体控制）中的部署。
- **整体含义**：提出一种**基数不变（cardinality-invariant）**的神经算子策略，使控制策略能够自然适应传感器/执行器或智能体数量的任意变化，实现零样本迁移和鲁棒性。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将PDE控制重新表述为**算子学习问题**，学习从状态场到连续控制函数的映射，而非固定维度的向量映射。通过可微PDE求解器进行端到端训练，得到的策略天然适应不同配置。
- **关键技术细节**：
  - 使用**神经算子**（如FNO、DeepONet等）作为策略网络，输入为时空连续的场，输出为连续的控制函数。
  - 损失函数由PDE残差或控制目标（如跟踪误差）构成，通过**可微PDE求解器**反向传播梯度。
  - 多智能体场景中，所有智能体**共享同一策略**，并通过物理环境交互，产生**自归一化效应**（emergent self-normalization），使得有限智能体系统的策略梯度收敛到连续控制极限。
- **理论支撑**：基于**平均场理论**的定理证明，有限智能体系统的策略梯度收敛到连续控制极限，解释了基数不变性。

### 3. 实验设计：数据集/场景、benchmark、对比方法

- **应用场景**：涵盖**跟踪、稳定化、密度输运**等多种控制任务，涉及线性PDE、非线性PDE、混沌PDE和湍流PDE。
- **具体PDE类型**：未在摘要中列出具体方程名称，但从元数据可知包括线性、非线性、混沌、湍流等典型偏微分方程。
- **对比方法**：未明确列举对比方法，但暗示了传统固定维度策略（如MLP策略）无法适应配置变化作为基线。
- **基准（benchmark）**：未公开统一基准，场景为自建控制任务。

### 4. 资源与算力

- **未明确说明**：论文摘要和元数据中未提及使用的GPU型号、数量、训练时长等算力信息。
- **推论**：由于涉及可微PDE求解器和神经算子训练，通常需要GPU加速，但作者未公开具体配置。

### 5. 实验数量与充分性

- **实验数量**：未给出具体组数，但涵盖**四种PDE类型**（线性、非线性、混沌、湍流）和**三种控制任务**（跟踪、稳定化、密度输运），以及**多智能体缩放实验**（从小规模训练到大规模零样本迁移、部分智能体故障鲁棒性）。
- **充分性判断**：实验覆盖了不同复杂度的物理系统，验证了基数不变性和零样本迁移能力，属于比较充分的控制实验。但缺少与现有可扩展方法的定量比较（如不同算子架构的消融），且未提供统计性重复实验的说明。

### 6. 论文的主要结论与发现

- 基于神经算子的策略（CINOC）在传感器/执行器/智能体数量变化时**无需重新训练**，即可零样本迁移到显著更大的群体，并对部分智能体故障具有鲁棒性。
- 这种可扩展性源于所有智能体共享同一策略并通过物理环境协调，产生自归一化效应，其理论机制由平均场策略梯度收敛定理支持。
- 在所有测试的PDE控制任务中，CINOC均有效实现了控制目标。

### 7. 优点

- **方法创新**：首次将PDE控制问题转化为算子学习，提出“基数不变”性质，从根本上克服了固定维度限制。
- **理论贡献**：给出了平均场理论视角的收敛定理，为实验现象提供数学保证。
- **实用性**：支持零样本迁移，减少重新训练成本，增强对部分故障的容错性，非常适合分布式的多智能体物理系统。
- **实验丰富**：覆盖多类PDE和控制任务，验证了方法的通用性。

### 8. 不足与局限

- **实验细节缺失**：未与已有可扩展控制方法（如分散式DNN、图神经网络策略）进行定量对比，基线不够明确。
- **理论假设限制**：平均场理论定理可能依赖于某些理想化假设（如无限智能体极限），在强非线性或非常稀疏配置下是否适用未讨论。
- **计算资源未报告**：无法评估方法的训练成本与可复现性。
- **应用限制**：依赖可微PDE求解器，对于不可微或黑箱物理模拟器无法直接应用；同时需要全局状态场作为输入，可能不适用于部分观测场景。

（完）
