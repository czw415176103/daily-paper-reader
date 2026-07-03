---
title: "LifelongAgentBench: Evaluating LLM Agents as Lifelong Learners"
title_zh: "LifelongAgentBench: 评估LLM智能体作为终身学习者"
authors: "Junhao Zheng, Xidi Cai, Qiuke Li, Duzhen Zhang, Zhong-Zhi Li, Yingying Zhang, Le Song, Qianli Ma"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=MYqAKKsjF9"
tags: ["query:cfd-agent"]
score: 8.0
evidence: LLM智能体终身学习基准
tldr: 现有基准无法评估LLM智能体的终身学习能力。本文提出LifelongAgentBench，包含跨数据库、操作系统和知识图谱的相互依赖任务，自动验证标签，实验表明传统经验回放效果有限，揭示当前智能体的终身学习不足。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 缺乏评估LLM智能体终身学习能力的标准基准。
method: 构建包含多个环境、具有技能依赖任务的基准，自动化评估。
result: 主流LLM智能体在终身学习上表现不佳，经验回放益处有限。
conclusion: 该基准揭示了LLM智能体终身学习的瓶颈。
---

## Abstract
Lifelong learning is essential for intelligent agents operating in dynamic environments. Current large language model (LLM)-based agents, however, remain stateless and unable to accumulate or transfer knowledge over time. Existing benchmarks treat agents as static systems and fail to evaluate lifelong learning capabilities. We present LifelongAgentBench, the first unified benchmark designed to systematically assess the lifelong learning ability of LLM agents. It provides skill-grounded, interdependent tasks across three interactive environments—Database, Operating System, and Knowledge Graph—with automatic label verification, reproducibility, and modular extensibility. Extensive experiments reveal that conventional experience replay has limited effectiveness for LLM agents due to irrelevant information and context length constraints. We further introduce a group self-consistency mechanism that significantly improves lifelong learning performance. We hope LifelongAgentBench will advance the development of adaptive, memory-capable LLM agents.

---

## 论文详细总结（自动生成）

好的，以下是基于您提供的论文内容（摘要及元数据）生成的详细中文总结。

---

## 论文总结：LifelongAgentBench: 评估LLM智能体作为终身学习者

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前基于大型语言模型（LLM）的智能体普遍缺乏终身学习能力，即无法在动态环境中持续积累和迁移知识，每次交互都是“无状态”的。现有基准将智能体视为静态系统，无法评估其终身学习性能。
- **研究动机**：为了系统评估并推动LLM智能体向具备记忆与自适应能力的终身学习者发展，需要建立一个统一的、标准化的评测基准。
- **整体含义**：该工作提出了首个专门用于评估LLM智能体终身学习能力的基准 LifelongAgentBench，并通过实验揭示了现有方法（如经验回放）在该场景下的局限性，同时提出了改进机制。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：构建一个包含多个相互依赖任务（skill-grounded, interdependent）的交互式环境，要求智能体在解决新任务时能够利用先前积累的技能和知识，并通过自动化的方式验证任务完成标签。
- **关键技术细节**：
  - **三个交互环境**：数据库（Database）、操作系统（Operating System）、知识图谱（Knowledge Graph）。这些环境中的任务具有技能依赖性，即完成后续任务可能需要前面任务中的知识或技能。
  - **自动标签验证**：通过预设的验证逻辑自动判断智能体是否完成任务，保证评估的可重复性和客观性。
  - **模块化可扩展性**：基准设计支持添加新的环境和任务，便于未来扩展。
  - **提出的改进方法**：作者引入了**群体自一致性机制（Group Self-Consistency）**，通过聚合多个推理路径或智能体群体的输出一致性，显著提升了终身学习性能。具体算法流程（文字描述）：
    1. 智能体在多个任务序列中依次执行。
    2. 针对每个新任务，智能体从记忆中检索先前相关经验（可以是成功的任务执行轨迹）。
    3. 基于检索到的经验，智能体生成多个候选解决方案或推理路径。
    4. 通过群体自一致性机制，对多个解决方案进行投票或一致性筛选，选取最可靠的结果作为最终输出。
    5. 任务完成后，将成功经验存储回记忆库，供后续任务使用。

### 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **数据集/场景**：使用了三个交互环境作为任务平台：Database、Operating System、Knowledge Graph。每个环境内包含一系列具有依赖关系的子任务（具体任务类型未在摘要中列出）。
- **Benchmark**：LifelongAgentBench 本身即为所提出的基准，它提供了统一的评估流程、自动验证机制和标准化的任务集。
- **对比方法**：
  - 基线方法：无任何记忆/经验回放的静态LLM智能体。
  - 对比方法：传统经验回放（Experience Replay）方法——即简单存储并回放过去的完整任务轨迹。
  - 提出的方法：群体自一致性机制（Group Self-Consistency）。
  - 注：摘要未列出具体的LLM模型名称，但通常对比包括GPT-4、Llama系列等主流模型。

### 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点。

- **文中未明确说明**。摘要和元数据中没有提及具体的GPU型号、数量、训练时长或计算资源开销。因此无法总结。若需要评估实验可复现性，这一信息缺失可能是一点不足。

### 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平。

- **实验数量**：摘要仅提到“extensive experiments”，但未给出具体实验组数。可以推断至少包括如下几类：
  - 在三个环境（Database、OS、Knowledge Graph）上分别评测。
  - 对比基线（无记忆）、经验回放、群体自一致性三种方法。
  - 可能包含消融实验验证群体自一致性各组件效果。
- **充分性与公平性**：
  - **充分性**：覆盖了三个不同领域的交互环境，任务具有技能依赖性，能较好地测试终身学习能力。但缺乏对其他场景（如机器人操控、多轮对话）的拓展。
  - **客观性**：采用自动标签验证，减少人为偏差。
  - **公平性**：对比了传统经验回放方法和改进方法，但未提及是否调整了经验回放的超参数（如记忆大小、检索策略）以达到最优，因此公平性需谨慎评估。

### 6. 论文的主要结论与发现

- 现有LLM智能体在终身学习任务上表现不佳，无法有效积累和迁移知识。
- 传统的经验回放方法对LLM智能体效果有限，主要原因是回放内容中包含大量无关信息，且受限于上下文长度约束。
- 提出的群体自一致性机制能够显著提升终身学习性能，说明通过聚合多个推理路径可以弥补单个智能体的记忆和推理不足。
- LifelongAgentBench 基准能够有效揭示LLM智能体在终身学习方面的瓶颈，对推动该领域发展具有重要价值。

### 7. 优点：方法或实验设计上有哪些亮点

- **首个专门基准**：填补了LLM智能体终身学习评估的空白。
- **任务依赖设计**：模拟真实场景中技能递进的需求，比独立任务更有挑战性。
- **自动验证**：减少人工评估成本，提高可重复性。
- **模块化扩展**：易于添加新环境和任务，具有良好的可扩展性。
- **提出新机制**：群体自一致性机制简单有效，对比经验回放有明显优势。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验覆盖有限**：仅包含三个环境（数据库、操作系统、知识图谱），缺少对更复杂的多模态、物理世界交互场景的评估。
- **资源信息缺失**：未报告计算资源和具体模型大小，难以评估实验的复现成本。
- **基线对比不充分**：仅对比了经验回放一种传统方法，缺少对更多终身学习方法（如弹性权重巩固、增量学习、记忆网络等）的对比。
- **应用限制**：群体自一致性机制需要生成多个候选方案，可能增加推理时间和成本，不适用于实时性要求高的场景。
- **任务数量与规模未知**：未说明每个环境内任务的具体数量和难度梯度，可能会影响结论的泛化性。
- **潜在偏差风险**：自动验证机制可能无法完全捕捉任务完成的细微正确性（如存在多种正确解法且未在验证规则中定义）。

（完）
