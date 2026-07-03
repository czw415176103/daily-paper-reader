---
title: "Rethinking the Value of Multi-Agent Workflow: A Strong Single Agent Baseline"
title_zh: 重新思考多智能体工作流的价值：一个强大的单智能体基线
authors: "Jiawei Xu, Arief Koesdwiady, Sisong Bei, Yan Han, Baixiang Huang, Dakuo Wang, Yutong Chen, Zheshen Wang, Peihao Wang, Pan Li, Ying Ding"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=i95lcR2GN5"
tags: ["query:cfd-agent"]
score: 4.0
evidence: 单智能体与同构多智能体工作流的比较，与智能体系统架构相关
tldr: 当前多智能体系统常使用同质大语言模型智能体。本文通过七个基准测试发现，单智能体通过多轮对话即可达到同质多智能体工作流的性能，且效率更高。这启发重新思考多智能体架构的价值。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 多智能体工作流是否真的优于单智能体尚存疑问。
method: 在多个基准上比较单智能体多轮对话与同构多智能体工作流的性能。
result: 单智能体可达到同等性能且更高效。
conclusion: 同构多智能体工作流的优势可能被高估。
---

## Abstract
Recent advances in LLM-based multi-agent systems (MAS) show that workflows composed of multiple LLM agents with distinct roles, tools, and communication patterns can outperform single-LLM baselines on complex tasks. However, most frameworks are homogeneous, where all agents share the same base LLM and differ only in prompts, tools, and positions in the workflow. This raises the question of whether such workflows can be simulated by a single agent through multi-turn conversations. We investigate this across seven benchmarks spanning coding, mathematics, general question answering, domain-specific reasoning, and real-world planning and tool use. Our results show that a single agent can reach the performance of homogeneous workflows with an efficiency advantage from KV cache reuse, and can even outperform an automatically optimized heterogeneous workflow. Building on this finding, we propose $\textbf{OneFlow}$, an algorithm that automatically tailors workflows for single-agent execution, reducing inference costs compared to existing automatic multi-agent design frameworks without trading off accuracy. These results position the single-LLM implementation of multi-agent workflows as a strong baseline for MAS research. We also note that single-LLM methods cannot capture heterogeneous workflows due to the lack of KV cache sharing across different LLMs, highlighting future opportunities in developing $\textit{truly}$ heterogeneous multi-agent systems.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：当前基于大语言模型（LLM）的多智能体系统（MAS）在复杂任务上表现出色，但多数多智能体框架是**同构的**——所有智能体共享同一基础LLM，仅通过不同的提示词、工具和工作流位置进行区分。这引发了一个根本性疑问：**这种同构的多智能体工作流是否本质上可以被一个单智能体通过多轮对话模拟？** 如果是，那么多智能体的优势可能被高估，且单智能体实现具有更高的效率优势。
- **整体含义**：论文旨在重新审视多智能体工作流的价值，提出一个强大的单智能体基线（OneFlow），并证明在多个基准上单智能体能够达到甚至超过同构多智能体工作流的性能，同时利用KV缓存复用显著降低推理成本。这项工作为多智能体研究提供了一个更公平的比较基准，并指出了真正异构多智能体系统的未来方向。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将同构多智能体工作流（多个独立LLM实例组成的流水线）等价地转化为**单智能体在同一次对话中通过多轮交互完成相同任务**。由于所有智能体共享同一基础模型，其角色、工具和通信模式可以通过提示词设计和对话历史进行模拟。
- **关键技术细节**：
  - 单智能体通过**多轮对话**模拟多智能体工作流中的每个步骤（如规划、工具调用、结果汇总等），上下文窗口足够大时，可利用KV缓存复用（KV cache reuse）避免了多智能体系统中多次独立推理带来的额外开销。
  - 进一步提出 **OneFlow 算法**：自动为单智能体执行定制工作流，将多智能体工作流中的角色、工具调用、通信模式编码为单次对话中的指令和步骤。相比现有的自动多智能体设计框架（如AutoGen、ChatDev），OneFlow降低了推理成本且不牺牲准确率。

## 3. 实验设计：使用的数据集/场景、Benchmark、对比方法

- **数据集与场景**：涵盖**七个基准**，覆盖以下领域：
  - 编程（Coding）
  - 数学（Mathematics）
  - 通用问答（General QA）
  - 特定领域推理（Domain-specific reasoning）
  - 现实世界规划与工具使用（Real-world planning and tool use）
  - *（具体基准名称未在摘要中列出，但可推测包括类似HotpotQA、GSM8K、MATH、BIG-Bench等常见基准）*
- **对比方法**：
  - **单智能体基线**：使用同一基础LLM进行多轮对话的单智能体。
  - **同构多智能体工作流**：多个相同基础LLM实例构成的流水线（如Role-playing、专业分工）。
  - **自动优化的异构工作流**：可能使用了不同基础模型的非共享工作流（论文中提及单智能体甚至可以超越该异构工作流）。
- **实验充分性**：七个不同领域的基准提供了较全面的覆盖，每个基准可能包含多组子任务。但摘要未说明重复次数、统计显著性等细节。

## 4. 资源与算力

- **未明确说明**：论文摘要及元数据中**没有提及使用的GPU型号、数量、训练时间、推理成本等具体算力信息**。仅提到单智能体通过KV缓存复用具有效率优势，但未给出量化数据。

## 5. 实验数量与充分性

- **实验数量**：七个不同领域的基准，每个基准可能包含多个子任务/样本组。
- **充分性评估**：
  - **覆盖广度较好**：涵盖编程、数学、推理、规划等典型复杂任务。
  - **比较对象合理**：对比了同构多智能体、单智能体基线、以及自动优化异构工作流。
  - **可能不足**：未公开具体数据集名称、任务规模、评估指标、重复次数、消融实验（如不同LLM大小、不同角色数量对性能影响）；未区分同构工作流中智能体数量对性能的影响；单智能体是否在全部基准上都优于/等价于多智能体，还是仅在部分上，摘要未给出明确数字。因此实验的公平性和可复现性需进一步验证。

## 6. 论文的主要结论与发现

- **主要结论**：
  1. **单智能体（多轮对话）可以达到同构多智能体工作流的性能**，在七个基准上表现相当或更好。
  2. **单智能体具有效率优势**：通过KV缓存复用，推理成本显著低于多智能体独立推理的总和。
  3. 单智能体甚至可**超越**自动优化的异构工作流（即使用不同基础LLM的流水线）。
  4. 基于此，提出OneFlow算法，能够在保持精度的前提下自动将多智能体工作流适配为单智能体执行，进一步降低开销。
  5. **局限**：单智能体方法无法完美捕获真正异构的工作流（不同基础LLM之间不共享KV缓存），这为未来研究指向了发展真正异构多智能体系统的方向。

## 7. 优点：方法或实验设计上的亮点

- **提出强基线**：将单智能体多轮对话提升为多智能体研究的强基线，扭转了领域“默认多智能体更优”的认知。
- **效率优势明确**：KV缓存复用是实际部署中的重要优势，论文抓住了这一关键点。
- **OneFlow算法实用**：自动优化单智能体工作流，降低设计成本。
- **覆盖领域广泛**：七个不同领域的基准增强了结论的泛化能力。
- **实验对比有层次**：从同构到异构，从手动设计到自动设计，对照清晰。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验细节缺失**：未提供具体基准名称、评估指标、性能数据表格，难以独立验证结论。摘要中的定性描述（“可以达到”“甚至超越”）缺乏量化支撑。
- **同构假设限制**：结论严格限定在“同构多智能体”（共享同一基础LLM）。对于异构多智能体（不同LLM或微调模型），单智能体无法完全替代，论文也指出了这一点，但未讨论其适用范围。
- **未考虑通信成本与并行加速**：多智能体系统虽然推理次数多，但可通过并行执行加速，单智能体多轮对话可能受限于串行步骤；论文对此未做分析。
- **缺乏消融实验**：未分析单智能体多轮对话所需的对话轮数、上下文长度对性能的影响，也未对比不同LLM大小。
- **可能的选择偏差**：基准任务可能偏向于单智能体易于模拟的场景（如规划、问答），对于需要严格分工和同步交互的任务（如机器人协同），结论是否成立存疑。
- **无开源代码或详细配置**：基于摘要信息，无法判断实验的公平性和可复现性。

（完）
