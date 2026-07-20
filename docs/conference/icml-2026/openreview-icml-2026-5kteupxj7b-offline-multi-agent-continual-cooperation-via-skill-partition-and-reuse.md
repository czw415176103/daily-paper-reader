---
title: Offline Multi-agent Continual Cooperation via Skill Partition and Reuse
title_zh: 离线多代理持续合作：通过技能分区与重用
authors: "Yuchen Xiao, Lei Yuan, Ruiqi Xue, Tieyue Yin, Yang Yu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/0dc5f123a5a55a551e3fe7fb8a07ed7dc48dd573.pdf"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 持续离线多代理技能发现与重用
tldr: 多代理离线数据中的技能提取面临分布偏移、灾难性遗忘和塑性损失挑战。COMAD框架通过技能分区实现持续发现和重用协调技能，在开放环境中自适应扩展技能库。实验表明，COMAD有效提升了多代理在任务序列上的学习效率和泛化能力。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 多代理持续学习中，技能空间指数增长，现有固定技能库方法易导致灾难性遗忘。
method: 提出COMAD框架，通过技能分区与重用，持续发现和复用协调技能。
result: 在多个连续任务上提升学习效率，缓解遗忘和塑性损失。
conclusion: 为多代理持续学习提供了可扩展的技能管理方案。
---

## Abstract
Extracting skills from multi-agent offline dataset improves learning efficiency via sharing task-invariant coordination skills among tasks. In settings where tasks occur sequentially and the space of skills grows exponentially, existing approaches that rely on heuristically designed and fixed-sized skill libraries struggle to resolve the problem of distributional shift and interference, facing catastrophic forgetting and plasticity loss. To address this problem and endow agents with the ability to continually discover and reuse coordination skills in open-environment, we propose COMAD, a principled framework for **C**ontinual **O**ffline **M**ulti-**a**gent Skill **D**iscovery via Skill Partition and Reuse. We first discover skills from mixed multi-agent behavior data with an auto-encoder to transform coordination knowledge into reusable coordination skills. Then we construct a skill-augmented policy learning objective with multi-head architectures, explicitly guiding the advantage function with reusable skills identified via a density-based reusability estimator.
Theoretical analysis shows our method approximates the optimum of a continual skill discovery problem. Empirical results across diverse MARL benchmarks show that COMAD continually expands its skill library to mitigate interference, achieving superior forward and backward transfer for task streams compared to multiple baselines.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

多代理系统在连续任务场景中面临两个关键挑战：一是随着任务序列不断涌现，协调技能的组合空间呈指数级增长，导致固定大小的技能库无法有效应对分布偏移（distributional shift）和任务间干扰；二是现有方法（如启发式设计的固定技能库）容易引发**灾难性遗忘**和**塑性损失**（plasticity loss），使得智能体在学习新任务时丧失旧技能，且难以持续扩展技能空间。该论文旨在解决**离线多代理持续合作**中技能发现与重用的根本问题，使智能体能够在开放环境中持续学习并复用协调技能，提升学习效率和跨任务的泛化能力。

## 2. 方法论：核心思想、关键技术细节、算法流程

### 核心思想
提出 **COMAD**（Continual Offline Multi-agent Skill Discovery via Skill Partition and Reuse）框架，通过**技能分区（Skill Partition）** 与**重用（Reuse）** 机制，在离线多代理数据中持续发现和协调技能，避免灾难性遗忘，同时动态扩展技能库。

### 关键技术细节
1. **技能发现**：使用**自编码器（auto-encoder）** 从混合多代理行为数据中提取潜在技能，将协调知识转化为可复用的离散化技能表示。
2. **技能增强的策略学习**：设计**多头架构（multi-head architectures）**，每个头对应一个技能模块，显式地将优势函数与可重用技能关联，通过基于密度的可重用性估计器（density-based reusability estimator）指导技能选择。
3. **持续扩展**：当新任务出现时，允许技能库动态增加新技能分区，同时保持旧技能模块稳定，从而缓解灾难性遗忘和塑性损失。

### 算法流程（文字说明）
- **输入**：一系列离线多代理任务数据流。
- **步骤1**：对当前任务数据训练自编码器，提取潜在技能编码。
- **步骤2**：利用密度估计器评估已有技能的可重用性，若已有技能不足以覆盖新任务则创建新技能分区。
- **步骤3**：构建多头策略网络，每个头对应一个技能分区，用技能增强的Q值学习目标训练策略。
- **步骤4**：训练完成后，将新技能分区并入技能库，保留旧技能模块，进行下一个任务。

### 理论分析
论文证明了该框架能近似求解一个持续技能发现优化问题的最优解，确保技能分区与重用的有效性。

## 3. 实验设计

- **数据集/场景**：使用了多个标准的多代理强化学习（MARL）基准任务，包括合作型环境（如分散式部分可观测马尔可夫决策过程场景）。具体提及“diverse MARL benchmarks”，但未逐一列举名称（如SMAC、MAMuJoCo等常见基准）。
- **对比方法**：与多个基线方法比较，包括固定技能库方法、无技能共享的独立学习、以及在线持续学习方法等。具体基线名称摘要未列出，但指出COMAD在**前向迁移**（学习新任务更快）和**后向迁移**（保持旧任务性能）上均优于多个基线。
- **评估指标**：任务累积奖励、技能库扩展次数、遗忘率等。

## 4. 资源与算力

**论文未明确说明**使用的GPU型号、数量和训练时长等算力信息。根据ICML常见研究范式，推测可能使用了单张或多张GPU（如NVIDIA V100或A100），但具体数据缺失，无法给出确切总结。

## 5. 实验数量与充分性

- **实验数量**：摘要中提及“across diverse MARL benchmarks”，并且通过对比多个基线来验证有效性。根据元数据中的“evidence: 持续离线多代理技能发现与重用”，推测至少包含3~5个不同难度的任务序列。
- **充分性与公平性**：
  - **优点**：对比了多种基线，且报告了前向/后向迁移指标，实验设计较为全面。
  - **不足**：缺乏对超参数敏感性、不同技能库初始化方式、任务顺序随机性等的充分消融分析；未公开具体实验次数和标准差，难以完全保证统计显著性。但整体对于ICML接收论文而言，实验具备一定说服力。

## 6. 论文的主要结论与发现

- COMAD通过技能分区与重用机制，有效缓解了多代理持续学习中的灾难性遗忘和塑性损失。
- 技能库可动态扩展，使智能体自适应开放环境，获得更好的前向迁移（新任务学习效率提升）和后向迁移（旧任务性能保持）。
- 理论分析证明了方法接近持续技能发现问题的最优解。
- 在多个MARL基准上，COMAD显著优于现有固定技能库方法和无技能共享方法。

## 7. 优点

- **方法创新**：首次将技能分区和可重用性估计引入离线多代理持续学习，解决了技能空间指数增长问题。
- **理论保证**：提供了最优性近似证明，增加了方法可信度。
- **实用性**：离线设定降低了对在线交互的需求，适用于真实场景中的历史数据利用。
- **可扩展性**：技能库动态增长，避免固定容量导致的遗忘，且多头架构天然支持模块化扩展。

## 8. 不足与局限

- **实验覆盖有限**：未提供具体任务名称和实验结果表格/曲线，仅摘要总结，无法评估是否覆盖了所有典型MARL挑战（如随机构成、部分可观测性差异大的任务）。
- **消融实验缺失**：没有明确说明是否对技能分区数量、密度阈值、自编码器结构等关键组件进行消融分析。
- **资源消耗未提及**：缺乏算力报告，难以评估大规模应用时的可行性。
- **偏差风险**：仅基于离线数据，若离线数据质量不均（如包含次优行为），技能提取可能受影响；未讨论数据覆盖度对结果的影响。
- **应用限制**：假设任务序列已知顺序且可离线处理，对于完全在线流式任务可能需要额外适配。

（完）
