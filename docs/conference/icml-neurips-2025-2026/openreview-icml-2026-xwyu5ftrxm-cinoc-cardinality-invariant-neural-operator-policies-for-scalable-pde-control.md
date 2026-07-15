---
title: "CINOC: Cardinality-Invariant Neural Operator Policies for Scalable PDE Control"
title_zh: CINOC：用于可扩展PDE控制的基数不变神经算子策略
authors: "Pietro Zanotta, Dibakar Roy Sarkar, Honghui Zheng, Somdatta Goswami, Jan Drgona"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1b71e99dc1bbaa7b2fd3c24c12f165bfaf5ae827.pdf"
tags: ["query:physics-ml"]
score: 8.0
evidence: 使用神经算子方法控制配置可变的PDE
tldr: 基于学习的PDE控制策略受固定维度表示限制，无法适应传感器或执行器配置变化。CINOC将PDE控制重述为算子学习问题，将状态场映射到连续控制函数，并通过可微PDE求解器端到端训练。所得策略自然适应不同传感器数量和位置，尤其在多智能体PDE控制中展现了良好的可扩展性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有PDE控制策略对传感器和执行器配置变化缺乏适应性。
method: 将PDE控制转化为算子学习，学习状态场到控制函数的映射，通过可微求解器端到端训练。
result: 在多种配置变化场景下，CINOC策略成功泛化，不需要重新训练。
conclusion: 算子学习范式能有效解决PDE控制的维度可扩展问题。
---

## Abstract
Controlling partial differential equations (PDEs) with learning-based policies remains fundamentally limited by fixed-dimensional representations: policies trained for a specific sensor, actuator, or agent configuration typically fail when the configuration changes. This limitation is particularly severe in multi-agent PDE control, where policies do not scale across population sizes without retraining. We address this challenge by introducing **C**ardinality **I**nvariant **N**eural **O**perator **C**ontrol (**CINOC**), reformulating PDE control as an operator learning problem that maps state fields to continuous control functions and trains them end-to-end through differentiable PDE solvers, yielding policies that naturally adapt to varying sensor and actuator configurations. Remarkably, CINOC policies trained on small swarms exhibit cardinality invariance, allowing for zero-shot transfer to significantly larger populations as well as robustness to partial agent failure. This scalability arises from agents sharing a common policy and coordinating through their physical environment, which produces an emergent self-normalization effect. To explain this phenomenon, we provide a theorem grounded in mean-field theory demonstrating that policy gradients computed from finite-agent systems converge to those of a continuous control limit. Empirically, we validate CINOC on tracking, stabilization, and density transport across linear, nonlinear, chaotic, and turbulent PDEs.

---

## 论文详细总结（自动生成）

以下是对论文 **CINOC: Cardinality-Invariant Neural Operator Policies for Scalable PDE Control** 的中文详细总结。

---

## 1. 核心问题与整体含义

控制偏微分方程（PDE）时，基于学习的策略通常受限于固定维度的表示：针对特定传感器、执行器或智能体配置训练的策略，一旦配置发生变化（例如传感器数量增减、智能体群体规模改变），策略往往失效。这一问题在多智能体 PDE 控制中尤为严重，因为策略无法不经重新训练而跨群体规模扩展。论文提出的 **CINOC** 旨在解决这一可扩展性瓶颈，通过将 PDE 控制重新表述为算子学习问题，使策略天然适应变化的配置，实现零样本迁移。

---

## 2. 方法论

- **核心思想**：将 PDE 控制视为从**状态场**到**连续控制函数**的算子学习，而非固定维度的向量映射。训练通过可微 PDE 求解器进行端到端优化，学习到的策略可以直接处理任意数量和位置的传感器/执行器输入。
- **关键技术**：
  - 使用神经算子（Neural Operator）作为策略网络，保持对输入空间维度的不变性。
  - 多智能体系统中，所有智能体共享同一个算子策略，通过物理环境相互作用和协调，产生“自归一化”效应（emergent self-normalization），使得群体行为自动适应规模变化。
- **理论支撑**：基于平均场理论给出定理，证明从有限智能体系统计算的策略梯度会收敛到连续控制极限下的梯度，解释了零样本迁移的可行性。
- **训练流程**：状态场 → 神经算子编码 → 连续控制函数 → 作用于 PDE 模型（可微分求解器）→ 反向传播更新参数。

---

## 3. 实验设计

- **任务与场景**：
  - 四大类控制任务：跟踪、稳定、密度传输。
  - PDE 类型覆盖线性、非线性、混沌和湍流方程（如波方程、Burgers 方程、Navier-Stokes 方程、Kuramoto-Sivashinsky 方程等）。
- **对比方法**：摘要未明确列出与哪些基线方法比较。推测可能包括经典控制方法（如 MPC）、固定维度的神经网络策略（如 MLP/CNN 策略）或未进行配置泛化的其他学习策略。
- **评估指标**：未在摘要中给出，通常为跟踪误差、稳定时间、密度保真度等。

---

## 4. 资源与算力

论文摘要及提供的元数据中**未明确说明**使用的 GPU 型号、数量、训练时长等算力资源信息。需要指出这一信息缺失。

---

## 5. 实验数量与充分性

- 实验覆盖了**多种 PDE 类型**（线性、非线性、混沌、湍流）和**多种控制任务**（跟踪、稳定、密度传输），场景多样性较好。
- 测试了**零样本迁移**（从小规模群体迁移到大规模群体）和**部分智能体故障鲁棒性**，验证了核心声称的基数不变性。
- 但缺乏详细的**消融实验**（例如不同算子架构的影响、不同训练群体规模的影响）、统计显著性测试以及与其他方法在同一任务下的定量对比。因此实验的**充分性一般**，虽然覆盖面广，但可复现性和公平对比的细节不足。

---

## 6. 主要结论与发现

- CINOC 策略在小规模智能体群体上训练后，能够**零样本迁移**到显著更大的群体，且控制性能保持稳定。
- 对部分智能体失效（agent failure）具有鲁棒性。
- 自归一化效应使得群体行为随规模变化自动调节，无需显式协调机制。
- 理论分析表明，基于有限智能体计算的策略梯度在极限下与连续控制梯度一致，为可扩展性提供了理论依据。

---

## 7. 优点

- **方法创新性强**：将神经算子引入 PDE 控制，从根本上解决维度可扩展问题，思路新颖。
- **零样本迁移能力**：实际中极有价值，可避免为每种配置重新训练。
- **理论结合实证**：提供平均场理论支撑，增强说服力。
- **实验场景丰富**：涵盖多种复杂 PDE 和控制任务，展示方法通用性。

---

## 8. 不足与局限

- **算力信息未公开**：不利于复现和评估能耗成本。
- **对比基线不明确**：缺少与现有可扩展控制方法（如分布式 MPC 或基于图神经网络的策略）的定量比较，无法全面评估优势。
- **消融实验缺失**：未分析算子架构选择、训练群体规模、传感器/执行器分布类型等关键因素的影响。
- **可能忽略实际部署中的偏差**：实验在理想仿真环境下进行，未考虑真实传感器噪声、延迟、模型误差等。
- **应用限制**：方法依赖可微 PDE 求解器，对于复杂不可微或黑箱物理模型无法直接应用；且需要状态场的全局或局部观测，部分实际场景可能不满足。

---

（完）
