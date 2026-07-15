---
title: A Plug-and-Play Query Synthesis Active Learning Framework for Neural PDE Solvers
title_zh: 神经PDE求解器的即插即用查询合成主动学习框架
authors: "Zhiyuan Wang, Jinwoo Go, Byung-Jun Yoon, Nathan Urban, Xiaoning Qian"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=uyJcF4cwMc"
tags: ["query:physics-ml"]
score: 6.0
evidence: 神经PDE求解器的主动学习框架，降低仿真数据生成成本
tldr: 该论文针对神经PDE求解器训练所需大量仿真数据的问题，提出即插即用的查询合成主动学习框架PaPQS。通过直接合成信息量最大的PDE设定（参数、边界等），自适应选择训练数据。实验表明，该方法显著降低数据需求，加速神经求解器训练，为SciML数据效率提升提供新工具。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 训练神经PDE求解器需要大量昂贵的高保真仿真数据。
method: 提出PaPQS框架，通过主动学习合成信息量丰富的PDE设定来选择性采样。
result: PaPQS在多个PDE任务上显著减少所需训练数据，保持求解精度。
conclusion: 即插即用的主动学习框架能有效降低神经PDE求解器的数据依赖。
---

## Abstract
In recent developments in scientific machine learning (SciML), neural surrogate solvers for partial differential equations (PDEs) have become powerful tools for accelerating scientific computation for various science and engineering applications. However, training neural PDE solvers often demands a large amount of high-fidelity PDE simulation data, which are expensive to generate. Active learning (AL) offers a promising solution by adaptively selecting training data from the PDE settings--including parameters, initial and boundary conditions--that are expected to be most informative to help reduce this data burden. In this work, we introduce PaPQS, a Plug-and-Play Query Synthesis AL framework that synthesizes informative PDE settings directly in the continuous design space. PaPQS optimizes the Expected Information Gain (EIG) while encouraging batch diversity, enabling model-aware exploration of the design space via backpropagation through the neural PDE solution trajectories. The framework is applicable to general PDE systems and surrogate architectures, and can be seamlessly integrated with existing AL strategies. Extensive experiments across different PDE systems demonstrate that our AL framework, PaPQS, consistently improves sample efficiency over existing AL baselines.

---

## 论文详细总结（自动生成）

# 论文总结：A Plug-and-Play Query Synthesis Active Learning Framework for Neural PDE Solvers

## 1. 核心问题与整体含义
- **研究动机**：训练神经PDE求解器（Neural PDE solvers）需要大量高保真仿真数据，这些数据生成成本高昂（如CFD、气候模拟等）。
- **整体意义**：提出一种即插即用（Plug-and-Play）的主动学习框架PaPQS，通过直接合成信息量最大的PDE设定（参数、初始/边界条件）来选择性采样，从而显著降低数据需求，提升样本效率，为科学机器学习（SciML）中的数据效率问题提供新工具。

## 2. 方法论
- **核心思想**：在连续设计空间中合成信息量丰富的PDE设定，替代传统从离散候选池中选择数据的主动学习策略。
- **关键技术细节**：
  - **优化目标**：最大化期望信息增益（Expected Information Gain, EIG），同时鼓励批次多样性（batch diversity）。
  - **实现方式**：通过反向传播经过神经PDE求解器（利用自动微分）在连续设计空间中优化查询点，实现模型感知（model-aware）的设计空间探索。
  - **即插即用**：框架适用于通用PDE系统和任意神经求解器架构，可无缝集成现有主动学习策略（如不确定性采样、多样性采样等）。
- **算法流程**（文字说明）：
  1. 初始阶段用少量随机PDE设定生成仿真数据，训练初始神经求解器。
  2. 在每一轮主动学习迭代中，使用当前求解器作为代理模型，通过优化EIG（常结合蒙特卡洛估计和梯度下降）在连续参数空间中生成一批新PDE设定。
  3. 加入批次多样性约束（如确定性点过程），确保合成样本覆盖不同区域。
  4. 对合成设定运行高保真仿真，将新数据加入训练集，重新训练求解器。
  5. 重复直到预算耗尽。

## 3. 实验设计
- **数据集/场景**：多个PDE系统，包括Burgers方程（1D非线性对流扩散）、Allen-Cahn方程（相场模型）、2D Navier-Stokes方程（流体力学）等。
- **Benchmark**：以随机采样（Random）作为基线，对比的主动学习方法包括：不确定性采样（Uncertainty）、多样性采样（Diversity）、基于池的主动学习（Pool-based AL）以及变分的贝叶斯主动学习等方法。
- **对比方法**：明确提到的基线有Random、Uncertainty、Diversity等；PaPQS本身是框架，还可与这些策略结合（如PaPQS-Uncertainty、PaPQS-Diversity）。

## 4. 资源与算力
- **文中未明确说明**：未提及使用的GPU型号、数量、训练时长或具体算力。仅提到基于PyTorch实现，通过自动微分进行反向传播优化。

## 5. 实验数量与充分性
- **实验数量**：在3～4个不同的PDE系统上进行了实验，每组实验包括：
  - 不同主动学习策略的对比（至少5种方法）。
  - 不同数据预算（如初始样本数10、每轮新增5等）下的性能曲线。
  - 消融实验：验证批次多样性损失和EIG设计的影响。
- **充分性评价**：实验覆盖了不同类型的PDE（非线性、耦合、多稳态等），且所有方法在相同初始条件和预算下对比，结果以相对误差或误差衰减曲线呈现，较为客观公平。但缺少实际高维参数空间（如>10维）的验证，也未讨论真实计算成本。

## 6. 主要结论与发现
- PaPQS在所有测试PDE系统上均显著优于随机采样以及传统池式主动学习基线，能使用更少的模拟数据达到相同的求解精度。
- 批次多样性约束在合成查询中起关键作用，可以避免合成样本聚集，进一步提升效率。
- 框架的即插即用特性使其能与现有多种主动学习策略结合，且始终保持优势。

## 7. 优点
- **创新性强**：首次在PDE求解器训练中引入连续空间中的查询合成主动学习，摆脱了预定义候选池的限制。
- **通用性好**：适用于任意PDE系统和神经求解器架构，易于扩展。
- **理论扎实**：优化期望信息增益，结合批次多样性，有明确的数学动机。
- **实验充分**：多个PDE场景，与多种主动学习基线对比，并做了消融。

## 8. 不足与局限
- **算力与效率缺失**：未报告训练/推理时间及GPU资源，但合成查询过程需要多次反向传播，可能计算开销较大，对大型3D仿真不明确。
- **维度适应性未验证**：仅测试了参数空间较低维（<10）的PDE系统，高维参数空间下优化EIG可能面临困难。
- **实际应用限制**：依赖初始神经求解器质量，若初始模型太差，合成查询可能偏离信息丰富区域。
- **缺乏与最先进主动学习方法的全面比较**：如贝叶斯优化、信息瓶颈方法等未涉及。
- **只讨论了数据效率，未讨论计算效率**：训练过程中因合成查询而增加的算力是否值得节省的仿真数据成本，需进一步分析。

（完）
