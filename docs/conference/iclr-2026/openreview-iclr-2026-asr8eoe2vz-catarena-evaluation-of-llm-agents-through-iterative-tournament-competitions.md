---
title: "CATArena:  Evaluation of LLM Agents Through Iterative Tournament Competitions"
title_zh: CATArena：通过迭代锦标赛竞争评估LLM智能体
authors: "Lingyue Fu, Xin Ding, Yaoming Zhu, Shao Zhang, Lin Qiu, Weiwen Liu, Weinan Zhang, Xuezhi Cao, Xunliang Cai, Jiaxin Ding, Yong Yu"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=aSr8eoe2vz"
tags: ["query:cfd-agent"]
score: 8.0
evidence: LLM智能体评估框架，强调学习能力和同伴学习以促进技能进化
tldr: 论文提出CATArena，一个基于迭代锦标赛竞争的LLM智能体评估框架。该框架强调学习能力（自我改进和同伴学习）是智能体进化的核心驱动力，通过反复交互和反馈让智能体优化策略。实验显示该评估方法能更全面反映智能体的进化潜力，避免了传统固定场景基准的分数饱和和评估偏差问题。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有智能体基准仅评估固定场景下的终端性能，无法衡量智能体的学习能力，且存在分数饱和和依赖专家标注问题。
method: 设计迭代竞争同伴学习框架，让智能体在重复对战中通过反馈优化策略，系统评估其自我改进和同伴学习能力。
result: 相比静态基准，CATArena能更好区分不同智能体的长期进化潜力，且无需人工标注。
conclusion: 学习能力应作为智能体评估的核心维度，CATArena为此提供了有效的动态评估方案。
---

## Abstract
Large Language Model (LLM) agents have evolved from basic text generation to autonomously completing complex tasks through interaction with external tools. However, current benchmarks mainly assess end-to-end performance in fixed scenarios, restricting evaluation to specific skills and suffering from score saturation and growing dependence on expert annotation as agent capabilities improve. In this work, we emphasize the importance of learning ability, including both self-improvement and peer-learning, as a core driver for agent evolution toward human-level intelligence. We propose an iterative, competitive peer-learning framework, which allows agents to refine and optimize their strategies through repeated interactions and feedback, thereby systematically evaluating their learning capabilities. To address the score saturation issue in current benchmarks, we introduce CATArena, a tournament-style evaluation platform featuring four diverse board and card games with open-ended scoring. By providing tasks without explicit upper score limits, CATArena enables continuous and dynamic evaluation of rapidly advancing agent capabilities. Experimental results and analyses involving both minimal and commercial code agents demonstrate that CATArena provides reliable, stable, and scalable benchmarking for core agent abilities, particularly learning ability and strategy coding.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

当前LLM智能体已从基础文本生成进化到能够通过外部工具自主完成复杂任务。然而，现有基准测试主要评估固定场景下的端到端性能，存在以下问题：
- 局限于特定技能的评估，无法衡量智能体的学习能力；
- 随着智能体能力提升，得分趋于饱和（score saturation）；
- 依赖专家人工标注，成本高且扩展性差。

论文的核心动机是：学习能力（包括自我改进和同伴学习）是智能体向人类水平智能进化的核心驱动力，但现有评估体系忽视了这一维度。因此，需要一种能够动态、持续评估智能体学习潜力的新框架。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
提出**迭代式竞争同伴学习框架**，让智能体在重复对战中通过反馈不断优化策略，从而系统评估其自我改进和同伴学习能力。

### 关键技术细节
- **锦标赛风格评估平台（CATArena）**：采用四种多样的棋盘游戏和纸牌游戏，任务具有开放式评分（无明确上限），避免分数饱和问题。
- **迭代竞争机制**：智能体在每一轮比赛中获得对手的反馈（如胜负结果、策略信息），并基于这些反馈调整自身策略。重复该过程，模拟智能体的进化轨迹。
- **评估指标**：重点考察学习能力（自我改进速度、从同伴处学习的能力）以及策略编码能力，而非仅最终得分。

没有提供具体的公式或算法伪代码，但可从描述推断其流程：
  1. 初始化智能体策略；
  2. 进行多轮锦标赛匹配，每轮智能体与其他智能体对抗；
  3. 每轮结束后，智能体接收反馈（可能是胜负、对手策略表现等）；
  4. 智能体利用反馈更新策略（自我改进或通过模仿同伴成功策略）；
  5. 重复多轮，记录智能体的得分变化曲线，评估其学习能力。

## 3. 实验设计

- **数据集/场景**：未使用传统自然语言处理数据集，而是构建了**四种棋盘和纸牌游戏**作为任务环境。这些游戏具有开放式得分，适合评估智能体长期进化能力。
- **Benchmark**：CATArena本身即为评估平台，与其他静态基准（如固定场景下的单轮评估）进行比较。
- **对比方法**：论文提到了“minimal and commercial code agents”，即基线智能体包括最小化代码实现的智能体（可能基于简单规则）和商业级代码智能体（如GPT-4等）。具体对比了哪些模型未详细列出，但强调了CATArena能更好区分不同智能体的长期进化潜力。

## 4. 资源与算力

论文中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。只提到实验涉及“minimal and commercial code agents”，但未披露具体硬件资源。推测这类评估可能不需要大规模训练，而是推理阶段的对抗模拟。

## 5. 实验数量与充分性

从摘要和元数据来看：
- 实验围绕四种游戏进行，可能包含多轮锦标赛（如每轮多次匹配）。
- 对比了至少两种类型智能体（简单规则智能体和商业LLM智能体）。
- 没有提及详细的消融实验（如不同学习机制的影响），但作者声称实验结果证明CATArena提供了“可靠、稳定、可扩展的基准测试”。
- **充分性评价**：实验覆盖了多种游戏场景，但仅有四种游戏，且未公开智能体数量、对抗轮次等细节。对比基线种类有限。整体看实验尚可，但不足以称为非常全面。结果统计未提供具体数值。

## 6. 主要结论与发现

- 学习能力（自我改进和同伴学习）应作为智能体评估的核心维度，CATArena为此提供了有效的动态评估方案。
- 相比静态基准，CATArena能更好区分不同智能体的长期进化潜力，且无需人工标注。
- 迭代竞争框架可以揭示智能体在重复交互中的策略优化程度，避免分数饱和问题。

## 7. 优点

- **创新性**：首次将学习能力作为评估核心，提出动态锦标赛框架，填补了现有基准的空白。
- **无上限评分**：通过开放式任务，解决了传统基准得分饱和问题，可持续评估进步中的智能体。
- **无需人工标注**：完全自动化的对抗评估，降低了成本。
- **强调同伴学习**：不仅考虑自我改进，还纳入社会学习因素，更贴近人类学习模式。

## 8. 不足与局限

- **实验覆盖有限**：仅四种游戏，且未说明游戏复杂度是否足够代表真实任务场景。可能缺乏通用性。
- **基线不足**：仅对比了最小化代码智能体和商业智能体，未与流行的LLM智能体（如ReAct、AutoGPT等）进行系统比较。
- **缺乏消融实验**：未分析不同学习机制（如仅自我改进 vs. 同伴学习）的贡献。
- **算力与资源未公开**：无法复现或评估实际运行成本。
- **潜在偏差**：游戏设计可能偏向某种策略类型，导致评估不全面；反馈机制的设计可能影响智能体学习效率。
- **应用限制**：目前仅适用于游戏场景，如何扩展到现实任务（如编程、对话）尚不明确。

（完）
