---
title: "EvoMAS: Evolutionary Generation of Multi-Agent Systems"
title_zh: EvoMAS：多智能体系统的进化生成
authors: "Yuntong Hu, Yuting Zhang, Matthew Trager, Yi Zhang, Shuo Yang, Wei Xia, Stefano Soatto"
date: 2026-04-30
pdf: "https://openreview.net/pdf/410b88190222509514e11e3aeed3145745decae0.pdf"
tags: ["query:cfd-agent"]
score: 7.0
evidence: 进化生成多智能体系统配置，增强智能体设计与适应性
tldr: 针对LLM多智能体系统设计劳动密集且脆弱的问题，提出EvoMAS方法，将MAS生成视为结构化配置生成问题。通过进化算法在配置空间中进行选择、变异和交叉，自动生成高效的多智能体系统架构。该方法避免了代码生成带来的可执行性问题，同时保持了架构的灵活性。实验表明EvoMAS生成的MAS在多个复杂任务上超越了手工设计的方法，推动了智能体技能的自动进化。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有LLM多智能体系统设计依赖手工或代码生成，效率低且鲁棒性差。
method: 提出EvoMAS，将MAS架构生成建模为结构化配置的进化优化，通过变异和交叉搜索最优配置。
result: 在推理、规划等任务上，EvoMAS自动生成的MAS优于手工设计和代码生成方法。
conclusion: EvoMAS为多智能体系统的自动化和技能进化提供了高效、灵活的进化框架。
---

## Abstract
Large language model (LLM)-based multi-agent systems (MAS) show strong promise for complex reasoning, planning, and tool-augmented tasks, but designing effective MAS architectures remains labor-intensive, brittle, and hard to generalize. Existing automatic MAS generation methods either rely on code generation, which often leads to executability and robustness failures, or impose rigid architectural templates that limit expressiveness and adaptability. We propose Evolutionary Generation of Multi-Agent Systems (EvoMAS), which formulates MAS generation as structured configuration generation. EvoMAS performs evolutionary generation in configuration space. Specifically, EvoMAS selects initial configurations from a pool, applies feedback-conditioned mutation and crossover guided by execution traces, and iteratively refines both the candidate pool and an experience memory. We evaluate EvoMAS on diverse benchmarks, including BBEH, SWE-Bench, and WorkBench, covering reasoning, software engineering, and tool-use tasks. EvoMAS consistently improves task performance over both human-designed MAS and prior automatic MAS generation methods, while producing generated systems with higher executability and runtime robustness. EvoMAS outperforms the agent evolution method EvoAgent by +10.5 points on BBEH reasoning and +7.1 points on WorkBench. With Claude-4.5-Sonnet, EvoMAS also reaches 79.1% on SWE-Bench-Verified, matching the top of the leaderboard. Code is available at https://github.com/amazon-science/EvoMAS

---

## 论文详细总结（自动生成）

# EvoMAS：多智能体系统的进化生成 —— 论文详细总结

## 1. 核心问题与整体含义（研究动机与背景）

- **研究动机**：基于大语言模型（LLM）的多智能体系统（MAS）在复杂推理、规划、工具使用等任务中展现出巨大潜力，但设计有效的MAS架构目前依赖大量人工劳动，过程脆弱且难以泛化。现有自动化MAS生成方法要么依赖代码生成（导致可执行性和鲁棒性问题），要么采用刚性架构模板（限制表达能力和适应性）。
- **核心问题**：如何自动、高效地生成健壮且高性能的多智能体系统架构，避免手工设计的低效和代码生成的不稳定性。
- **整体含义**：提出一种将MAS生成视为**结构化配置空间中的进化优化**问题的新范式，通过进化算法自动搜索最优配置，实现智能体技能的自动进化。

## 2. 方法论：核心思想、关键技术细节与算法流程

- **核心思想**：EvoMAS将MAS生成建模为**结构化配置生成**，在配置空间中执行进化操作（选择、变异、交叉），而不是直接生成代码或使用固定模板。
- **关键技术细节**：
  - **配置空间**：定义MAS的组件（如智能体数量、角色、工具、协作策略等）为可配置参数，形成结构化搜索空间。
  - **进化流程**：
    1. **初始化**：从候选池中选取初始配置。
    2. **反馈条件变异与交叉**：基于执行轨迹（execution traces）的反馈，指导变异和交叉操作，使生成的新配置更可能高效。
    3. **迭代优化**：迭代更新候选池和经验记忆（experience memory），积累历史有效配置信息，加速搜索。
  - **无需代码生成**：直接操作配置，避免代码可执行性问题，同时保持架构灵活性。
- **算法流程（文字描述）**：
  - 输入：任务描述、初始配置池、进化代数、种群大小。
  - 循环：对当前种群中的每个配置，执行MAS并收集运行轨迹；根据轨迹质量评分选择优秀配置；对配置进行反馈引导的变异和交叉，生成子代；将子代加入候选池并更新经验记忆；重复直到满足停止条件。
  - 输出：最优配置对应的MAS。

## 3. 实验设计：数据集、基准与对比方法

- **数据集/场景**：
  - **BBEH**（推理任务）：覆盖多步推理、常识推理等。
  - **SWE-Bench**（软件工程任务）：自动化代码修复/生成。
  - **WorkBench**（工具使用任务）：多工具组合调度。
- **基准**：使用标准 benchmarks 的验证集或测试集（如SWE-Bench-Verified）。
- **对比方法**：
  - 手工设计的MAS（人工架构）。
  - 自动MAS生成方法：EvoAgent（智能体进化方法）作为主要基线。
  - 其他先前自动MAS生成方法（未具体列出）。
- **主要结果**：
  - 在BBEH上，EvoMAS比EvoAgent提升**+10.5个点**。
  - 在WorkBench上，提升**+7.1个点**。
  - 使用Claude-4.5-Sonnet时，在SWE-Bench-Verified上达到**79.1%**，匹配排行榜顶尖水平。

## 4. 资源与算力

- **文中未明确说明** 使用的GPU型号、数量、训练时长等具体算力信息。
- 仅提及代码开源，但未讨论计算成本。需要指出：论文未提供算力细节，可能存在实验可复现性方面的不足。

## 5. 实验数量与充分性

- **实验数量**：覆盖三个不同领域的 benchmark（推理、软件工程、工具使用），每个 benchmark 包含多个子任务。对比了至少两种基线方法（手工设计、EvoAgent），并在最强基线上进行了比较。
- **充分性评价**：
  - **优点**：覆盖多个任务类型，验证了方法的通用性；使用了公认的公开基准，结果具有客观性。
  - **不足**：缺乏消融实验的详细描述（如进化算子贡献、配置空间大小影响、经验记忆效果等），也未比较与代码生成方法的直接性能（仅提及代码生成方法可执行性差）。实验在单一大模型（Claude-4.5-Sonnet）上进行了最强验证，但缺乏跨模型泛化实验（如GPT-4等）。
- **总体**：实验设计较为合理，但细节披露不足，充分性中等偏上。

## 6. 主要结论与发现

- **EvoMAS能自动生成高效MAS**，在多个复杂任务上超越手工设计和现有自动生成方法。
- **配置空间进化优于代码生成**：避免了可执行性失败，提升了鲁棒性，同时保持架构表达力。
- **可扩展性**：在SWE-Bench上搭配最强闭源模型可达排行榜顶尖水平，说明方法可充分利用基座模型能力。
- **技能自动进化**：通过进化迭代，MAS能够自适应地优化协作策略与角色分配。

## 7. 优点

- **创新性**：首次将MAS架构设计形式化为结构化配置空间的进化优化问题，而非代码生成，思路新颖。
- **实用性**：生成的MAS具有高可执行性（无需调试代码）和高鲁棒性，适合实际部署。
- **可迁移性**：不依赖特定LLM，可与各种基座模型搭配使用。
- **清晰流程**：反馈条件变异/交叉利用执行轨迹，使搜索更具方向性，效率更高。
- **实验覆盖广**：涵盖推理、工程、工具三大类任务，证明了方法的通用性。

## 8. 不足与局限

- **实验披露不足**：
  - 未公开算力消耗（GPU/时间），难以评估实际成本。
  - 未提供详细的消融研究（如变异率、交叉率、种群大小对性能的影响）。
  - 未对比与纯代码生成方法（如ChatDev等）的任务完成率与生成时间。
- **偏差风险**：仅在Claude-4.5-Sonnet上报告了最佳结果，可能存在模型偏好偏差；其他模型（如GPT-4o、Llama）上的表现未知。
- **应用限制**：
  - 配置空间的设计依赖于领域知识，若任务场景复杂，配置空间可能巨大，进化搜索效率可能下降。
  - 进化过程需要执行MAS获取轨迹，对于需要外部工具调用的场景，执行成本较高。
  - 没有讨论安全性或一致性（如有害输出、幻觉）问题。
- **客观性**：对比EvoAgent的方法细节未充分给出，且EvoAgent可能也是同一研究社区的方法，存在潜在偏向。

（完）
