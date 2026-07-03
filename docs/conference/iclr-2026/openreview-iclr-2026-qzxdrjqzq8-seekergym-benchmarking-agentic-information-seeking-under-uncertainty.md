---
title: "SeekerGym: Benchmarking Agentic Information Seeking under Uncertainty"
title_zh: SeekerGym：不确定性下的智能体信息搜索基准
authors: "Remy Kim, Minseung Lee, Shuo Li, Osbert Bastani"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=QzXdRJqZq8"
tags: ["query:cfd-agent"]
score: 6.0
evidence: LLM智能体信息搜索基准，包含学习组件
tldr: 论文提出SeekerGym模块化环境，用于评估LLM智能体在不确定性下的信息搜索能力，并设计SeekerAgent智能体。该智能体采用信念结构化流程，包括元反思实现跨示例学习。实验表明，该方法显著提升了智能体在维基百科重建和文献查找任务中的信息整合能力，为提升智能体信息搜索技能提供了新的评估和训练平台。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 当前AI智能体在不确定环境中自主识别和整合相关信息的能力不足，且缺乏专门评估此类技能的基准。
method: 构建模块化环境SeekerGym，定义重构维基百科页面和查找计算机科学文献两项复杂任务，并设计SeekerAgent采用元反思信念结构进行跨示例学习。
result: SeekerAgent在两项任务中均优于基线，且元反思机制有效提升了信息检索的准确性和完整性。
conclusion: 信息搜索能力评估应关注自主性而非端到端任务表现，SeekerGym为此提供了有效基准。
---

## Abstract
Effective information seeking is a prerequisite for AI agents, yet current systems often fail to autonomously identify, retrieve, and integrate relevant context. We propose SeekerGym, a modular environment for evaluating LLM agents on information-seeking tasks. Unlike prior benchmarks that focus on end-to-end task performance, SeekerGym evaluates agentic information seeking capabilities in two complex tasks: reconstructing Wikipedia pages and finding related literature for computer science survey papers. Furthermore, we design an information seeking agent called SeekerAgent, which employs various belief structuring pipelines including meta-reflection for cross-example learning. Through comprehensive experiments using SeekerGym, we evaluate several design choices for information seeking agents. We find that SeekerAgent improve recall by as much as 68% compared to frontier models.

---

## 论文详细总结（自动生成）

好的，根据您提供的论文元数据及摘要，我将对论文《SeekerGym: Benchmarking Agentic Information Seeking under Uncertainty》进行结构化总结。

### 1. 论文的核心问题与整体含义（研究动机和背景）

当前AI智能体在不确定性环境中，自主识别、检索和整合相关信息的能力严重不足。例如，大型语言模型（LLM）驱动的智能体往往难以在复杂任务中主动寻找缺失的上下文，而是依赖内部知识或简单检索。现有基准大多关注端到端任务表现（如问答准确率），缺乏专门针对“智能体信息搜索能力”的评估，特别是对其“自主性”的衡量。因此，论文提出了**SeekerGym**——一个模块化环境，用于评估LLM智能体在不确定性下的信息搜索能力；并设计了**SeekerAgent**，一种采用元反思（meta-reflection）信念结构化流程的智能体，以提升跨示例学习能力。

### 2. 论文提出的方法论：核心思想、关键技术细节

**核心思想**：将信息搜索能力从端到端任务表现中解耦，通过构建需要主动检索和整合信息的复杂任务来评估智能体。同时，提出一种结构化信念管理方法，使智能体能基于历史经验进行反思和改进。

**关键技术细节**：
- **SeekerGym环境**：模块化设计，支持多种信息搜索场景。定义了两项复杂任务：
  - 任务一：**维基百科页面重建**（给定部分片段，智能体需搜索缺失内容并重构完整页面）。
  - 任务二：**计算机科学文献查找**（针对某综述论文，智能体需找到相关文献并整合）。
- **SeekerAgent设计**：
  - 采用**信念结构化管线**（belief structuring pipelines），将信息状态组织为结构化信念（如当前已知、缺失、不确定等）。
  - 包含**元反思**（meta-reflection）机制：在完成一个示例后，智能体会反思自己的搜索策略和错误，并将经验应用于后续示例（跨示例学习）。这类似于一种在线学习/元学习范式。
- **核心流程**（文字描述）：
  1. 智能体接收部分信息（如维基百科片段或综述题目）。
  2. 智能体通过查询（如调用搜索引擎或知识库）获取新信息。
  3. 智能体根据新信息更新信念结构。
  4. 在每一步，智能体决定下一步行动（继续搜索或整合并输出）。
  5. 每完成一个任务示例后，智能体进行元反思，记录成功策略和失败模式，用于改进后续任务策略。

（注：原文未给出具体公式或算法伪代码，但上述流程基于摘要和tldr推断。）

### 3. 实验设计：使用的数据集/场景、基准、对比方法

- **数据集/场景**：
  - **维基百科重建任务**：从维基百科中选取部分页面，隐藏部分内容，要求智能体通过搜索恢复完整页面。
  - **计算机科学文献查找任务**：给定计算机科学综述论文标题/摘要，要求智能体查找并列出相关文献（模拟文献综述过程）。
- **基准（Benchmark）**：SeekerGym本身作为评估基准。
- **对比方法**：
  - 未明确列出所有基线，但提到与“前沿模型”（frontier models，如GPT-4、Claude等）对比。
  - 可能包括无搜索策略的LLM直接回答、简单检索增强生成（RAG）、以及未使用元反思的智能体变体。
- **评估指标**：主要使用**召回率（Recall）** 评估信息检索的完整性（是否找全所需信息）。此外可能涉及准确性、任务完成度等。

### 4. 资源与算力

论文元数据**未明确说明**使用了多少算力（GPU型号、数量、训练时长等）。仅提到实验在SeekerGym环境中进行，可能涉及在LLM API上调用（如GPT-4），但未披露具体算力消耗。

### 5. 实验数量与充分性

- **实验数量**：根据摘要，进行了“全面实验”（comprehensive experiments），评估了多种信息搜索智能体的设计选择。具体组数未列出，但应包含：
  - 主要任务上的性能对比（维基百科重建、文献查找）。
  - 消融实验：可能包括去除元反思、不同信念结构、不同搜索策略变体。
  - 与多个基线对比（至少包括前沿模型）。
- **充分性评估**：
  - **优点**：任务设计具有挑战性且现实（重建、文献查找），能较好反映信息搜索能力。元反思机制的跨示例学习设计新颖，消融实验有助于验证其效果。
  - **不足**：任务种类有限（仅两个），覆盖范围可能不够广泛（如未包括更开放域或实时信息搜索）。未明确说明统计显著性或重复次数。对比基线可能不够全面（例如未包括专门的信息检索系统）。由于未公开完整实验日志，对公平性（如是否使用相同基础模型、检索工具）尚难完全确认。

### 6. 论文的主要结论与发现

- **主要结论**：信息搜索能力评估应关注**自主性**（能否主动搜寻并整合信息），而非单纯端到端任务表现。SeekerGym为此提供了有效基准。
- **关键发现**：SeekerAgent通过采用信念结构化管道和元反思机制，相比于前沿模型（如GPT-4等），在召回率上提升了**68%**（即信息检索的完整性显著提高），证明了结构化信念管理与跨示例学习对信息搜索任务的有效性。

### 7. 优点

- **方法亮点**：
  - 提出模块化环境**SeekerGym**，将评估焦点从任务结果转向智能体的信息搜索过程，具有创新性。
  - **元反思机制**：使智能体能从以往经验中学习，避免了每次任务从头开始，更贴近人类学习方式。
  - 信念结构化管线能够显式表示信息状态，有助于提升搜索策略的决策透明度。
- **实验设计亮点**：
  - 任务选取兼顾复杂性与现实意义（维基百科重建、学术文献查找）。
  - 对比了前沿模型并报告了显著的改进幅度（68%召回率提升），结果清晰有力。

### 8. 不足与局限

- **实验覆盖不足**：
  - 仅包含两个任务场景，未涉及跨领域、多模态或实时动态信息（如对话、网页导航）等更复杂的搜索任务，限制了泛化性结论。
  - 未详细展示在不同难度级别、信息噪声程度下的表现，可能存在偏差（如维基百科重建依赖结构化知识库）。
  - 未考虑搜索成本（如API调用次数、时间开销）或鲁棒性（如对抗性误导）。
- **方法局限**：
  - 元反思机制可能增加计算开销和延迟，且依赖于任务样本的多样性。若训练示例少或偏差大，跨示例学习可能无效甚至有害。
  - 信念结构化管线设计依赖人工预设（如信念分类），可能无法覆盖所有不确定性形式。
- **资源与算力未披露**：无法评估方法的可复现性及实际部署可行性。
- **公平性风险**：对比的前沿模型可能未针对信息搜索任务进行优化，且未公开用于评估的具体模型版本和检索工具，可能存在不公平比较。

（完）
