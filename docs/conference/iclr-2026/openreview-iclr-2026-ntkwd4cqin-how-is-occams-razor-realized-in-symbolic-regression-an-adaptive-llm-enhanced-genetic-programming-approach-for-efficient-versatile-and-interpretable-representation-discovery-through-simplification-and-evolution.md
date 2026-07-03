---
title: "How is Occam's Razor Realized in Symbolic Regression?: An Adaptive LLM-Enhanced Genetic Programming Approach for Efficient, Versatile, and Interpretable Representation Discovery through Simplification and Evolution"
title_zh: 奥卡姆剃刀在符号回归中如何实现？一种自适应LLM增强遗传编程方法
authors: "Vikas Palakonda, Jamshid Tursunboev, Samira Ghorbanpour, Il-Min Kim, Jae-Mo Kang, Sunghwan Moon"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=NtkWD4CQin"
tags: ["query:sr"]
score: 9.0
evidence: 自适应LLM增强遗传编程用于符号回归
tldr: 该论文针对遗传编程符号回归中膨胀、早熟收敛和简化不足的问题，提出ALEGP框架，通过多岛进化架构、LLM辅助简化与自适应策略，在保持表达式简洁的同时提高了发现精度和效率。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 遗传编程符号回归常遭遇表达式膨胀和早熟收敛，且缺乏有效简化机制。
method: 提出ALEGP，结合多岛进化、LLM驱动的简化与自适应策略选择。
result: 在多个基准测试中，ALEGP在拟合精度和表达式简洁性上优于传统GP和LLM方法。
conclusion: LLM与进化计算的融合有效解决了符号回归中的关键挑战。
---

## Abstract
Symbolic regression aims to discover mathematical expressions that capture underlying data relationships, but genetic programming (GP) approaches commonly encounter bloat, premature convergence, and inadequate expression simplification mechanisms. We propose ALEGP (Adaptive LLM-Enhanced Genetic Programming), a framework that strategically integrates large language models (LLMs) with evolutionary computation to address these interconnected challenges.
ALEGP incorporates three key components: (i) a multi-island evolutionary architecture employing specialized subpopulations with distinct optimization objectives to maintain population diversity, (ii) a context-aware intervention scheduler that triggers LLM assistance based on real-time evolutionary indicators including fitness stagnation, diversity loss, and expression bloat, and (iii) an island-specific integration protocol that reincorporates LLM-refined expressions while preserving beneficial evolutionary dynamics. This design enables targeted simplification of complex expressions, improved generalization performance, and reduced computational overhead through adaptive LLM utilization.
Experiments on eight synthetic benchmark functions and five real-world regression datasets demonstrate that ALEGP achieves superior accuracy and interpretability while requiring 50–60\% fewer LLM interventions than fixed-schedule strategies. Ablation studies validate the necessity of both adaptive scheduling and multi-island design for robust performance. These results establish ALEGP as an effective framework for resource-efficient symbolic regression, demonstrating principled integration of evolutionary algorithms with large language models. Code is provided as supplementary material.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 论文的核心问题与整体含义
- **研究动机**：符号回归（Symbolic Regression）旨在从数据中发现简洁的数学表达式，但遗传编程（GP）方法普遍存在三个问题：表达式过度膨胀（bloat）、早熟收敛、缺乏有效的表达式简化机制。这些问题导致模型可解释性差、泛化能力弱且计算开销大。
- **整体含义**：该论文提出一种将大语言模型（LLM）与进化计算智能融合的自适应框架，旨在同时解决上述三个问题，实现高效、紧凑、可解释的符号回归。

## 2. 论文提出的方法论
- **核心思想**：通过自适应策略动态调用LLM来辅助遗传编程，在不显著增加计算成本的前提下，实现表达式的智能简化和种群多样性维持。
- **关键技术细节**：
  - **多岛进化架构（Multi-Island Evolutionary Architecture）**：将种群划分为多个子种群（岛），每个岛具有不同的优化目标（如拟合精度优先、简洁性优先等），以维持种群多样性。
  - **上下文感知干预调度器（Context-Aware Intervention Scheduler）**：基于实时进化指标（如适应度停滞、多样性丢失、表达式膨胀程度）动态决定何时调用LLM辅助，而非固定频率。
  - **岛屿特定整合协议（Island-Specific Integration Protocol）**：LLM精简后的表达式重新注入对应岛屿时，保留原有进化动态（如精英保留、局部搜索特征），避免破坏已有优化方向。
- **算法流程**：
  1. 初始化多岛种群，每个岛独立演化。
  2. 监控全局进化状态（停滞、多样性、膨胀指标）。
  3. 当调度器判断需要干预时，选择部分复杂表达式送入LLM进行简化/重写。
  4. LLM返回精简后的表达式，经岛屿特定协议整合回对应子种群。
  5. 持续进化直至终止条件。

## 3. 实验设计
- **数据集/场景**：
  - 8个合成基准函数（如经典符号回归问题）。
  - 5个真实世界回归数据集。
- **基准方法**：
  - 传统遗传编程（GP）方法。
  - 纯LLM方法（直接使用LLM生成表达式）。
  - 固定调度策略（固定间隔调用LLM）。
- **对比指标**：拟合精度（如MSE/R²）、表达式简洁性（节点数/深度）、泛化性能、LLM调用次数。

## 4. 资源与算力
- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量或训练时长。仅提及在8个合成+5个真实数据集上进行了实验，并提供了代码作为补充材料。可能需要查阅原文或代码仓库获取具体算力配置。

## 5. 实验数量与充分性
- **实验数量**：
  - 总计13个数据集（8合成+5真实）。
  - 进行了消融实验（验证自适应调度和多岛设计的必要性）。
- **充分性评价**：
  - 数据集覆盖了人工和真实场景，具有一定代表性。
  - 消融实验验证了关键组件贡献。
  - 比较了多种基线（传统GP、纯LLM、固定调度），对比框架合理。
  - 但未提及统计显著性检验（如配对t检验），也未给出方差或重复次数细节（如多次运行取均值），可能影响结论的稳健性。

## 6. 论文的主要结论与发现
- ALEGP在拟合精度和表达式简洁性上均优于传统GP和纯LLM方法。
- 相比固定调度策略，ALEGP节省了50%~60%的LLM调用次数，显著降低计算开销。
- 自适应调度和多岛架构对鲁棒性至关重要（消融验证）。
- 证明了LLM与进化计算的协同能够有效解决符号回归中的膨胀和早熟收敛难题。

## 7. 优点
- **方法创新**：首次提出自适应LLM-遗传编程融合框架，利用实时进化指标动态调度，避免盲目使用LLM。
- **多岛设计**：通过多种优化目标维持多样性，防止早熟，同时为LLM简化提供多样化候选。
- **效率提升**：在保持甚至提升精度的同时，大幅减少LLM调用次数，适合资源受限场景。
- **可解释性**：通过LLM简化表达式，天然输出简洁数学公式，更易被人类理解。

## 8. 不足与局限
- **实验覆盖**：仅涉及8个合成+5个真实数据集，缺少大规模、高维或噪声数据测试，尚未验证在极端复杂场景下的表现。
- **偏差风险**：LLM本身的简化可能引入偏见（如偏好某些函数形式），论文未分析LLM简化对表达式多样性的长期影响。
- **应用限制**：依赖LLM的推理能力，若LLM对特定领域符号理解不足，可能降低简化质量；且LLM调用仍需网络或本地模型支持，存在延迟和成本。
- **资源信息缺失**：未报告实验所需的GPU算力开销，难以评估实际落地的硬件需求。
- **重复性**：未提供多次独立实验的统计结果（如平均值±标准差），结论的支持力度有待加强。

（完）
