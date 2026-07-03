---
title: A Heterogeneous Graph Optimization Perspective for Multi-Agent System Workflows
title_zh: 多智能体系统工作流的异构图优化视角
authors: "Bowen Guan, Runze Wang, Yanming Shen"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=vvSrgJIdvn"
tags: ["query:cfd-agent"]
score: 6.0
evidence: 通过图方法优化多智能体系统工作流
tldr: 该论文针对大语言模型多智能体系统工作流设计困难的问题，提出异构图优化方法HeGFlow。将代理、工具和推理步骤建模为异构图组件，通过优化邻接矩阵自动发现高效工作流。实验表明HeGFlow在多种复杂任务上自动化了工作流设计，提升了系统性能。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 手动设计多智能体工作流难以扩展，自动方法未充分利用历史上下文。
method: 构建异构图表示，将工作流优化转化为邻接矩阵学习问题。
result: 在复杂任务上，HeGFlow自动生成的工作流优于手动设计和现有自动方法。
conclusion: 异构图视角为多智能体工作流优化提供了系统化解决方案。
---

## Abstract
Large Language Model (LLM)-based multi-agent systems (MAS) have shown potential in solving complex tasks across a wide range of domains. However, designing effective MAS workflows remains a significant challenge. Manually crafted workflows are difficult to scale and adapt. Automated workflow optimization techniques usually depend heavily on the planning capability of meta-agent, cannot fully utilize the historical context, and neglect the dynamic interactions between agents and tools. To address these limitations, we propose Heterogeneous Graph-based workFlow optimization (HeGFlow), which models agents, tools, and reasoning steps as interconnected graph components, transforming the design of MAS workflow into a heterogeneous graph adjacency matrix optimization problem. To efficiently explore the vast search space, HeGFlow introduces a two-stage matrix training process guided by a subgraph sampling strategy. Extensive experiments across six complex domains show that HeGFlow enables smaller LLMs to match or even surpass the performance of much larger models. Furthermore, HeGFlow consistently outperforms existing manually and automated workflow approaches on four widely-used benchmarks, establishing a new paradigm for scalable and effective MAS workflow generation.

---

## 论文详细总结（自动生成）

好的，以下是根据提供的论文元数据和摘要信息生成的详细中文总结。

## 1. 核心问题与整体含义（研究动机和背景）

*   **研究背景**：基于大语言模型（LLM）的多智能体系统（MAS）在处理复杂任务中展现出潜力，但设计有效的工作流（workflows）仍然面临重大挑战。
*   **核心问题**：
    *   手动设计工作流难以扩展和适应不同任务。
    *   现有的自动化工作流优化技术通常严重依赖元代理（meta-agent）的规划能力，未能充分利用历史上下文（historical context），且忽视了智能体与工具之间的动态交互。
*   **整体含义**：为了解决上述问题，论文提出了一种新的视角——将多智能体工作流建模为异构图，通过优化图的邻接矩阵来自动发现高效工作流，从而提升MAS的可扩展性和性能。

## 2. 方法论：核心思想、关键技术细节

*   **核心思想**：提出**HeGFlow**（Heterogeneous Graph-based workFlow optimization），将代理（agents）、工具（tools）和推理步骤（reasoning steps）建模为异构图的组件，将工作流设计问题转化为异构图邻接矩阵优化问题。
*   **关键技术细节**：
    *   **异构图建模**：将MAS中的不同实体（如智能体、工具、推理步骤）视为不同类型节点，它们之间的交互关系（如调用、顺序、反馈）视为边，构建异构图。
    *   **优化目标**：通过优化图的邻接矩阵，自动发现最优的工作流结构（即节点之间的连接方式）。
    *   **搜索空间高效探索**：为处理巨大的搜索空间，HeGFlow引入**两阶段矩阵训练过程**，并结合**子图采样策略**，逐步收敛到高质量的工作流配置。
*   **算法流程（文字说明）**：
    1.  根据任务需求，初始化一个包含可能节点（代理、工具、步骤）的异构图模板。
    2.  使用子图采样策略从全图中抽取子图，降低每次优化的复杂度。
    3.  通过两阶段训练（例如先粗粒度搜索，再细粒度微调）优化邻接矩阵，学习各节点之间的最优连接权重。
    4.  根据优化后的邻接矩阵解码出具体的工作流，供多智能体系统执行。

## 3. 实验设计

*   **数据集/场景**：论文在**六个复杂领域**（具体领域未在元数据中列出，可能包括数学推理、代码生成、问答等）进行实验。
*   **基准（benchmark）**：使用了**四个广泛使用的基准**（具体名称未提及，可能包括类似HotPotQA、MATH等常见LLM评估集）。
*   **对比方法**：
    *   **手工设计工作流**：人工精心编排的MAS流程。
    *   **现有自动化方法**：其他基于元代理规划或固定模板的自动化工作流生成方法。
    *   同时比较了不同规模LLM（如较小LLM与更大LLM）在HeGFlow优化下的性能。

## 4. 资源与算力

*   论文摘要和元数据中**未明确说明**所使用的GPU型号、数量、训练时长等具体算力信息。因此，无法提供此处的详细总结，仅能指出这一点。

## 5. 实验数量与充分性

*   **实验数量**：覆盖了**六个复杂领域**和**四个广泛基准**，并进行了与手工及自动化方法的系统对比。此外，结合子图采样和两阶段训练，隐含了消融实验（如采样策略、训练阶段的影响）。
*   **充分性与公平性**：
    *   **充分性**：多领域、多基准的评估足够支撑其核心结论。但未提供消融实验的明确细节，略显不完整。
    *   **公平性**：对比了多种基线方法（手工和自动），且展示了较小LLM通过优化后可超越较大LLM，证明了方法的通用性。但需注意是否在同一算力条件下进行对比（未说明）。

## 6. 主要结论与发现

*   在六个复杂领域上，HeGFlow能够使**较小LLM**达到甚至超越**更大LLM**的性能。
*   在四个广泛使用的基准上，HeGFlow**一致优于**现有手工和自动化工作流方法。
*   实证表明：异构图视角为多智能体工作流优化提供了一种系统化、可扩展且有效的解决方案，建立了新的范式。

## 7. 优点

*   **方法创新性**：首次将MAS工作流优化形式化为异构图邻接矩阵优化问题，提供了理论清晰且系统化的框架。
*   **自动化与可扩展性**：避免了手工设计的繁琐和元代理规划的不稳定性，能够自动发现高效工作流，且通过子图采样解决了搜索空间爆炸问题。
*   **性能提升显著**：使小模型通过优化达到大模型水平，具有实际部署价值（节省算力成本）。
*   **实验覆盖广泛**：多领域、多基准验证了泛化能力。

## 8. 不足与局限

*   **实验细节缺失**：具体使用的数据集、基准名称、模型大小、对比方法详细列表均未在提供的元数据中给出，难以复现和深入评价。
*   **算力信息未披露**：无法判断方法是否依赖于大量计算资源或是否在公平比较下进行。
*   **消融研究不充分**：虽然提到两阶段训练和子图采样，但缺少对这两个关键设计的单独消融实验量化分析（如采样比例、训练阶段贡献度）。
*   **应用限制**：可能仅限于可建模为异构图的工作流类型；对于需要高度动态交互或实时反馈的场景，固定图优化可能不够灵活。另外，异构图构建本身也需要一定先验知识（如节点类型定义）。

（完）
