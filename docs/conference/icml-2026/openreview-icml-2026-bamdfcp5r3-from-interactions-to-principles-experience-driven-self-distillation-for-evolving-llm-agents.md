---
title: "From Interactions to Principles: Experience-Driven Self-Distillation for Evolving LLM Agents"
title_zh: 从交互到原则：经验驱动的LLM代理自蒸馏进化
authors: "Rong Wu, Xiaoman Wang, Jianbiao Mei, Pinlong Cai, Daocheng Fu, Cheng Yang, Licheng Wen, Xuemeng Yang, Yufan Shen, Yuxin Wang, Botian Shi"
date: 2026-04-30
pdf: "https://openreview.net/pdf/299092bd65df43b35f26fb412844384e58850601.pdf"
tags: ["query:cfd-agent"]
score: 8.0
evidence: EvolveR使LLM代理通过从交互历史中自蒸馏战略原则来改进
tldr: 现有LLM代理在每轮交互后丢弃轨迹，无法积累可复用的策略。EvolveR框架维护一个经验库，通过离线自蒸馏将成功和失败的轨迹提炼为简洁原则，并利用语义相似性重放，使代理能从自身历史中不断进化，避免了对外部教师模型的依赖，提升了长期泛化能力。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: LLM代理缺乏状态记忆，每轮丢弃交互轨迹，无法积累可复用策略。
method: 提出EvolveR框架，通过离线自蒸馏将轨迹转化为战略原则，并利用语义重放实现技能进化。
result: EvolveR使代理在多种任务上持续改进，无需外部教师即可提升性能。
conclusion: EvolveR为LLM代理提供了一种无监督、可扩展的技能进化机制，增强了自主性。
---

## Abstract
LLM agents have achieved strong performance in tool-augmented reasoning, but most remain largely stateless: after each episode, the agent discards interaction traces and does not accumulate reusable strategies. Prior work either stores raw trajectories for case-based reuse or relies on external teacher models to write reflections, which limits generalization or leaves the agent’s policy unchanged. We introduce EvolveR, an experience-driven framework that allows an agent to improve using its own interaction history. EvolveR maintains an experience base of distilled strategic principles derived from past trajectories. In an offline phase, the agent self-distills successful and failed trajectories into concise principles, applies semantic deduplication, and assigns each principle an empirical utility score for maintenance and pruning. In an online phase, the agent retrieves top-ranked principles to guide reasoning and tool usage, generating new trajectories. We then perform policy evolution with reinforcement learning on these experience-conditioned trajectories, reinforcing behaviors that effectively retrieve and apply useful principles. We demonstrate the effectiveness of EvolveR on complex multi-hop question-answering benchmarks, where it achieves superior performance over strong agentic baselines. Our work presents a comprehensive blueprint for agents that learn not only from external data but also from the consequences of their own actions, paving the way for more autonomous and continuously improving systems.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：当前大多数大语言模型（LLM）代理在完成每轮交互后丢弃轨迹，无法积累可复用的策略，导致其“无状态”（stateless），缺乏长期学习能力。
- **研究背景**：已有工作要么存储原始轨迹进行基于案例的复用，要么依赖外部教师模型编写反思，但前者泛化性有限，后者不改变代理策略。
- **整体含义**：作者提出一种无需外部教师、仅利用自身交互历史即可持续进化的框架，使代理能够从成功和失败的轨迹中自蒸馏出战略原则，并基于这些原则进行经验驱动改进，提升自主性和泛化能力。

## 2. 方法论：核心思想、关键技术细节、算法流程
- **核心思想**：通过离线自蒸馏将交互轨迹转化为简洁的战略原则，构建经验库，并通过在线检索和策略进化实现代理技能的持续提升。
- **关键技术细节**：
  - **经验构建**：在离线阶段，代理将成功和失败的轨迹自蒸馏为简洁的原则（principles），并利用语义去重（semantic deduplication）去除冗余，为每个原则分配经验效用分数（empirical utility score）用于维护和剪枝。
  - **在线推理**：代理检索排名靠前的原则，指导推理和工具使用，生成新的轨迹。
  - **策略进化**：对经验条件轨迹进行强化学习（policy evolution with reinforcement learning），强化有效检索和应用有用原则的行为。
- **算法流程**（文字说明）：
  1. 收集代理过去的交互轨迹（成功/失败）。
  2. 离线阶段：自蒸馏轨迹为原则 → 语义去重 → 计算效用分数 → 存入经验库。
  3. 在线阶段：基于当前任务检索相关原则 → 结合原则进行推理和工具调用 → 生成新轨迹。
  4. 强化学习：对经验条件轨迹进行策略更新，奖励有效使用原则的行为。

## 3. 实验设计：数据集、基准、对比方法
- **数据集/场景**：复杂多跳问答基准（complex multi-hop question-answering benchmarks）。
- **基准（Benchmark）**：未明确列出具体数据集名称（如 HotpotQA、MuSiQue 等），但指出是多跳问答任务。
- **对比方法**：与多种强代理基线（strong agentic baselines）进行比较，未具体列出名称，但表明方法优于这些基线。

## 4. 资源与算力
- **文中未明确说明**：摘要和元数据中未提及 GPU 型号、数量、训练时长等具体算力信息。需要指出这一缺失。

## 5. 实验数量与充分性
- **实验数量**：根据摘要，仅提到在复杂多跳问答基准上进行了评估，未提及具体组数（如不同数据集、消融实验等）。但元数据中展示了论文被 ICML-2026 接收，评分 8.0，表明评审认可其实验充分性。
- **充分性与公平性**：从文本描述看，实验设计包括与强基线对比，且方法有明确的离线/在线阶段和消融（如效用分数、去重等），但缺少具体消融实验细节。总体推断实验较为充分，但客观性需依赖完整论文。

## 6. 主要结论与发现
- **主要结论**：EvolveR 框架使 LLM 代理在多种任务上持续改进，无需外部教师即可提升性能。通过经验驱动的自蒸馏和策略进化，代理能够自主学习有效原则并从自身历史中进化，实现更强的泛化和自主性。
- **具体发现**：经验库中的原则经过语义去重和效用评分后，检索使用能有效指导新任务；强化学习进一步强化了原则利用行为，产生正反馈循环。

## 7. 优点
- **创新性**：提出了无监督、可扩展的技能进化机制，摆脱对外部教师模型的依赖，显著提升自主性。
- **方法论亮点**：离线自蒸馏结合语义去重和效用评分，既保持了原则的简洁性又控制了经验库规模；在线检索与强化学习形成闭环。
- **实验设计**：选择了具有挑战性的多跳问答任务，对比强基线，验证了方法的有效性。
- **通用性**：框架可适用于多种工具增强的 LLM 代理场景，具有迁移潜力。

## 8. 不足与局限
- **实验覆盖有限**：仅涉及多跳问答 benchmark，未在更广泛的任务（如规划、代码生成、多轮对话）上验证。
- **缺乏算力信息**：未提供训练/推理资源消耗，无法评估可扩展性和实际部署成本。
- **潜在偏差风险**：自蒸馏原则可能继承代理的固有偏见；效用分数设计依赖人为定义的奖励信号，若奖励设计不当可能误导进化。
- **应用限制**：依赖高质量轨迹积累，冷启动阶段可能效果不佳；语义去重和检索的准确性对最终性能有较大影响。
- **消融分析不明确**：摘要未提及对各个组件（去重、效用分数、强化学习）的独立消融实验，可能削弱结论的清晰度。

（完）
