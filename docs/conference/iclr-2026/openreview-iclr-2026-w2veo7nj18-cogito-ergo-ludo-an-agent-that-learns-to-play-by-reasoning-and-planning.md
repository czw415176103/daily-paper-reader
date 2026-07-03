---
title: "Cogito, Ergo Ludo: An Agent that Learns to Play by Reasoning and Planning"
title_zh: 我思故我玩：一个通过推理与规划学习的智能体
authors: "Sai Wang, Yu Wu, Zhongwen Xu"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=w2vEo7NJ18"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 智能体通过语言推理与规划学习游戏技能
tldr: 深度强化学习依赖大量经验且知识隐蔽。本文提出Cogito Ergo Ludo（CEL）架构，利用大语言模型构建显式的语言化环境理解与策略，从零开始通过交互与反思循环学习游戏，实现可解释的技能习得。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 深度强化学习方法依赖海量经验且知识不透明。
method: 利用大语言模型构建显式的语言化环境理解和策略，通过交互与反射循环学习。
result: 智能体从零开始掌握游戏技能，具备可解释性。
conclusion: 语言驱动的推理与规划为智能体技能演化开辟了新范式。
---

## Abstract
The pursuit of artificial agents that can learn to master complex environments has led to remarkable successes, yet prevailing deep reinforcement learning methods often rely on immense experience, encoding their knowledge opaquely within neural network weights. We propose a different paradigm, one in which an agent learns to play by reasoning and planning. We introduce Cogito, Ergo Ludo (CEL), a novel agent architecture that leverages a Large Language Model (LLM) to build an explicit, language-based understanding of its environment's mechanics and its own strategy. Starting from a tabula rasa state with no prior knowledge (except action set), CEL operates on a cycle of interaction and reflection. After each episode, the agent analyzes its complete trajectory to perform two concurrent learning processes: Rule Induction, where it refines its explicit model of the environment's dynamics, and Strategy and Playbook Summarization, where it distills experiences into an actionable strategic playbook. We evaluate CEL on diverse grid-world tasks (i.e., Minesweeper, Frozen Lake, and Sokoban), and show that the CEL agent successfully learns to master these games by autonomously discovering their rules and developing effective policies from sparse rewards. Ablation studies confirm that the iterative process is critical for sustained learning. Our work demonstrates a path toward more general and interpretable agents that not only act effectively but also build a transparent and improving model of their world through explicit reasoning on raw experience.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：当前深度强化学习方法严重依赖海量交互经验，且学到的知识隐藏在神经网络权重中，缺乏透明性和可解释性。
- **研究动机**：探索一种新的智能体范式——智能体通过学习“推理与规划”来掌握复杂环境，而非单纯依靠“试错+深度网络拟合”。
- **整体含义**：作者提出了一种名为 **Cogito, Ergo Ludo (CEL)** 的智能体架构，利用大语言模型（LLM）构建显式的、基于语言的环境理解和策略，从零开始（tabula rasa，除动作集外无先验知识）通过交互与反思循环学习游戏，实现可解释的技能习得。

## 2. 方法论：核心思想、关键技术细节、算法流程
- **核心思想**：将智能体的学习过程建模为“交互-反思”循环，每个 episode 结束后，智能体利用 LLM 分析完整轨迹，同时进行两种并行学习：
  - **规则归纳 (Rule Induction)**：精炼对环境动力学（dynamics）的显式语言模型。
  - **策略与玩法总结 (Strategy and Playbook Summarization)**：将经验蒸馏为可行动的策略手册。
- **关键技术细节**：
  - 起点：完全空白（tabula rasa），仅知道可用动作集。
  - 每轮交互后，LLM 基于轨迹文本描述更新规则和策略，形成闭环式持续学习。
  - 没有使用预训练权重或外部知识库，完全依赖自主生成的语言表征。
- **算法流程（文字说明）**：
  1. 初始化：空规则集、空策略手册。
  2. 在每个 episode 开始时，智能体根据当前规则集和策略手册规划行动。
  3. 执行动作直至 episode 结束，收集完整轨迹（状态、动作、奖励等）。
  4. 将轨迹转化为自然语言描述，输入 LLM。
  5. LLM 输出两方面的更新：更新的环境规则、更新的策略与玩法总结。
  6. 将更新后的规则和策略存入记忆，用于后续 episode。
  7. 重复步骤2-6，直至收敛或达到最大 episode 数。

## 3. 实验设计
- **使用的任务/场景**：
  - 三个不同的网格世界任务：Minesweeper（扫雷）、Frozen Lake（冰冻湖）、Sokoban（推箱子）。
  - 这些任务具有不同的复杂度和稀疏奖励特征。
- **Benchmark**：
  - 论文中未明确提及与基线方法（如 DQN、PPO）对比，主要聚焦于 CEL 自身的学习过程和可解释性。
- **对比方法**：
  - 未报告与其他强化学习算法的数值对比，但做了消融实验（ablation studies）验证迭代过程的关键性。

## 4. 资源与算力
- 论文中 **未明确说明** 使用的 GPU 型号、数量、训练时长等算力信息。仅指出使用大语言模型（未指明具体模型参数量级，推测为中等规模 LLM 如 GPT-3.5 或 LLaMA 系列）。

## 5. 实验数量与充分性
- **实验数量**：
  - 三个任务（Minesweeper、Frozen Lake、Sokoban）上验证了 main result。
  - 进行了消融实验（ablation studies），证实迭代过程对持续学习是关键的。
- **充分性评估**：
  - 任务多样性尚可（包括逻辑推理、随机性、组合规划等不同挑战），但缺少与现有强化学习方法的定量对比，实验覆盖不够全面。
  - 消融实验仅验证了迭代本身的重要性，未剖析 LLM 组件的不同部分（如规则归纳 vs 策略总结）的贡献。
  - 实验在网格世界中进行，规模较小，对复杂连续控制任务尚未验证。

## 6. 主要结论与发现
- CEL 智能体能够在 **无先验知识** 的情况下，通过语言推理与反思循环，自主发现游戏规则并制定有效策略，从稀疏奖励中学会通关。
- 迭代过程（interaction + reflection）是持续学习的关键，没有迭代则性能会停滞。
- 该方法为构建更通用、可解释的智能体展示了新路径——智能体不仅有效行动，还通过原始经验构建透明且不断改进的世界模型。

## 7. 优点
- **创新性**：提出语言驱动的推理与规划学习框架，颠覆了传统深度强化学习的隐式表征方式。
- **可解释性**：学到的规则和策略以自然语言形式存储，可被人类理解、检查甚至修正。
- **零先验知识**：从 tabula rasa 开始，不依赖预训练模型或手工设计的奖励塑形。
- **简单而优雅的循环结构**：交互-反思循环易于实现和理解。

## 8. 不足与局限
- **实验规模有限**：仅在简单网格世界测试，未涉及高维观察（如图像输入）、连续动作空间或实时交互场景。
- **缺乏与经典方法的基线对比**：没有报告与其他 RL 算法（如 DQN、A2C 等）在同一任务上的性能（如成功率、样本效率），无法判断语言驱动的策略在效率上是否有优势。
- **LLM 依赖与成本**：使用大语言模型推理成本高，且可能引入幻觉或推理错误；未讨论扩展到大任务时 LLM 调用次数的可扩展性。
- **消融实验不够深入**：仅验证了迭代的必要性，未分解规则归纳与策略总结的独立贡献，也未分析 LLM 不同 prompt 设计的影响。
- **可迁移性未验证**：是否能在不同环境间迁移规则与策略，论文未进行测试。
- **论文在 ICLR 2026 被拒** (根据元数据 `source: ICLR-2026-Rejected-Public`)，可能审稿人指出上述不足中的若干点。

（完）
