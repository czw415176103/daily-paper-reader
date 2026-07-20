---
title: "AgentConductor: Topology Evolution for Multi-Agent Competition-Level Code Generation"
title_zh: AgentConductor：多智能体竞赛级代码生成的拓扑进化
authors: "Siyu Wang, Ruotian Lu, Zhihao Yang, Yuchao Wang, Yanzhou Zhang, Lei Xu, Qimin Xu, Guojun Yin, Cailian Chen, Xinping Guan"
date: 2026-04-30
pdf: "https://openreview.net/pdf/6ffc4b8b771eaf3318c96846c00a057cf5739573.pdf"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 强化学习优化多智能体拓扑以提升技能
tldr: 本文提出AgentConductor，通过强化学习优化多智能体系统中的通信拓扑结构，使其适应任务难度并利用执行反馈实时调整。在竞赛级代码生成任务中，该方法减少了冗余通信，提升了协作效率，显著优于固定拓扑的基线方法。该方法为多智能体技能提升提供了新范式。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有多智能体系统使用固定拓扑结构，无法适应任务难度变化，导致通信冗余和性能瓶颈。
method: 提出基于强化学习和LLM编排器的框架，动态进化多智能体拓扑，根据执行反馈调整通信图。
result: 在竞赛级代码生成任务中显著提升了代码质量，减少了不必要的通信开销。
conclusion: 强化学习驱动的拓扑进化有效提升了多智能体系统的技能表现和效率。
---

## Abstract
Large language model(LLM)-driven multi-agent systems(MAS) coordinate specialized agents through predefined topologies and show promise for complex tasks such as competition-level code generation. Recent studies demonstrate that carefully designed multi-agent workflows and communication graphs can significantly improve code generation performance by leveraging collaborative reasoning. However, existing methods neither adapt topology density to task difficulty nor refine the topology within an instance using execution feedback, which leads to redundant communication and performance bottlenecks. To address these issues, we propose AgentConductor: a reinforcement learning-optimized MAS with an LLM-based orchestrator agent as its core, which enables end-to-end feedback-driven dynamic generation of interaction topologies. For each query, AgentConductor infers agent roles and task difficulty, then constructs a task-adapted, density-aware layered directed acyclic graph(DAG) topology, underpinned by two key innovations. First, we design a novel topology density function to quantify communication-aware multi-agent interactions. Second, we adopt difficulty interval partitioning to avoid excessive pruning for precise topological density upper bound measurement per difficulty level and finer-grained control. Across five code datasets, AgentConductor outperforms the strongest baseline by up to 14.6\% in pass@1, with 13\% lower topology density and 68\% lower token cost.

---

## 论文详细总结（自动生成）

# AgentConductor 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现有多智能体系统（MAS）在竞赛级代码生成等复杂任务中，通常采用**预定义的固定拓扑结构**（如链式、树状或全连接图），无法根据任务难度动态调整通信密度，也无法在执行过程中利用反馈对拓扑进行细粒度改进。这导致了**冗余通信**（简单任务中过多交互浪费资源）和**性能瓶颈**（困难任务中缺乏有效协作）。
- **核心问题**：如何让多智能体系统在代码生成任务中，根据任务难度和实时执行反馈，**自适应地进化通信拓扑**，以最小化冗余并最大化协作效率。
- **整体含义**：本文提出一种**强化学习优化的动态拓扑生成框架**，将多智能体系统的通信结构设计从静态预设变为动态自适应，为提升多智能体协作能力提供了新范式。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：以**LLM驱动的编排器（orchestrator）智能体**为核心，结合**强化学习**，实现端到端、反馈驱动的交互拓扑动态生成。
- **关键技术细节**：
  - **角色与难度推断**：对于每个查询（query），编排器首先推断所需智能体角色和任务难度等级。
  - **拓扑构建**：生成一个**任务自适应、密度感知的分层有向无环图（DAG）拓扑**。该拓扑根据难度区间划分进行密度调节。
  - **两大创新点**：
    1. **拓扑密度函数**：量化通信感知的多智能体交互密度，用于衡量拓扑的稀疏/密集程度。
    2. **难度区间划分**：通过将难度划分为区间，对每个难度级别设置精确的拓扑密度上限，避免过度剪枝，实现更细粒度的控制。
  - **学习与优化**：利用强化学习（RL）根据执行反馈（如代码通过率）优化拓扑生成策略，使得生成的拓扑既能适应任务难度，又能随执行结果动态调整。
- **算法流程（文字说明）**：
  1. 输入代码竞赛问题 → 编排器智能体分析任务，推断所需角色（如解题者、测试者、调试者等）和难度。
  2. 基于推断结果，从预定义的难度区间密度池中选择合适的拓扑密度上界，通过LLM生成初始DAG拓扑。
  3. 多智能体在该拓扑下协作生成代码，并收集执行反馈（如编译错误、测试结果）。
  4. 强化学习模型根据反馈更新拓扑生成策略（奖励信号为代码质量指标，如pass@1）。
  5. 在下一轮或下一个实例中，拓扑自动进化，更加适配任务。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：在**5个代码数据集**上进行评估（具体数据集名称未在摘要中列出，但涵盖竞赛级代码生成）。
- **基准（Benchmark）**：多智能体竞赛级代码生成任务。
- **对比方法**：包括**最强的基线方法**（未在摘要中具体命名，但声称在pass@1上比最强基线提升14.6%）。固定拓扑结构的MAS（如全连接、链式等）作为对比基线。

## 4. 资源与算力

- **论文未明确说明**：摘要和元数据中没有提及GPU型号、数量、训练时长等算力信息。因此无法总结具体资源消耗，但可推断模型使用LLM（如GPT系列）作为基础，强化学习训练需要一定计算资源。

## 5. 实验数量与充分性

- **实验数量**：在**5个数据集**上进行了主实验，并且出现了**消融实验**（从元数据“evidence: 强化学习优化多智能体拓扑以提升技能”可推测很可能包含消融实验，但摘要未列出具体数量）。至少包含主性能对比和拓扑密度、token成本等指标。
- **充分性与公平性**：
  - 指标覆盖**pass@1**（代码一次通过率）、**拓扑密度**（反映通信冗余）和**token成本**（计算开销），对比全面。
  - 使用了多项指标证明其不仅提升性能，还减少开销，评价体系较客观。
  - 但摘要未提供对比方法的详细设置，也未给出统计显著性检验，公平性需依赖论文原文补充。

## 6. 论文的主要结论与发现

- **性能提升**：AgentConductor在pass@1上**比最强基线提升14.6%**，同时拓扑密度降低13%，token成本降低68%。
- **效率与效果的平衡**：动态自适应拓扑在提升代码生成质量的同时显著降低了通信和计算开销。
- **验证拓扑进化有效性**：强化学习驱动的拓扑密度调节和难度区间划分是关键，证明了动态进化优于固定拓扑。

## 7. 优点：方法或实验设计上的亮点

- **方法论亮点**：
  - 首次将**拓扑密度函数**和**难度区间划分**引入多智能体协作，实现细粒度控制。
  - 采用**端到端强化学习**优化拓扑生成，使系统能从执行反馈中持续学习，避免人工预设的限制。
- **实验设计亮点**：
  - 不仅考察生成质量（pass@1），还量化了成本（token成本）和冗余（拓扑密度），多维度评估更全面。
  - 数据集覆盖5个代码生成场景，结果具有一定泛化性。

## 8. 不足与局限

- **实验覆盖**：数据集均为代码生成领域，未在非代码任务（如数学推理、对话）上验证，通用性有待检验。
- **偏差风险**：可能依赖于特定LLM（如GPT-4）的能力，若更换基础模型结果可能变化；强化学习奖励函数设计可能有主观性。
- **应用限制**：需要额外训练一个强化学习模型，增加了系统复杂度；对于小规模任务，训练开销可能不划算。
- **信息缺失**：论文摘要部分未提供对比方法的名称、消融实验的具体细节、离线/在线评估方式等，限制了对实验充分性的全面判断。

（完）
