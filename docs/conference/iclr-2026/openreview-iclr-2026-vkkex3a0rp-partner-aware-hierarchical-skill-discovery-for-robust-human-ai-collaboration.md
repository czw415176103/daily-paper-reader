---
title: Partner-Aware Hierarchical Skill Discovery for Robust Human-AI Collaboration
title_zh: 面向鲁棒人机协作的同伴感知层级技能发现
authors: "Adnan Ahmad, Bahareh nakisa, Mohammad Naim rastgoo"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=vKkEX3a0rP"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 基于同伴行为的多智能体层级强化学习技能发现
tldr: 现有深度层级强化学习仅关注智能体自身奖励，忽略同伴行为，导致技能泛化差。PASD框架引入对比内在奖励，学习基于同伴行为的技能表示，使智能体能够适应新同伴。在多人协作任务中，PASD显著提升了适应性和协作效果。该方法为多智能体技能学习提供了新范式。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有智能体技能发现忽视同伴行为，导致无法适应新伙伴。
method: 提出同伴感知技能发现（PASD），在DHRL中引入对比内在奖励以学习同伴条件技能。
result: PASD在多人协作基准上优于基线，提升了适应性。
conclusion: 同伴感知的技能发现是实现鲁棒人机协作的关键。
---

## Abstract
Multi-agent collaboration, especially in human-AI teaming, requires agents that can adapt to novel partners with diverse and dynamic behaviors. Conventional Deep Hierarchical Reinforcement Learning (DHRL) methods focus on agent-centric rewards and overlook partner behavior, leading to shortcut learning, where skills exploit spurious information instead of adapting to partners’ dynamic behaviors. This limitation undermines agents' ability to adapt and coordinate effectively with novel partners. We introduce Partner-Aware Skill Discovery (PASD), a DHRL framework that learns skills conditioned on partner behavior. PASD introduces a contrastive intrinsic reward to capture patterns emerging from partner interactions, aligning skill representations across similar partners while maintaining discriminability across diverse strategies. By structuring the skill space based on partner interactions, this approach mitigates shortcut learning and promotes behavioral consistency, enabling robust and adaptive coordination. We extensively evaluate PASD in the Overcooked-AI benchmark with a diverse population of partners characterized by varying skill levels and play styles. We further evaluate the approach with human proxy models trained from human–human gameplay trajectories. PASD consistently outperforms existing population-based and hierarchical baselines, demonstrating transferable skill learning that generalizes across a wide range of partner behaviors. Analysis of learned skill representations shows that PASD adapts effectively to diverse partner behaviors, highlighting its robustness in human-AI collaboration.

---

## 论文详细总结（自动生成）

# 论文中文详细总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：在多智能体协作，特别是人机协作场景中，智能体需要能够适应具有多样动态行为的新伙伴。然而，现有的深度层级强化学习（DHRL）方法仅关注智能体自身的奖励，忽略了同伴的行为，导致“捷径学习”（shortcut learning）——技能利用虚假信息而非适应同伴动态行为，严重限制了智能体对新伙伴的适应和有效协调能力。
- **整体含义**：本文旨在解决DHRL技能发现中同伴信息缺失的问题，提出一种同伴感知技能发现（PASD）框架，使智能体学到基于同伴行为的技能，从而实现鲁棒且可迁移的人机协作。

## 2. 论文提出的方法论
- **核心思想**：将同伴行为作为技能学习的条件，通过对比内在奖励（contrastive intrinsic reward）来捕获同伴交互中涌现的模式，使相似同伴的技能表示趋于一致，同时保持对不同策略的区分性，从而缓解捷径学习，促进行为一致性，实现鲁棒自适应协调。
- **关键技术细节**：
  - 在DHRL框架中引入一个对比内在奖励模块，该奖励基于智能体对同伴行为的观测计算得到。
  - 通过对比学习（contrastive learning）使得在相同或相似同伴策略下的技能表示更靠近，不同策略下的技能表示更远离。
  - 技能空间被结构化地组织，基于同伴交互而非仅基于智能体自身状态。
- **算法流程（文字说明）**：
  1. 智能体通过高层策略（manager）选择目标技能（subgoal），低层策略（worker）执行动作以达成该技能。
  2. 在训练过程中，除了环境奖励外，还计算对比内在奖励，该奖励衡量当前技能表示与从同伴行为中提取的上下文表示之间的一致性。
  3. 通过最大化该内在奖励，智能体学习到的技能能够反映同伴行为的模式，从而在新同伴出现时，智能体可基于其行为选择合适的技能。

## 3. 实验设计
- **数据集/场景**：使用 **Overcooked-AI** 基准测试环境，该环境模拟双人协作烹饪任务。实验包含多样化的同伴群体，具有不同的技能水平和游戏风格。此外，还使用从真实人-人游戏轨迹中训练得到的人类代理模型进行评估。
- **对比方法**：与现有的基于群体的（population-based）基线方法和层级（hierarchical）基线方法进行比较。
- **评价指标**：协作成功率、适应性（对新同伴的泛化能力）、技能表示的可解释性等。

## 4. 资源与算力
- 论文中**未明确说明**使用的 GPU 型号、数量或训练时长等计算资源信息。

## 5. 实验数量与充分性
- 实验覆盖了多种同伴类型（不同技能水平和风格）、人类代理模型以及消融分析（对比内在奖励的影响）。虽然未列举具体实验组数，但摘要中称“extensively evaluate”，且结果一致优于基线，表明实验较为充分。
- **公平性/客观性**：采用了公开基准（Overcooked-AI）和标准对比基线，实验设计相对客观。但未提供统计显著性检验或多次重复实验的详细数据，可能存在一定偏差风险。

## 6. 论文的主要结论与发现
- PASD 在所有同伴行为上持续优于现有的基于群体的和层级的基线方法，展示了可迁移的技能学习，能够泛化到广泛的同伴行为。
- 对学习到的技能表示进行分析表明，PASD 能有效适应多样化同伴行为，体现鲁棒性。
- 对比内在奖励有助于缓解捷径学习，使技能更专注于同伴交互模式，而非虚假相关特征。

## 7. 优点
- **问题导向明确**：直击DHRL中同伴行为被忽视这一核心缺陷。
- **方法创新**：将对比学习引入层级强化学习，设计同伴条件下的内在奖励，结构清晰。
- **实验场景多样**：涵盖模拟同伴和从人类数据训练的代理，兼具生态效度和可控性。
- **结果突出**：在公开基准上全面超越基线，特别是对新同伴的适应能力。

## 8. 不足与局限
- **算力信息缺失**：未提供计算成本，难以评估可复现性和资源需求。
- **统计细节不足**：未给出多次实验的均值、方差及显著性检验，结果的稳健性有待进一步确认。
- **应用限制**：仅在Overcooked这一特定协作任务中验证，真实人机交互场景的复杂性可能更高（如通信、信任等因素未考虑）。
- **被拒稿记录**：该论文在 ICLR 2026 被拒稿（来源标注为“Rejected-Public”），可能意味着存在评审指出的其他问题（如理论深度、泛化性证据等），本文未充分讨论。

（完）
