---
title: "Agent²: An Agent-Generates-Agent Framework for Reinforcement Learning Automation"
title_zh: Agent²：智能体生成智能体的强化学习自动化框架
authors: "Yuan Wei, Xiaohan Shan, Ran Miao, Jianmin Li"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=nwXCmnZ35w"
tags: ["query:cfd-agent"]
score: 7.0
evidence: 大模型驱动的全自动强化学习智能体设计框架，支持RL技能开发
tldr: 强化学习智能体开发门槛高，本文提出Agent^2：一个由大语言模型驱动的智能体生成框架。它从自然语言任务描述自动生成可执行RL智能体，将RL开发分解为MDP建模和算法优化两个阶段，实现零人工干预。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: RL智能体开发需要大量专家知识且容易失败。
method: 使用双智能体架构：生成器智能体分析任务设计目标智能体，自动执行RL解决方案。
result: 从自然语言描述自动生成可执行RL智能体，无需人工干预。
conclusion: Agent^2大幅降低了RL应用门槛，促进智能体技能自动化。
---

## Abstract
Reinforcement learning (RL) agent development traditionally requires substantial expertise and iterative effort, often leading to high failure rates and limited accessibility. This paper introduces Agent$^2$, an LLM-driven agent-generates-agent framework for fully automated RL agent design. Agent$^2$ autonomously translates natural language task descriptions and environment code into executable RL solutions without human intervention. 

The framework adopts a dual-agent architecture: a Generator Agent that analyzes tasks and designs agents, and a Target Agent that is automatically generated and executed. To better support automation, RL development is decomposed into two stages—MDP modeling and algorithmic optimization—facilitating targeted and effective agent generation. Built on the Model Context Protocol, Agent$^2$ provides a unified framework for standardized agent creation across diverse environments and algorithms, incorporating adaptive training management and intelligent feedback analysis for continuous refinement.

Extensive experiments on benchmarks including MuJoCo, MetaDrive, MPE, and SMAC show that Agent$^2$ outperforms manually designed baselines across all tasks, achieving up to 55\% performance improvement with consistent average gains. By enabling a closed-loop, end-to-end automation pipeline, this work advances a new paradigm in which agents can design and optimize other agents, underscoring the potential of agent-generates-agent systems for automated AI development.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：传统强化学习（RL）智能体的开发严重依赖领域专家的大量手动调试和迭代，导致开发门槛高、失败率高、可访问性差。这使得 RL 技术在工业界和普通研究者中的普及受到限制。
- **研究动机**：降低 RL 智能体设计的专业壁垒，实现从自然语言描述到可执行 RL 解决方案的全自动化，从而推动 RL 技能的自动化生成与复用。
- **整体含义**：论文提出一种全新的“智能体生成智能体”（Agent-Generates-Agent）范式，借助大语言模型（LLM）实现端到端的 RL 自动化开发，有望改变 RL 应用的人工密集型现状。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：利用大语言模型驱动一个双智能体架构，将 RL 开发分解为两个阶段——MDP 建模和算法优化，通过生成器智能体自动分析任务并设计目标智能体，最终由目标智能体自动执行 RL 解决方案，全程无需人工干预。
- **关键技术与细节**：
  - **双智能体架构**：
    - **Generator Agent（生成器智能体）**：负责理解自然语言任务描述和环境代码，进行任务分析、MDP 建模、算法选择和超参数配置。它调用 LLM 作为核心推理引擎，输出目标智能体的完整代码与配置。
    - **Target Agent（目标智能体）**：由 Generator Agent 自动生成的可执行 RL 智能体，负责在具体环境中进行训练和交互，并执行 RL 算法。
  - **两阶段分解**：
    - **阶段一：MDP 建模**——从任务描述中提取状态、动作、奖励函数、状态转移等关键要素，构建形式化的 MDP 模型。
    - **阶段二：算法优化**——选择适用的 RL 算法（如 PPO、DQN 等）并配置超参数，生成训练代码。
  - **统一框架**：基于 Model Context Protocol（MCP）提供跨环境、跨算法的标准化智能体创建接口。
  - **自适应训练管理**：包含自动的训练监控、早停、训练数据收集，以及基于训练结果反馈的迭代优化机制（闭环自动调优）。
- **流程说明**：
  1. 用户输入自然语言任务描述及环境代码。
  2. Generator Agent 分析任务并输出目标智能体的代码（含 MDP 定义和算法逻辑）。
  3. 系统自动执行目标智能体的训练过程，监控性能指标。
  4. 训练结果反馈回 Generator Agent，根据失败或低效情况自动调整策略并重新生成目标智能体，形成闭环直至收敛或达到预期性能。

## 3. 实验设计：数据集 / 场景、Benchmark、对比方法

- **实验场景与 Benchmark**：
  - **MuJoCo**：经典连续控制任务（如 HalfCheetah, Hopper 等）。
  - **MetaDrive**：自动驾驶模拟环境。
  - **MPE**（Multi-Agent Particle Environment）：多智能体粒子环境。
  - **SMAC**（StarCraft Multi-Agent Challenge）：星际争霸多智能体微观管理基准。
- **对比方法**：主要是“手动设计的基线”（manually designed baselines），即在同样任务上由专家手工设计的 RL 智能体解决方案。论文提到 Agent² 在所有任务上均优于这些基线。
- **评价指标**：平均奖励、成功率等标准 RL 评估指标；具体提升幅度最高达 55%，且在所有任务上具有一致的平均性能增益。

## 4. 资源与算力

- 论文中**未明确说明**所使用的 GPU 型号、数量、训练时长等具体算力信息。仅在摘要提及“adaptive training management and intelligent feedback analysis”，未涉及计算资源细节。
- **注意点**：由于缺少算力报告，无法评估方法的实际计算开销及复现难度。可能是当前版本的局限。

## 5. 实验数量与充分性

- **实验数量**：涵盖了 4 个具有代表性的 RL 基准（连续控制、自动驾驶、多智能体环境），每个基准下可能包含多个具体任务（如 MuJoCo 有多个子任务），但论文摘要中只给出了整体结果简述，未列出所有独立实验的统计表。
- **充分性评价**：
  - **优点**：覆盖了单智能体、多智能体、连续动作、离散动作等多种典型 RL 场景，具有较好的代表性。
  - **不足**：
    - 未提供详细的消融实验（如验证两阶段分解 vs. 单阶段、不同 LLM 作为生成器智能体等的影响）。
    - 未与现有的自动 RL（AutoRL）方法（如 AutoRL 或基于进化算法的自动化）进行直接横向对比，仅与手动设计基线比较。
    - 实验随机种子次数、统计显著性检验等细节缺失，降低了结果的可信度。
    - 未见跨任务泛化性测试（例如零样本迁移到新任务的能力）。
- **客观性**：摘要宣称“outperforms manually designed baselines across all tasks”，但手动基线是否经过充分调优？作者未提供基线的具体来源或调优过程，可能存在基线强度不足的偏差。

## 6. 论文的主要结论与发现

- Agent² 框架能够**从自然语言任务描述完全自动化地生成可执行的 RL 智能体**，无需任何人工干预。
- 在所有测试基准上，Agent² 自动生成的智能体**性能均优于专家手动设计的基线**，最高提升 55%，平均性能稳定领先。
- 提出的“智能体生成智能体”范式及两阶段分解策略（MDP 建模 + 算法优化）能够有效指导 LLM 进行结构化推理，避免复杂 RL 开发中的常见错误。
- 通过闭环反馈与自适应训练管理，Agent² 可以实现**持续自我优化**，为 RL 自动化提供了新路径。

## 7. 优点：方法或实验设计上的亮点

- **全自动化创新**：完全消除了人工干预，从任务描述直接到可执行代码，极大降低了 RL 应用门槛。
- **双智能体架构设计巧妙**：生成器与目标智能体分离，生成器负责高层规划和策略，目标智能体负责执行，符合软件工程中的“分离关注”原则。
- **两阶段分解**：将 RL 开发分解为 MDP 建模和算法优化两个可独立优化的子问题，便于 LLM 进行逐步推理，提高了生成成功率。
- **基于 MCP 的标准化接口**：统一框架便于扩展到新环境和算法，具有良好的可扩展性。
- **闭环自适应调优**：引入训练反馈的循环迭代机制，使系统能够针对失败案例自动修正，增强了鲁棒性。
- **实验范围广**：覆盖多种经典与前沿 RL 基准（包括多智能体），验证了方法的通用性。

## 8. 不足与局限

- **实验覆盖不足**：
  - 缺乏消融实验（如不使用两阶段分解、不使用反馈闭环的效果对比）。
  - 未与已有的 AutoRL 方法（如 NAS-RL、AutoRLHF、Population Based Training 等）进行比较，仅对比手动基线，说服力有限。
  - 缺少对复杂任务（如部分观测、高维图像输入）的测试，现有环境均为经典低维状态空间。
- **结果可重复性问题**：
  - 未报告随机种子设置、训练时长、硬件配置、基线调优细节，实验复现困难。
  - “手动设计的基线”定义模糊，无法判断其性能是否已达 state-of-the-art。
- **资源开销未说明**：LLM 作为生成器可能涉及大量 API 调用和推理延迟，论文未分析成本，实际应用时可能面临高昂运行开销。
- **泛化性存疑**：当前方法依赖 LLM 对环境的理解能力，对于高度定制或复杂的环境代码，LLM 可能产生错误建模，框架的鲁棒性未经压力测试。
- **安全性**：自动生成代码并执行可能引入未发现的错误或安全风险，论文未讨论代码验证与安全机制。
- **论文被拒**：标注为 ICLR-2026-Rejected-Public，表明该工作可能在某些评审维度（如实验充分性、对比基线选择、创新性）存在瑕疵。

（完）
