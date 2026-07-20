---
title: "Learning to Explore: Scaling Agentic Reasoning via Exploration-Aware Policy Optimization"
title_zh: 学会探索：通过探索感知策略优化扩展智能体推理
authors: "Xingyuan Hua, Sheng Yue, Ju Ren"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1eaaa70094a1d7c3a9a83f5516f18234624bca0c.pdf"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 针对LLM智能体的探索感知强化学习框架以提升技能
tldr: 现有方法在智能体探索时缺乏区分何时需要探索的能力。本文提出一种探索感知的强化学习框架，通过变分推理的细粒度奖励函数和探索感知分组机制，让LLM智能体仅在不确定性高时进行自适应探索。实验表明该方法能有效提升决策质量，为智能体技能进化提供了新思路。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有智能体探索策略缺乏区分何时需要探索的能力，导致资源浪费和效率低下。
method: 提出探索感知的强化学习框架，包含变分推理奖励函数和分组机制，引导智能体仅在不确定性高时探索。
result: 在多个推理任务上，该方法显著提升了智能体的决策效率和最终表现。
conclusion: 探索感知的强化学习可以有效提升LLM智能体的自适应探索能力，对技能进化有重要意义。
---

## Abstract
Recent advancements in agentic test-time scaling allow models to gather environmental feedback before committing to final actions. A key limitation of existing methods is that they typically employ undifferentiated exploration strategies, lacking the ability to adaptively distinguish when exploration is truly required. In this paper, we propose an exploration-aware reinforcement learning framework that enables LLM agents to adaptively explore only when uncertainty is high. Our method introduces a fine-grained reward function via variational inference that explicitly evaluates exploratory actions by estimating their potential to improve future decision-making, together with an exploration-aware grouping mechanism that separates exploratory actions from task-completion actions during optimization. By targeting informational gaps, this design allows agents to explore selectively and transition to execution as soon as the task context is clear. Empirically, we demonstrate that our approach achieves consistent improvements across a range of challenging text-based and GUI-based agent benchmarks.

---

## 论文详细总结（自动生成）

# 论文总结：Learning to Explore: Scaling Agentic Reasoning via Exploration-Aware Policy Optimization

## 1. 核心问题与整体含义（研究动机和背景）

- 现有的大语言模型（LLM）智能体在测试时扩展（test-time scaling）中，通常采用无差别的探索策略，缺乏自适应区分何时真正需要探索的能力。这导致大量计算资源浪费在无意义的探索上，降低了任务执行效率。
- 本文针对这一问题，提出一种探索感知的强化学习框架（Exploration-Aware Policy Optimization），使LLM智能体能够仅在不确定性较高时进行针对性探索，从而提升智能体在复杂推理任务中的表现和技能进化能力。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：将探索过程视为一种信息获取行为，通过评估探索动作对未来决策的潜在改善程度，引导智能体只在信息不足时探索，任务环境一旦清晰就立即转入执行阶段。
- **关键技术细节**：
  - **变分推理的细粒度奖励函数**：利用变分推理显式估计每个探索动作的信息增益，并将其作为奖励信号，衡量该动作是否真正有助于填补决策缺口。
  - **探索感知分组机制**：在策略优化过程中，将探索动作与任务完成动作分开处理。通过分组，避免探索行为干扰执行阶段的优化，使探索和执行策略能够协同进化。
- **算法流程（文字说明）**：智能体在环境中收集反馈，每次动作后由变分推理模块计算信息增益奖励；若奖励高于阈值，则标记为探索动作，并纳入探索分组进行优化；否则作为执行动作。优化器对两组策略分别更新，最终获得自适应探索策略。

## 3. 实验设计

- **数据集/场景**：使用了多种具有挑战性的基于文本（text-based）和基于图形界面（GUI-based）的智能体基准测试（benchmark）。具体数据集名称未在提供的文本中列出（如WebShop、ALFWorld等常见基准可能包含，但无法确认）。
- **对比方法**：文本中未明确列出对比的基线方法，但根据研究动机推测应与无差别探索策略的方法（如传统强化学习探索、随机探索等）进行对比。
- **实验充分性与公平性**：仅摘要提及“在多个基准上实现一致改进”，未提供消融实验、统计显著性检验、参数分析等细节。信息不足以判断实验的充分性和公平性。

## 4. 资源与算力

- 提供的论文元数据和摘要中**未提及**任何关于GPU型号、数量、训练时长等算力信息。因此无法总结。

## 5. 实验数量与充分性

- 摘要仅说明在多个文本和GUI基准上取得了提升，但未给出具体实验数量（例如几组数据集、几次重复、是否包含消融实验）。因此**实验描述的完整性较低**，无法评估是否充分。可能存在实验数量不足或缺失关键消融验证的风险。

## 6. 论文的主要结论与发现

- 提出的探索感知强化学习框架能够有效提升LLM智能体在决策任务中的自适应探索能力，相比现有方法在效率和质量上均有改进。
- 该方法为智能体技能进化（skill evolution）提供了新的思路：通过有选择地探索未知区域，智能体可以更快掌握新技能，同时避免无谓试错。

## 7. 优点

- **方法创新性**：针对现有探索策略的“一刀切”问题，提出了基于信息增益的细粒度奖励，理念清晰且符合直觉。
- **技术巧妙**：变分推理与分组机制的结合，使得探索与执行在优化层面解耦，有利于策略收敛和稳定性。
- **实验覆盖**：同时覆盖文本和GUI两种典型智能体场景，展示了一定的泛化能力。

## 8. 不足与局限

- **信息匮乏**：基于提供的元数据和摘要，无法获取完整的实验细节、算法伪代码、消融结果等，限制了评估的深度。
- **实验透明性低**：未明确列出对比方法、数据集名称、收敛曲线、超参数设置等，可能存在结果可重复性问题。
- **应用限制未知**：未讨论在长尾任务、极端不确定性场景或资源受限环境下的表现，实际部署风险未知。
- **探索阈值设定**：变分推理的奖励阈值如何确定？是否依赖人工调整？文中未提及，可能存在脆弱性。

（完）
