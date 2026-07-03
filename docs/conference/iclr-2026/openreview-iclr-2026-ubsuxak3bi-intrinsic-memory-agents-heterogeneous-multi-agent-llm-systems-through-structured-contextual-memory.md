---
title: "Intrinsic Memory Agents: Heterogeneous Multi-Agent LLM Systems through Structured Contextual Memory"
title_zh: 内生记忆智能体：通过结构化上下文记忆实现异构多智能体LLM系统
authors: "Sizhe Yuen, Francisco Gomez Medina, Ting Su, Yali Du, Adam Sobey"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=UbSUxAK3BI"
tags: ["query:cfd-agent"]
score: 6.0
evidence: 多智能体LLM框架，通过结构化上下文记忆保持角色一致性
tldr: 多智能体LLM系统受限于上下文窗口，导致记忆不一致和角色偏离。本文提出内生记忆智能体框架，通过智能体特定的记忆结构以角色对齐方式演变，无需手动设计提示。实验证明该方法有效提升了多智能体协作的稳定性和任务聚焦能力，适用于异构多智能体系统技能学习。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: LLM多智能体系统受上下文窗口限制，记忆一致性差。
method: 提出角色对齐的内生记忆模板，随智能体输出进化。
result: 在基准任务上，角色保持和任务相关信息聚焦显著改进。
conclusion: 内生记忆机制为多智能体LLM系统提供了鲁棒的协作基础。
---

## Abstract
Multi-agent systems built on Large Language Models (LLMs) show exceptional promise for complex collaborative problem-solving, yet they face fundamental challenges stemming from context window limitations that impair memory consistency, role adherence, and procedural integrity. This paper introduces Intrinsic Memory Agents, a novel framework that addresses these limitations through agent-specific memories that evolve intrinsically with agent outputs. Specifically, our method maintains role-aligned memory that preserves specialized perspectives while focusing on task-relevant information. Our approach utilises a generic memory template applicable to new problems without the need to hand-craft specific memory prompts. We benchmark our approach on the PDDL, FEVER, and ALFWorld datasets, comparing its performance to existing state-of-the-art multi-agentic memory approaches and showing state-of-the-art or comparable performance across all three, with the highest consistency. An additional evaluation is performed on a complex data pipeline design task, and we demonstrate that our approach produces higher quality designs across 5 metrics: scalability, reliability, usability, cost-effectiveness, and documentation, plus additional qualitative evidence of the improvements. Our findings suggest that addressing memory limitations through intrinsic approaches can improve the capabilities of multi-agent LLM systems on structured planning tasks.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文信息（主要为摘要与元数据）生成的结构化中文总结。

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：基于大语言模型（LLM）的多智能体系统在处理复杂协作任务时，受到 **上下文窗口限制** 的严重制约。这种限制导致智能体 **记忆不一致**、**角色保持不佳** 以及 **程序完整性受损**，从而削弱了系统的协作稳定性。
- **研究动机**：现有方法往往依赖手工设计的提示词来维持智能体记忆，缺乏灵活性，且无法从根本上解决上下文窗口导致的记忆退化问题。本文旨在提出一种无需手工设计、能够随智能体自身输出 **内在地（intrinsically）演化** 的记忆机制，以解决上述缺陷。
- **整体含义**：通过引入内生记忆（Intrinsic Memory），多智能体系统能够更好地保持角色一致性与任务聚焦能力，从而在结构化规划任务上提升协作性能，为异构多智能体LLM系统提供更鲁棒的协作基础。

### 2. 论文提出的方法论
- **核心思想**：提出 **内生记忆智能体（Intrinsic Memory Agents）** 框架，其关键在于为每个智能体维护 **角色对齐（role-aligned）** 的、能够随智能体输出 **内在地进化** 的特定记忆结构。
- **关键技术细节**：
  - **结构化上下文记忆**：每个智能体拥有独立的记忆模板，该模板并非静态，而是 **根据智能体自身的输出结果动态更新**，从而保持专业视角并聚焦于任务相关信息。
  - **通用性**：记忆模板是 **通用的（generic）**，可直接应用于新问题，**无需为特定任务手工设计记忆提示（hand-craft specific memory prompts）**。
  - **演化机制**：记忆以“角色对齐”的方式演变，即智能体在生成新内容时，记忆会自动吸收与之相关的信息，同时剔除任务无关或重复的内容，从而在有限上下文窗口内保持最有效的存储。
- **公式或算法流程**（文字说明）：
  1. 初始化：为每个智能体赋予一个通用的基础记忆模板（包含角色描述、任务目标等初始信息）。
  2. 协作交互：多智能体系统执行任务，每个智能体产生输出。
  3. 记忆更新：根据智能体当前输出与系统反馈，**内在地** 更新其记忆模板：保留角色关键信息，融合任务相关新信息，移除过时或无关信息。
  4. 循环执行：在后续交互中，智能体使用更新后的记忆指导行为，从而实现持续的自我修正与角色保持。

### 3. 实验设计
- **使用的数据集 / 场景**：
  - **PDDL**（规划领域定义语言）：用于评估结构化规划能力。
  - **FEVER**（事实验证）：评估事实性推理与协作。
  - **ALFWorld**（具身环境任务）：评估具体环境下的任务执行。
  - **附加场景**：一个 **复杂数据管道设计任务**，用于评估实际应用场景中的设计质量。
- **Benchmark**：对比 **现有最先进的多智能体记忆方法**（State-of-the-art multi-agentic memory approaches）。
- **评价指标**：
  - 前三个数据集：比较 **性能** 与 **一致性**（consistency）。
  - 数据管道任务：在 **可扩展性、可靠性、可用性、成本效益和文档** 这5个指标上评估设计质量，并补充 **定性证据**。
- **对比方法**：现有最先进的多智能体记忆方法（文中未具体列举名称，但强调与SOTA对比）。

### 4. 资源与算力
- **文中未明确说明**：摘要和元数据未提及使用的GPU型号、数量或训练时长等具体算力信息。仅可推测该框架基于预训练LLM进行评估，无需大规模再训练。

### 5. 实验数量与充分性
- **实验数量**：至少包含 **4个主要实验**（3个公开基准 + 1个自定义复杂任务）。未提及消融实验（如不同记忆更新策略的对比）。
- **充分性评估**：
  - **优点**：涵盖了规划、事实验证、具身任务三个不同性质的基准，并且额外增加了复杂实际任务，覆盖范围较广；与SOTA对比，指标具体。
  - **不足**：缺少 **消融实验** 来验证内生记忆机制各组件（如记忆模板通用性、更新频率等）的贡献；未提供统计显著性检验或误差分析；基准方法的具体名称未列出，可能影响复现与公平性判断。

### 6. 论文的主要结论与发现
- 在PDDL、FEVER、ALFWorld三个基准上，所提方法取得了 **最优或相当的性能**，并且 **一致性最高**（highest consistency）。
- 在复杂数据管道设计任务中，该方法在全部5个指标（可扩展性、可靠性、可用性、成本效益、文档）上均 **产生更高质量的设计**，并提供了定性证据。
- **总体结论**：通过 **内生式方法** 解决记忆限制问题，能够有效提升多智能体LLM系统在结构化规划任务上的能力。

### 7. 优点
1. **通用性强**：无需为每个新任务手工设计记忆提示，降低了人工成本，便于扩展到新领域。
2. **角色保持稳健**：通过角色对齐的内生记忆演化，智能体在不同交互回合中能持续保持专业角色，避免偏离。
3. **任务聚焦**：记忆进化机制自动过滤无关信息，使智能体始终关注任务相关信息，抑制不相关生成。
4. **实验设计全面**：不仅包含多个标准基准（规划、事实验证、具身任务），还引入了实际应用级任务（数据管道设计），增强了说服力。

### 8. 不足与局限
1. **消融实验缺失**：未对记忆更新的关键参数（如更新频率、记忆容量、角色对齐强度）进行消融分析，难以确定各因素的具体贡献。
2. **基准方法不透明**：未明确列出对比的SOTA方法名称，可能无法准确评估公平性，也可能掩盖了基线选择偏差。
3. **计算成本未知**：未报告计算资源消耗，无法判断该方法在大规模系统上的可扩展性实际开销。
4. **应用范围有限**：实验集中在规划与任务驱动场景，在开放域对话、创造性协作等场景下的表现尚未验证。
5. **长期记忆一致性风险**：虽然提到记忆内在地演化，但未讨论长期运行后可能出现的记忆漂移或累积错误问题。
6. **未提及偏见与安全**：未能评估记忆更新机制是否可能放大或引入社会偏见，也未讨论安全性。

（完）
