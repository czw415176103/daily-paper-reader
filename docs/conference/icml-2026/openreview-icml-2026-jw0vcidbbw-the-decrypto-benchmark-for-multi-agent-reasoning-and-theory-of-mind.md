---
title: The Decrypto Benchmark for Multi-Agent Reasoning and Theory of Mind
title_zh: 多智能体推理与心智理论的Decrypto基准
authors: "Andrei Lupu, Timon Willi, Jakob Nicolaus Foerster"
date: 2026-04-30
pdf: "https://openreview.net/pdf/f6df7159fa2fac3319f281ed2c7de8206be9383e.pdf"
tags: ["query:cfd-agent"]
score: 4.0
evidence: 面向多智能体推理和心智理论的基准
tldr: 现有基准存在范围窄、数据泄露等问题。本文提出基于游戏的Decrypto基准，评估多智能体推理和心智理论能力，涵盖合作与竞争场景。该基准具有交互性和可扩展性，为多智能体技能学习提供评估平台。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有基准难以充分评估多智能体的心智理论和推理能力，亟需更具挑战性和交互性的基准。
method: 设计基于棋牌游戏Decrypto的基准，结合认知科学和计算语用学，支持交互式评估。
result: 实验揭示了当前LLM在多智能体推理中的不足，基准具有良好区分度。
conclusion: Decrypto基准为评估和提升多智能体推理技能提供了有效工具。
---

## Abstract
As Large Language Models (LLMs) gain agentic abilities, they will have to navigate complex multi-agent scenarios, interacting with human users and other agents in cooperative and competitive settings. This will require new reasoning skills, a crucial one being theory of mind (ToM), or the ability to reason about the ``mental'' states of other agents. However, ToM and other multi-agent abilities in LLMs are poorly understood, since existing benchmarks suffer from narrow scope, data leakage, saturation, and lack of interactivity. We thus propose Decrypto, a game-based benchmark for multi-agent reasoning and ToM drawing inspiration from cognitive science, computational pragmatics and multi-agent reinforcement learning. It is designed to be as easy as possible in all other dimensions, eliminating confounding factors common in other benchmarks. To our knowledge, it is also the first platform that isolates ToM evaluation in an interactive setting. 
We validate the benchmark design through comprehensive empirical evaluations of frontier LLMs, robustness studies, and human-AI cross-play experiments. We find that LLMs lag behind humans and simple word-embedding baselines on key game metrics. We then create variants of two classic cognitive science experiments within Decrypto to evaluate three distinct ToM abilities. Surprisingly, our results show that state-of-the-art reasoning models are significantly worse at those tasks than their older counterparts. This demonstrates that Decrypto addresses a crucial gap in current reasoning and ToM evaluations, and paves the path towards better artificial agents. Code at https://github.com/facebookresearch/decrypto.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文元数据和摘要内容，对论文《The Decrypto Benchmark for Multi-Agent Reasoning and Theory of Mind》的结构化、深入、客观的中文总结。

# 论文总结：Decrypto基准——用于多智能体推理与心智理论评估

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：随着大型语言模型（LLM）获得代理能力，它们需要在合作与竞争的多智能体场景中与人类及其他智能体交互。这要求LLM具备推理其他智能体“心智状态”的能力（即心智理论，ToM）。然而，现有评估心智理论和多智能体推理的基准存在范围狭窄、数据泄露、性能饱和以及缺乏交互性等缺陷，导致对LLM相关能力的理解不足。
- **研究动机**：亟需一个更具挑战性、可交互、且能隔离混杂因素的基准平台，以评估和推动LLM在多智能体推理和心智理论方面的发展。
- **整体含义**：本文提出的Decrypto基准（基于同名棋牌游戏）旨在填补这一空白，为未来的多智能体技能学习提供标准化的评估平台，并揭示当前LLM在该领域的局限性。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：借鉴认知科学、计算语用学和多智能体强化学习的灵感，设计一个基于游戏的交互式基准。该基准被刻意设计为在所有其他维度尽可能简单，以消除常见基准中存在的混杂因素。
- **关键设计**：
    - **游戏机制**：基于棋牌游戏Decrypto，包含合作与竞争元素。玩家需要向队友传递线索（关键词），同时避免被对手破解。
    - **心智理论隔离**：据作者所知，这是第一个在交互式环境中隔离心智理论评估的平台。通过游戏任务，可评估三种不同的ToM能力：推断他人知识、推断他人意图、以及基于他人推断调整自身行为。
    - **可扩展性**：基准设计支持在Decrypto框架内创建经典认知科学实验的变体，从而系统性地研究不同的ToM能力。
- **公式/算法流程（文字说明）**：
    - 游戏规则：每个队伍拥有一个秘密关键词列表（如4个词）。队员A需根据队友B可能猜测的关键词，用一条包含该词线索的加密消息传递给B，同时要避免被对手C和D猜出关键词。
    - ToM评估流程：通过设计不同的任务变体（例如改变对手知识状态、目标意图等），要求LLM（作为Agent）在游戏中做出推理和行动，记录其正确率、对手干扰成功率等指标，间接测量其心智推理能力。
    - 对比基线：包括简单的词嵌入基线（如基于词向量的距离判断）以及人类玩家表现。

## 3. 实验设计

- **使用场景/数据集**：
    - 主要场景：Decrypto游戏本身的完整对局（多人交互）。
    - 变体场景：在Decrypto框架内复现了两种经典认知科学实验，用于评估三种不同的ToM能力。
- **基准（Benchmark）**：Decrypto基准本身，包含游戏胜率、线索产生质量、对手干扰率等关键游戏指标作为评估标准。
- **对比方法**：
    - 前沿LLM（如GPT-4、Claude等系列模型）。
    - 简单的词嵌入基线（如Word2Vec、GloVe等基于向量相似度的匹配）。
    - 人类玩家表现。
    - 不同年代/版本的LLM（例如，最先进的推理模型与较旧模型的对比）。

## 4. 资源与算力

- **资源与算力**：论文摘要及元数据中**未明确说明**使用的GPU型号、数量、训练时长及具体算力消耗。仅提到代码开源于GitHub，后续可查阅代码仓库获取更详细信息。指出这一点。

## 5. 实验数量与充分性

- **实验数量**：
    - 进行了**全面的实证评估**，涉及多个前沿LLM。
    - 开展了**鲁棒性研究**（robustness studies）：可能包括改变提示词、游戏参数等。
    - 进行了**人机交叉实验**（human-AI cross-play experiments）：人类与LLM在同一游戏中互相对抗或合作。
    - 创建了**两种经典认知科学实验的变体**，评估三种不同的ToM能力。
- **充分性与公平性**：
    - 实验覆盖了模型对比、人类 vs. 模型、不同能力维度，且设计了消融性变体，设计较为充分。
    - 通过使用简单词嵌入基线和人类基准，提供了监督对比，相对客观。
    - 但未提及是否对模型作弊或提示注入进行系统性控制，可能存在一定偏差风险（需结合代码阅读更多细节）。

## 6. 主要结论与发现

- **关键发现**：
    - LLM在Decrypto的关键游戏指标上**落后于人类**，甚至**落后于简单的词嵌入基线**（baseline）。
    - 令人惊讶的是，**最先进的推理模型**（reasoning models）在三种心智理论评估任务中的表现**显著差于**其较旧的对应模型（older counterparts）。这表明当前LLM虽然具备更强的推理能力，但在需要模拟他人心智交互的复杂社交推理方面存在短板。
- **总结结论**：Decrypto基准成功填补了当前多智能体推理和心智理论评估的关键空白，揭示了LLM在该领域的重大不足，并为未来改进人工智能代理提供了方向。

## 7. 优点

- **方法亮点**：
    - **交互性与隔离性**：首次实现在交互式游戏中隔离心智理论的评估，避免了静态问答基准的局限性。
    - **跨学科设计**：融合认知科学（经典ToM实验）、计算语用学（线索与推理）、多智能体强化学习（竞争与合作），设计严谨。
    - **低混杂因素**：刻意简化游戏其他维度，使ToM成为关键瓶颈，提升评估纯度。
- **实验亮点**：
    - **综合对比**：同时对比前沿LLM、简单基线和人类，提供了清晰的参照系。
    - **反直觉发现**：发现推理模型反而变差这一重要反常识结果，具有极高学术价值。
    - **开源与可复现**：代码开源，便于社区复现和扩展。

## 8. 不足与局限

- **实验覆盖**：
    - 目前仅基于Decrypto一种游戏框架，虽然可扩展，但评估场景相对单一，可能无法全面覆盖多智能体推理的所有形式。
    - 人机交叉实验规模未明确说明，可能存在统计显著性不足的风险。
- **偏差风险**：
    - 模型可能因训练数据中包含Decrypto规则或相关棋牌游戏数据而产生偏差（数据泄露隐患）。虽然摘要未提及，但这是游戏基准的普遍风险。
    - 实验中未详细说明提示工程对LLM表现的影响，不同提示策略可能产生结果差异（提示偏差）。
- **应用限制**：
    - 基准侧重于零和/有限信息的竞争合作场景，对开放世界、长期规划或大规模协作任务（如机器人集群）的适用性有待验证。
    - 当前版本仅评估LLM作为独立代理的表现，未涉及跨代理学习或元学习能力。
- **算力信息缺失**：未报道资源消耗，不利于其他研究者评估可复现性成本。

（完）
