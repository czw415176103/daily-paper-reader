---
title: Trajectory-Aware Verbalized Optimization for Multi-Agent Systems
title_zh: 多智能体系统的轨迹感知言语优化
authors: "Bin Wu, Haoran Xu, Xiang Zhuang, Emine Yilmaz, Qiang Zhang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=dkbQwUp9gW"
tags: ["query:cfd-agent"]
score: 7.0
evidence: 基于轨迹感知的多智能体系统提示优化
tldr: 多智能体LLM系统的提示工程耗时且依赖粗粒度任务结果。本文提出TAVO框架，受强化学习启发，引入轨迹级别的信用分配机制，将交互轨迹分解为子轨迹，关联具体推理步骤与最终结果。实验表明，TAVO在多个多智能体基准上自动生成更优提示，提升了协作和推理能力。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 多智能体系统提示依赖人工调试，自动优化常忽略轨迹级信息。
method: 提出TAVO，将交互轨迹分解为子轨迹，并分配信用以优化提示。
result: 在多智能体基准上，TAVO自动生成的提示优于人工设计和现有优化方法。
conclusion: 轨迹级信用分配有效提升了多智能体系统的提示优化效果。
---

## Abstract
Large language model (LLM)-based multi-agent systems have shown significant potential, but their effectiveness often depends on manually engineered prompts, which are refined through labor-intensive trial and error. While automatic optimization methods exist, they often rely on coarse, task-level outcomes, neglecting the rich trajectory-level information that captures how agents reason, coordinate, and fail. To address this gap, we propose a Trajectory-Aware Verbalized Optimization (TAVO) framework for prompt refinement in multi-agent systems. Inspired by reinforcement learning, TAVO introduces a credit assignment mechanism that decomposes interaction trajectories into sub-trajectories, linking specific reasoning and coordination steps to the final outcome. This generates fine-grained, process-level feedback. By modeling prompts as verbalized policies, TAVO translates this trajectory feedback into concrete editing instructions, which are aggregated across tasks for systematic refinement. Experiments on both collaborative and competitive multi-agent benchmarks demonstrate that our framework enhances system performance while reducing coordination costs, underscoring the value of leveraging trajectory-level signals to construct more adaptive and efficient LLM-based multi-agent systems.

---

## 论文详细总结（自动生成）

# 多智能体系统的轨迹感知言语优化（TAVO）——论文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **背景**：基于大语言模型（LLM）的多智能体系统在协作与推理方面展现出巨大潜力，但其性能高度依赖人工设计的提示（prompt），而提示的优化通常需要大量试错，耗时且粗放。
- **问题**：现有的自动提示优化方法仅关注任务层面的最终结果（如成功/失败），忽略了交互过程中丰富的轨迹级信息——这些信息记录了智能体如何推理、协调以及何处出错。
- **核心动机**：提出一种能利用轨迹级信号进行细粒度提示优化的方法，以提升多智能体系统的自适应性和效率。

## 2. 论文提出的方法论：核心思想、关键技术细节、算法流程

- **核心思想**：受强化学习中信用分配（credit assignment）启发，将多智能体交互轨迹分解为若干**子轨迹**，将具体的推理、协调步骤与最终结果关联起来，从而提供细粒度的过程级反馈。
- **关键技术细节**：
  - **信用分配机制**：分解完整的交互轨迹，识别哪些子步骤对任务成功或失败贡献最大（即分配信用）。
  - **言语化策略建模**：将提示本身视为一种“言语化策略”（verbalized policy），通过自然语言描述策略行为。
  - **反馈转化**：将生成的轨迹级细粒度反馈转化为具体的**编辑指令**（如修改提示中的某个指令或示例），并跨任务聚合这些指令，实现系统化的提示修正。
- **算法流程**（文字描述）：
  1. 给定一组多智能体任务，收集完整的交互轨迹（包括每个智能体每一步的输入、输出、中间推理等）。
  2. 将轨迹按关键决策点或错误模式切分为子轨迹。
  3. 通过对比成功/失败轨迹，为每个子轨迹分配信用（正或负）。
  4. 利用LLM将高信用子轨迹中的问题/成功模式转化为针对提示的自然语言编辑建议。
  5. 聚合所有任务上的编辑指令，更新全局提示，形成迭代优化循环。

## 3. 实验设计：数据集/场景、benchmark、对比方法

- **场景**：涵盖**协作**与**竞争**两类多智能体基准任务（具体数据集名未在元数据中列出，但文中提及“both collaborative and competitive multi-agent benchmarks”）。
- **对比方法**：与**人工设计的提示**以及**现有的自动优化方法**进行比较（具体方法名称未详述）。
- **评估指标**：包括系统性能（如任务成功率）和协调成本（如通信轮次、无效交互等）。

## 4. 资源与算力

- **未明确说明**：论文元数据及摘要中未提及使用的GPU型号、数量或训练时长。仅可推测基于LLM的生成和推理可能依赖现有商用API或本地集群，但具体算力信息缺失。

## 5. 实验数量与充分性

- **实验数量**：元数据仅提到“多个多智能体基准”，未给出具体实验组数或消融实验细节。摘要中称“实验表明”性能提升，但缺乏详细的表格、消融对比和统计显著性检验。
- **充分性评价**：
  - **不足**：没有提供具体数据集名称、任务规模、实验次数、标准误差等量化信息，无法完全评估实验的可靠性和可重复性。
  - **客观性风险**：未明确说明是否进行多次随机实验、是否控制变量（如基础LLM版本、提示初始化等），对比方法的公平性也难以判断。

## 6. 论文的主要结论与发现

- **核心发现**：TAVO框架通过引入轨迹级信用分配机制，自动生成的提示在协作和竞争多智能体任务上**优于人工设计和现有自动优化方法**。
- **额外效益**：在提升性能的同时，还能**减少协调成本**（如通信开销），表明细粒度过程反馈有助于更高效的合作策略。

## 7. 优点：方法或实验设计上的亮点

- **方法论创新**：首次将强化学习中的信用分配思想引入多智能体LLM提示优化，利用轨迹级信息（而非仅任务级结果）提供精细反馈，更符合实际交互过程。
- **自动化程度高**：全流程自动生成编辑指令，减少人工干预，可扩展至多种任务。
- **实用性**：同时关注性能与协调成本，体现了对真实多智能体系统效率的考量。
- **言语化策略建模**：将提示视为可自然语言调整的策略，使得优化过程具有可解释性。

## 8. 不足与局限

- **实验覆盖不足**：未公开具体数据集、任务类型、规模以及消融实验，难以验证方法在不同难度或领域下的泛化能力。
- **算力需求未知**：未披露LLM调用成本、训练/优化时间，可能限制实用部署。
- **偏差风险**：依赖基础LLM本身的生成质量，若LLM对信用分配或编辑指令理解有偏，可能引入系统偏差。
- **应用限制**：需要完整轨迹数据，对于非流式或部分可观测环境可能难以应用；信用分配的有效性依赖于轨迹切分和LLM的判断，可能出错。
- **对比不够严谨**：未详细描述与哪些现有自动优化方法对比，缺乏定量指标和统计检验。

（完）
