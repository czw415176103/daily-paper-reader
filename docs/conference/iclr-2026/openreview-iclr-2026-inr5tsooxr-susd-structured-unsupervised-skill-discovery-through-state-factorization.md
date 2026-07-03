---
title: "SUSD: Structured Unsupervised Skill Discovery through State Factorization"
title_zh: SUSD：通过状态因式分解的结构化无监督技能发现
authors: "Seyed Mohammad Hadi Hosseini, Mahdieh Soleymani Baghshah"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=INr5TSooxR"
tags: ["query:cfd-agent"]
score: 7.0
evidence: 基于状态因式分解的结构化无监督技能发现
tldr: 该论文指出现有无监督技能发现方法偏好静态技能，限制了动态任务相关技能的发现。提出SUSD框架，利用环境组合结构通过状态因式分解鼓励全面动态技能，实验表明其发现的技能多样性和下游任务性能均优于基线。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有无监督技能发现方法易学到简单静态技能，缺乏动态和任务相关技能。
method: 提出SUSD，通过状态因式分解利用环境组合结构发现覆盖所有可控因素的技能。
result: 在多个连续控制环境中，SUSD发现的技能更多样，在下游任务中取得更高奖励。
conclusion: SUSD通过结构化分解有效促进了全面动态技能的发现。
---

## Abstract
Unsupervised Skill Discovery (USD) aims to autonomously learn a diverse set of skills without relying on extrinsic rewards. One of the most common USD approaches is to maximize the Mutual Information (MI) between skill latent variables and states. However, MI-based methods tend to favor simple, static skills due to their invariance properties, limiting the discovery of dynamic, task-relevant behaviors. Distance-Maximizing Skill Discovery (DSD) promotes more dynamic skills by leveraging state-space distances, yet still fall short in encouraging comprehensive skill sets that engage all controllable factors or entities in the environment. 
In this work, we introduce SUSD, a novel framework that harnesses the compositional structure of environments by factorizing the state space into independent components (e.g., objects or controllable entities). SUSD allocates distinct skill variables to different factors, enabling more fine-grained control on the skill discovery process. A dynamic model also tracks learning across factors, adaptively steering the agent’s focus toward underexplored factors.
This structured approach not only promotes the discovery of richer and more diverse skills, but also yields a factorized skill representation that enables fine-grained and disentangled control over individual entities which facilitates efficient training of compositional downstream tasks via Hierarchical Reinforcement Learning (HRL).
Our experimental results across three environments, with factors ranging from 1 to 10, demonstrate that our method can discover diverse and complex skills without supervision, significantly outperforming existing unsupervised skill discovery methods in factorized and complex environments. Code is available at the anonymous repository: [https://anonymous.4open.science/r/SUSD](https://anonymous.4open.science/r/SUSD).

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：无监督技能发现（Unsupervised Skill Discovery, USD）旨在不依赖外在奖励的情况下自主学到多样化的技能。现有主流方法基于互信息（MI）最大化，但这类方法因不变性偏好简单、静态的技能，限制了动态、任务相关行为的发现。另一类方法（如距离最大化技能发现 DSD）虽能促进动态技能，但仍难以覆盖环境中所有可控因素或实体。
- **核心问题**：如何利用环境的组合结构，鼓励发现覆盖所有可控因素的全面动态技能，并实现细粒度解耦控制。
- **整体含义**：提出 SUSD 框架，通过状态因式分解（state factorization）将状态空间分解为独立组件（如物体或可控实体），为不同因子分配不同的技能变量，从而更精细地引导技能发现过程，并借助动态模型自适应地探索未充分学习的因子。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：利用环境的组合结构，将状态空间分解为**多个独立因子**（例如物体、关节等可控实体），每个因子对应一个**专门的技能变量**。通过最大化每个因子与其对应技能变量之间的互信息，同时利用**动态模型**跟踪各因子的学习进度，自适应地引导智能体关注尚未充分探索的因子。
- **关键技术细节**：
  - **状态因式分解**：假设环境状态可以分解为 \( s = (s_1, s_2, ..., s_K) \)，其中每个 \( s_i \) 是独立可控因子。
  - **技能分配**：为每个因子 \( i \) 分配一个独立的潜变量 \( z_i \)，技能策略 \( \pi(a|s, z_1,...,z_K) \) 输出动作。
  - **优化目标**：对每个因子分别最大化互信息 \( I(s_i; z_i) \)（例如通过变分下界），同时鼓励因子间解耦。
  - **动态探索引导**：使用一个动态模型（如预测器）估计每个因子的学习进展（例如预测误差或信息增益），根据未探索程度调整技能采样权重，使智能体更多地去探索那些尚未被良好覆盖的因子。
- **流程说明**：  
  1. 环境状态分解为多个因子；  
  2. 每个因子绑定一个技能潜变量；  
  3. 策略网络根据联合潜变量输出动作；  
  4. 优化每个因子的互信息目标，同时动态模型计算每个因子的探索优先级；  
  5. 自适应调整批次中技能潜变量的采样分布，优先学习欠探索因子。

### 3. 实验设计：数据集/场景、Benchmark、对比方法

- **实验场景**：三个连续控制环境，因子数量从 1 到 10（具体环境名称摘要未列出，元数据中只提到“factorized environments”）。
- **Benchmark**：与现有无监督技能发现方法（包括 MI-based 方法和 DSD 方法）进行比较；下游任务通过分层强化学习（HRL）评估性能。
- **对比方法**：
  - 互信息最大化类方法（如 DIAYN、DADS 等）
  - 距离最大化技能发现方法（DSD）
- **评估指标**：技能多样性（如状态覆盖率、熵）、下游任务累积奖励。

### 4. 资源与算力

- **文中未明确说明**使用的 GPU 型号、数量、训练时长等算力信息。仅提供代码仓库地址，未提及训练硬件细节。因此无法从现有文本中总结具体算力需求。

### 5. 实验数量与充分性

- **实验数量**：三个环境，覆盖因子数 1 到 10，对比多个基线方法。元数据中可能包含消融实验（如动态模型的作用、因子分解方式），但摘要未详细列出，需查阅全文确认。
- **充分性分析**：环境数量较少（3个），但因子范围较宽（1~10），能体现方法在组合复杂度增加时的表现。缺乏在更复杂（如高维视觉）或真实机器人场景中的验证。实验设计较为系统（有与多种基线的对比），但未提及重复次数、统计显著性等细节。整体而言，实验在展示方法有效性上具备一定说服力，但**充分性中等偏低**——尤其缺少大规模或多样化环境的验证。

### 6. 论文的主要结论与发现

- SUSD 能发现比现有方法**更丰富、更多样**的技能，尤其是覆盖所有可控因子的动态技能。
- 在下游任务（通过 HRL 利用因子化技能表示）中，SUSD 显著超越现有无监督技能发现方法，取得更高奖励。
- 因子化的技能表示允许对单个实体进行**细粒度、解耦的控制**，有利于组合式下游任务的学习。
- 动态模型自适应探索机制有效避免了某些因子被忽略的问题。

### 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - 创新性地将 **状态因式分解** 与 **技能发现** 结合，利用环境内在的组合结构，使技能覆盖所有可控因子。
  - **动态探索引导机制**：自动调整学习焦点，避免因子不平衡，提升学习效率。
  - 技能表示天然解耦，可直接用于 HRL 实现多层级控制。
- **实验设计亮点**：
  - 覆盖从单一因子到多因子（10个）的多种复杂度环境，体现可扩展性。
  - 与多种代表性基线（MI类、DSD类）全面对比。

### 8. 不足与局限

- **实验覆盖有限**：仅三个环境，且未公开环境名称（需查阅论文附录），缺少在视觉、高维状态空间或真实机器人上的验证。
- **因子分解依赖先验知识**：假设状态可分解为独立可控因子，这在许多实际任务中可能不成立或难以获得，限制泛化性。
- **未讨论计算开销**：多技能变量 + 动态模型增加模型复杂度，可能带来较大训练成本，文中未分析。
- **缺少消融实验细节**：摘要未明确展示各组件（如动态模型、因子分解方式）的独立贡献，需全文补充。
- **超参数敏感性未知**：未提及因子数量、技能维度等超参数选择的影响。

（完）
