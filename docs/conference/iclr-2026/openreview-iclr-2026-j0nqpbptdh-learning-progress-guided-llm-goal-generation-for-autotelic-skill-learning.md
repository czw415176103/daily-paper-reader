---
title: Learning Progress-Guided LLM Goal Generation for Autotelic Skill Learning
title_zh: 基于学习进度的LLM目标生成用于自驱技能学习
authors: "Guillaume Pourcel, Grgur Kovač, Thomas Carta, Cédric Colas, Pierre-Yves Oudeyer"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=J0NqPbpTDh"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 利用LLM指导目标生成，推进强化学习代理技能习得
tldr: 强化学习代理通常受限于固定目标空间，限制了技能广度。本文提出基于学习进度的LLM目标生成方法，直接优化目标的可学习性与多样性。在Crafter领域，该方法使代理获得多样且可学习的技能组合，优于基于能力的基线。这为自主技能进化提供了新途径。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 固定目标空间限制强化学习代理的技能广度。
method: 利用LLM生成目标并基于学习进度优化，而非基于能力。
result: 在Crafter领域，代理习得多样且可学习的技能，优于基线。
conclusion: 优化学习进度的目标生成可有效驱动自驱技能发展。
---

## Abstract
Reinforcement learning agents typically operate within fixed goal spaces, which limits the breadth of skills they can acquire. Large language models promise to overcome this constraint through dynamic goal generation, yet prompting them for merely *interesting* goals rarely produces effective curricula. We evaluate open-ended curricula using two key dimensions — *learnability* and *diversity* — and show that competence-based LLM approaches generate goals that appear promising but drive limited genuine learning progress. Our method instead optimizes goal generation directly for learning progress and consistently outperforms competence-based baselines on both learnability and diversity. In the *Crafter* domain, this leads agents to acquire diverse, challenging, and practically useful skills in the absence of extrinsic rewards.

---

## 论文详细总结（自动生成）

# 基于学习进度的LLM目标生成用于自驱技能学习

## 1. 核心问题与整体含义

- **研究背景**：强化学习代理通常被限制在固定的目标空间内，这严重制约了其技能习得的广度。传统方法难以让代理自主探索并掌握多样化的技能。
- **核心问题**：如何突破固定目标空间的限制，使代理能够自主生成并学习广泛、有用且可习得的技能？
- **整体含义**：本文提出利用大型语言模型（LLM）动态生成目标，并通过优化学习进度而非能力来驱动代理的技能发展。这一思路为构建真正开放、自驱动的技能进化系统提供了新途径。

## 2. 方法论

- **核心思想**：不再依赖LLM生成“有趣”或“基于能力”的目标，而是直接优化目标的可学习性（learnability）与多样性（diversity），并以学习进度（learning progress）作为优化目标。
- **关键技术细节**：
  - 使用LLM作为目标生成器，动态提出新的目标。
  - 设计一个评价机制，衡量代理在每个目标上的学习进度（即能力提升速率）。
  - 目标生成器根据学习进度反馈，优先生成那些能带来显著进步且多样化的目标，形成良性循环的课程。
- **公式或算法流程**（文字说明）：
  - 初始阶段，LLM生成一批候选目标。
  - 代理尝试学习这些目标，并记录每个目标上的表现变化（学习进度）。
  - 选择学习进度最高的目标作为下一阶段的主攻方向，并调整LLM的生成策略（例如通过prompt或微调）以生成更多类似的高进度目标。
  - 同时引入多样性约束，避免陷入单一技能。
  - 迭代上述过程，直到代理掌握广泛技能。

## 3. 实验设计

- **使用场景/数据集**：在 **Crafter** 领域（一个类似Minecraft的2D开放世界环境）中进行评估。
- **Benchmark**：以代理习得技能的多样性、可学习性以及实用性作为评价指标。
- **对比方法**：
  - 基于能力的LLM目标生成基线（competence-based LLM approaches），即生成代理当前能力可能达到的目标。
  - 随机目标生成等简单基线。

## 4. 资源与算力

- 文中未明确说明实验所使用的GPU型号、数量、训练时长等算力信息。需要推测实验规模较小（Crafter环境），但LLM推理可能消耗一定资源。具体细节缺失。

## 5. 实验数量与充分性

- 文中仅提及在Crafter领域中进行了一组主要实验和对比，未提及多种环境、消融实验或多组重复实验。
- **充分性评价**：实验设计较为初步，仅覆盖一个环境，缺少跨领域验证、超参数敏感性分析以及详细的消融研究。对比基线也较少，因此实验的客观性和公平性有待加强。但结果确实展示了方法的优势。

## 6. 主要结论与发现

- 基于学习进度的LLM目标生成方法在Crafter领域上，使代理习得了多样化、具有挑战性且实际有用的技能，且显著优于基于能力的基线方法。
- 直接优化学习进度能够更有效地驱动自驱技能发展，而“有趣”或“能力匹配”的目标往往导致有限的实际进步。
- 该方法无需外部奖励，完全依赖内在驱动（学习进度），实现了自主技能进化。

## 7. 优点

- **方法创新**：将学习进度作为目标生成的直接优化目标，而非间接依赖能力评估，更符合自驱学习的本质。
- **实践效果**：在单一环境中表现出色，证明了LLM结合学习进度优化的潜力。
- **简洁性**：核心思想清晰，易于理解和复现。

## 8. 不足与局限

- **实验覆盖不足**：仅在一个领域（Crafter）验证，缺少机器人操控、游戏、物理模拟等其他环境的评估，推广性存疑。
- **对比基线有限**：仅对比了基于能力的LLM方法，未全面对比其他目标生成策略（如基于新奇性、基于内在奖励的课程学习等）。
- **未提供资源消耗**：无法评估方法的实际算力成本。
- **缺乏消融分析**：未明确分离LLM生成能力与学习进度优化各自的贡献，可能存在混杂因素。
- **LLM依赖**：方法依赖LLM生成目标的质量与多样性，LLM本身的偏见或局限性可能影响最终技能习得。

（完）
