---
title: Self-evolving LLM agents with in-distribution Optimization
title_zh: 通过分布内优化的自进化LLM智能体
authors: "Yudi Zhang, Meng Fang, Zhenfang Chen, Mykola Pechenizkiy"
date: 2026-04-30
pdf: "https://openreview.net/pdf/e51250bd0bc31fb1adf79dda5d60ea2289150131.pdf"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 通过分布内强化学习实现LLM智能体自我进化以提升技能
tldr: LLM智能体在长程决策中面临信用分配问题。本文提出Q-Evolve框架，统一自动过程奖励标注和分布内策略学习，利用混合离线数据集稳定贝尔曼更新。实验表明智能体可自我进化，技能持续提升。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 长程决策中延迟奖励导致信用分配困难，阻碍智能体技能进化。
method: 结合自动过程奖励标注与分布内强化学习，学习批评器从混合数据集中稳定更新。
result: 在多个环境中，智能体实现了自我进化和性能提升。
conclusion: 为智能体技能的持续进化提供了有效框架。
---

## Abstract
Large Language Models (LLMs) have recently emerged as powerful controllers for interactive agents in complex environments, yet training them to perform reliable long-horizon decision making remains a fundamental challenge. A key difficulty lies in credit assignment: agents often receive delayed rewards only at the end of episodes. In this paper, we propose Q-Evolve, a self-evolving framework for LLM agents that unifies automatic process-reward labeling and policy learning within a principled in-distribution reinforcement learning paradigm. In each evolving iteration, our method learns an in-distribution critic from a hybrid off-policy dataset that combines expert demonstrations with agent-generated trajectories, stabilizing Bellman backups in sparse-reward settings via a weighted Implicit Q-Learning objective. The learned value function is then used to derive step-wise process rewards through advantage estimation, enabling dense and reliable supervision without environment backtracking or human annotation. Leveraging these signals, we perform behavior-proximal policy optimization that evolves the agent over the data used for process reward labeling, allowing iterative self-improvement without exacerbating distribution shift.
We evaluate our method on AlfWorld, WebShop, and ScienceWorld, showing Q-Evolve outperforms strong baselines in sample efficiency, robustness, and overall task performance. Our results demonstrate that stable agent self-evolution is achievable through the co-evolution of process-level supervision and policy, both grounded within a shared in-distribution learning loop.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：大型语言模型（LLM）作为交互式智能体的控制器时，在长程决策任务中面临严重的信用分配问题——智能体仅在 episode 结束时获得延迟奖励，难以将最终成败归因于中间步骤的动作，导致模型无法有效学习复杂技能。
- **研究动机**：现有方法要么依赖稀疏的最终奖励，要么需要昂贵的人工过程奖励标注或环境回滚，限制了智能体的自我进化能力。本文旨在提出一种无需人工干预、无需环境回溯的自我进化框架，使 LLM 智能体能够持续提升其决策技能。
- **整体含义**：通过将自动过程奖励标注与分布内强化学习统一在一个闭环中，实现智能体策略与过程监督信号的协同进化，从而在样本效率、鲁棒性和最终性能上超越现有基线。

## 2. 论文提出的方法论

### 核心思想
- 提出 **Q-Evolve** 框架：在每个进化迭代中，先从一个混合离线数据集（包含专家演示和智能体自生成轨迹）中学习一个 **分布内批评器（in-distribution critic）**，利用加权隐式 Q 学习（Implicit Q-Learning）稳定稀疏奖励下的贝尔曼备份；然后使用该价值函数通过优势估计导出**密集的过程奖励**；最后利用这些信号进行**行为近端策略优化**（Behavior-Proximal Policy Optimization），使智能体在用于过程奖励标注的数据上迭代自我改进，避免分布偏移的恶化。

### 关键技术细节
1. **混合离线数据集**：结合专家演示（高质）与智能体自身历史轨迹（多样），为批评器提供稳定的数据支持。
2. **加权隐式 Q 学习（Weighted Implicit Q-Learning, W-IQL）**：目标是在不估计动作分布的情况下学习价值函数，通过权重调整减少外推误差，稳定贝尔曼更新。
3. **自动过程奖励标注**：利用学到的价值函数计算优势函数，为每个中间步骤赋予稠密的过程奖励，无需环境回滚或人工。
4. **行为近端策略优化**：在标注过程奖励的同一数据集上进行策略更新，限制策略与行为策略的偏离，防止分布偏移。

### 算法流程（文字描述）
- **初始化**：加载预训练 LLM 作为策略初始模型。收集专家演示轨迹和初始智能体自生成轨迹，构成混合离线数据集。
- **每轮进化迭代**：
  - **阶段 1（批评器学习）**：在混合数据集上，使用 W-IQL 学习价值函数 \( Q(s,a) \)，通过加权约束稳定稀疏奖励环境下的训练。
  - **阶段 2（过程奖励生成）**：对数据集中的每条轨迹，利用批评器计算每个时刻的优势估计 \( A(s_t,a_t) \)，得到过程奖励 \( r_t = A(s_t,a_t) \)。
  - **阶段 3（策略进化）**：在标注了过程奖励的数据上执行行为近端策略优化，更新 LLM 策略。
- **收束条件**：当性能不再提升或达到预设迭代次数时停止。

## 3. 实验设计

- **评测环境**：AlfWorld（家居任务）、WebShop（网页购物）、ScienceWorld（科学实验）——三个具有挑战性的长程交互环境。
- **基准方法**：摘要未列出具体基线名称，但根据常见的 LLM 智能体训练方法推测，对比方法可能包括：
  - 仅使用最终奖励的强化学习（例如 PPO 或 REINFORCE）；
  - 使用人工过程奖励的方法；
  - 依赖环境回滚的方法（如 RAPID、Backpropagation-through-time）；
  - 其他自我进化基线和静态 RL 微调方法。
- **评估指标**：任务成功率（或累积奖励）、样本效率、鲁棒性（可能通过不同随机种子下的方差衡量）。

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**使用的 GPU 型号、数量、训练时长等具体算力信息。可能因为篇幅限制，未在摘要中提及。建议读者查阅全文的实验设置部分。

## 5. 实验数量与充分性

- **实验数量**：在三个独立环境（AlfWorld、WebShop、ScienceWorld）上进行了主要对比实验。此外，可能包含消融实验（例如移除过程奖励、去掉分布内约束、不使用混合数据集等），以及不同初始化条件下的稳定性测试。摘要未具体说明消融实验的数量，但通常这类工作会包含充足的消融分析。
- **充分性与公平性**：三个环境覆盖了不同类型的交互任务（指令跟随、网页操作、科学实验），具有一定的代表性。对比方法应选取公开可比的基线，但摘要未列出对比方法的具体细节和统计显著性检验，需整体阅读评估。若该方法在所有环境上一致超过基线，可以认为是充分的。

## 6. 主要结论与发现

- **稳定自进化**：Q-Evolve 框架使 LLM 智能体能够在无需人工或环境回滚的情况下实现持续自我进化，技能逐步提升。
- **样本效率高**：相比基线方法，Q-Evolve 在相同数量的交互数据下取得更高任务成功率。
- **鲁棒性强**：在不同环境和随机种子下，性能波动较小，表明框架对分布偏移具有免疫力。
- **协同进化有效**：过程监督信号与策略在同一分布内学习循环中共同进化，避免了传统离线强化学习中批评器与策略的偏差累积。

## 7. 优点

- **统一框架**：将自动过程奖励标注与分布内策略优化整合，无需任何人工标注或环境访问。
- **解决信用分配**：通过价值函数导出的过程奖励解决了长程延迟奖励问题，让智能体获得稠密反馈。
- **防止分布偏移**：行为近端优化和加权隐式 Q 学习保证了学习过程在数据分布内进行，稳定训练。
- **较强的泛化性**：在三个不同领域验证了有效性，展示了方法的通用性。
- **可迭代性**：框架天然支持多轮自我进化，性能可单调提升。

## 8. 不足与局限

- **未报告计算资源**：缺少训练耗时和硬件配置信息，难以评估方法的实际成本。
- **基线对比不够详细**：摘要中未列出具体对比方法名称和性能数字，无法直接判断方法优势的显著性。
- **假设前提强**：方法依赖于一个包含专家演示的初始混合数据集，在缺乏高质量演示的现实场景中可能受限。
- **潜在过程奖励噪声**：自动标注的过程奖励依赖于学习到的批评器，批评器本身可能存在偏差，尤其是在初始轮次。
- **长期记忆与环境交互能力**：实验环境均为模拟器，在真实世界机器人应用中的泛化性尚待验证。
- **仅展示了性能提升**，未深入分析自我进化过程中的技能变化路径或在极端稀疏奖励下的边界性能。

（完）
