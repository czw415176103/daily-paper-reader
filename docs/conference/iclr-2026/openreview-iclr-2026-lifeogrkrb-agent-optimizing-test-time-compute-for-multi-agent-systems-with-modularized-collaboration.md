---
title: "AGENT*: Optimizing Test-Time Compute for Multi-Agent Systems with Modularized Collaboration"
title_zh: "AGENT*：通过模块化协作优化多智能体系统的测试时计算"
authors: "Dongwon Jung, Peng Shi, Yi Zhang"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=lifeoGrKRB"
tags: ["query:cfd-agent"]
score: 6.0
evidence: 多智能体系统的测试时计算优化
tldr: "该论文针对多智能体系统中测试时计算分配缺乏协作机制的问题，提出AGENT*框架。通过模块化协作和预算约束下的计算调度，实现多代理间高效协作。在多个基准上，AGENT*在有限计算预算内显著提升了任务成功率。"
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有测试时扩展方法在多智能体系统中无法有效促进协作和管理计算。
method: 设计模块化协作框架，在计算预算约束下优化多代理的推理计算分配。
result: "AGENT*在计算预算严格的情况下提高了多智能体系统的任务完成质量。"
conclusion: 模块化协作与计算优化相结合，可提升多智能体系统的实用性和效率。
---

## Abstract
Scaling test-time computation has emerged as a powerful and increasingly popular approach for improving the performance of large language models without additional training. Recent work demonstrates that techniques such as repeated sampling, self-verification, and self-reflection can significantly enhance task success by allocating more inference-time compute. However, applying these techniques directly to multi-agent systems is challenging, as they provide no principled way to encourage collaboration or manage compute allocation across multiple agents under budget constraints. To address this, we propose AGENT*, a general framework for enabling effective multi-agent collaboration while operating within strict compute budgets. AGENT* introduces the notion of \emph{modularized collaboration}, formalized as callable functions that encapsulate reusable multi-agent workflows, automatically constructed via self-play reflection by abstracting recurring interaction patterns from past trajectories. Building on these collaboration modules, AGENT* proposes \emph{a dual-level planning architecture} that optimizes compute allocation by reasoning over the current task state while also \emph{speculating} on future steps. Experiments on complex agent benchmarks demonstrate that AGENT* consistently outperforms baselines across diverse budget settings, validating its effectiveness for multi-agent collaboration in inference-time optimization.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：当前通过扩展测试时计算（test-time computation）来提升大语言模型性能的方法（如重复采样、自我验证、自我反思）已被证明有效，但这些方法直接应用于多智能体系统时存在两大缺陷：**无法有效促进多智能体之间的协作**，以及**缺乏在预算约束下跨智能体管理计算分配的机制**。
- **核心问题**：如何在有限的计算预算内，实现多智能体系统的高效协作与计算资源优化分配。
- **整体含义**：提出AGENT*框架，旨在通过模块化协作和双层规划架构，使多智能体系统在推理阶段能以更低计算成本获得更高任务成功率。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：引入**模块化协作**（Modularized Collaboration）概念，将可复用的多智能体工作流封装为可调用的函数模块；同时设计**双层规划架构**（Dual-level Planning Architecture），在优化当前任务步骤的计算分配时，对未来步骤进行推测（speculation）。
- **关键技术细节**：
  - **协作模块的自建构**：通过自我博弈反思（self-play reflection），从过去交互轨迹中抽象出重复出现的协作模式，自动构建可复用的模块化协作函数。
  - **计算分配优化**：底层规划根据当前任务状态分配计算资源；顶层规划对未来步骤进行推测，从而全局优化预算使用。
- **算法流程（文字说明）**：系统首先通过历史轨迹学习并构建协作模块库；当面对新任务时，双层规划器在计算预算约束下，选择最合适的协作模块序列并分配各模块的推理计算量，确保整体协作高效且不超预算。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **数据集/场景**：论文摘要仅提及“在复杂智能体基准上”进行实验，未具体列出数据集名称（如AlfWorld、WebArena、M3等未提及）。
- **Benchmark**：未明确给出标准benchmark名称。
- **对比方法**：摘要未列出具体基线方法名称，仅表述为“一致优于基线”（consistently outperforms baselines）。根据元数据中“score: 6.0”及“source: ICLR-2026-Rejected-Public”，推测可能包括常见多智能体协作方法或单智能体扩展方法，但具体不详。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点。

- **未明确说明**：论文提供的文本（摘要及元数据）中未提及任何关于GPU型号、数量、训练时长或其他计算资源的信息。需要指出这一点。

## 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平。

- **实验数量**：论文摘要仅提到在“多个复杂智能体基准”上实验，且进行了“不同预算设置”的对比。未给出具体实验组数、消融实验详情。
- **充分性评估**：由于信息有限，无法判断实验数量是否充分。但元数据显示该论文被ICLR 2026拒稿（Rejected-Public），可能实验设计或结果说服力存在不足。不过从摘要看，AGENT*在多种预算下均优于基线，至少初步支持了方法的有效性。
- **客观公平性**：未说明基线方法是否经过同等优化或超参数调优，公平性无法确定。

## 6. 论文的主要结论与发现

- **主要结论**：AGENT*框架能够有效结合模块化协作与计算预算优化，在推理阶段显著提升多智能体系统的任务完成质量，且能在不同计算预算下保持优势。
- **关键发现**：模块化协作的自动构建和双层规划推测机制是提升协作效率的关键，使系统能在有限计算资源内做出更优决策。

## 7. 优点：方法或实验设计上有哪些亮点

- **方法亮点**：
  - 提出**模块化协作**概念，将协作工作流抽象为可复用函数，支持自动构建，降低手动设计成本。
  - **双层规划架构**同时考虑当前和未来步骤，实现前瞻性计算分配，与现有单步优化方法形成差异。
  - 适用于严格计算预算场景，实用性强。
- **实验亮点**（基于有限信息）：在多个复杂基准和多预算设置下进行验证，展示了方法的泛化能力和预算鲁棒性。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验覆盖不足**：未公开具体数据集、基线方法、消融实验等细节，难以全面评估方法的普适性和消融贡献。
- **偏差风险**：自我博弈反思构建模块依赖历史轨迹质量，若轨迹分布偏移或任务分布变化，模块可能失效。
- **应用限制**：
  - 当前仅针对测试时计算优化，未涉及训练阶段或模型本身修改。
  - 模块化协作需要预建模块库，在全新任务领域可能需要额外适应。
  - 未讨论多智能体通信开销或模块调用延迟对实际系统的影响。
- **算力信息缺失**：未提供实验计算资源，影响结果可复现性和实用性评估。
- **论文状态**：该论文被ICLR 2026拒稿，可能说明方法或实验存在尚未解决的根本性问题。

（完）
