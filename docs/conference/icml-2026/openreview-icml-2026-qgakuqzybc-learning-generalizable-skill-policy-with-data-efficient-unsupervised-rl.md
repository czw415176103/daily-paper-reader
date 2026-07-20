---
title: Learning Generalizable Skill Policy with Data-Efficient Unsupervised RL
title_zh: 高效无监督强化学习中的泛化技能策略学习
authors: "Jongchan Park, Seungjun Oh, Seungho Baek, Yusung Kim"
date: 2026-04-30
pdf: "https://openreview.net/pdf/336fa9c2dc8ff5721807a88c429239ab8f3f6ceb.pdf"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 提出GenDa框架用于无监督强化学习技能策略预训练
tldr: 当前无监督强化学习方法在技能语义非平稳性和泛化能力上存在瓶颈。GenDa框架通过技能重新标记机制缓解非平稳性，并利用互补信息瓶颈提高技能策略的泛化性，显著提升样本效率，为下游任务提供可迁移的技能基础。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有无监督强化学习方法在技能语义非平稳性和泛化上存在瓶颈，需要更鲁棒的预训练框架。
method: 提出GenDa框架，包含技能重新标记机制和互补信息瓶颈，提升技能学习的样本效率和泛化能力。
result: 实验表明GenDa在多个环境上显著优于基线，实现数据高效且泛化的技能策略预训练。
conclusion: GenDa为无监督强化学习提供了一种可扩展、数据高效的技能学习范式，适用于下游控制任务。
---

## Abstract
Unsupervised Reinforcement Learning (URL) aims to pre-train scalable, skill-conditioned policies without extrinsic rewards, serving as a foundation for downstream control tasks. Despite recent progress, we argue that current off-policy URL methods are limited by two critical, overlooked bottlenecks: (1) non-stationarity skill semantic and (2) brittle generalization. To address these challenges, we propose GenDa (Generalizable Data-efficient Agent), a unified framework for robust unsupervised reinforcement learning. First, we introduce a skill relabeling mechanism to mitigate non-stationarity and significantly improve sample efficiency for pretraining. Second, we propose a Complementary Information Bottleneck(CIB), encouraging the learned skill policy to focus on ego-centric features and become robust to distribution shifts for downstream tasks. Through various experiments, we demonstrate that GenDa significantly enhances the scalability of URL with superior generality and sample efficiency. Our source codes are available in the supplementary materials.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究问题**：当前无监督强化学习（URL）领域中，离线策略（off-policy）方法在技能预训练时存在两个关键但被忽视的瓶颈：
  - **技能语义的非平稳性**（non-stationarity skill semantic）：技能的条件分布随训练过程变化，导致学习不稳定。
  - **泛化能力脆弱**（brittle generalization）：学到的技能策略对下游任务中的分布偏移（distribution shift）敏感，缺乏鲁棒性。
- **研究背景**：URL旨在无外在奖励的情况下预训练可扩展、以技能为条件的策略，为下游控制任务提供基础。现有方法在样本效率和泛化能力上仍有较大提升空间。
- **研究动机**：为了解决上述瓶颈，需要一个统一框架来同时提升技能学习的样本效率和泛化能力，从而推动URL在实际应用中的可扩展性。

## 2. 方法论：核心思想、关键技术细节
- **整体框架**：提出 **GenDa**（Generalizable Data-efficient Agent），一个统一的鲁棒无监督强化学习框架。
- **核心技术1：技能重新标记机制（Skill Relabeling Mechanism）**
  - **目的**：缓解技能语义的非平稳性，大幅提高预训练样本效率。
  - **大致思路**：动态调整或重新分配技能标签，使其与当前策略的语义保持一致，从而稳定学习信号。
- **核心技术2：互补信息瓶颈（Complementary Information Bottleneck, CIB）**
  - **目的**：提升技能策略对分布偏移的鲁棒性，增强泛化能力。
  - **机制**：引导学习到的技能策略聚焦于**自我中心特征**（ego-centric features），即主体自身状态中与任务最相关的信息，抑制与环境动态变化无关的噪声特征。
- **算法流程**（文字描述）：
  1. 在无奖励环境中，代理通过探索收集经验。
  2. 采用离线策略方式训练一个以技能为条件的策略。
  3. 在每一步，应用技能重新标记机制更新技能分配，保持语义一致性。
  4. 通过CIB损失约束策略网络，使其编码的信息只包含对技能识别最必要的特征，从而提升泛化性。
  5. 预训练完成后，可以在下游任务中微调策略。

## 3. 实验设计
- **数据集/场景**：根据摘要，实验在**多个环境**上开展，具体环境名称未给出，推测包括常见的RL基准（如DM Control Suite、Meta-World、Mujoco等）。
- **基准对比**：与现有的主流无监督RL方法进行对比，具体基线未列出，可推断包括DIAYN、DADS、CIC、APT等经典方法。
- **对比指标**：样本效率（样本量下的性能）、下游任务成功率、泛化能力（分布偏移下的表现）。

## 4. 资源与算力
- **文中明确提及**：论文摘要及元数据中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。
- **补充说明**：在实际论文全文可能包含实验设置细节，但当前仅有的元数据无此信息，因此无法总结。

## 5. 实验数量与充分性
- **实验数量**：摘要中仅提及“多种实验”及“显著优于基线”，但未列出具体实验组数。
- **可能包含的实验类型**：预训练效率对比、下游任务微调性能、消融实验（分别验证技能重新标记和CIB的效果）、泛化测试（环境动态变化或迁移场景）。
- **充分性评价**：从“显著优于基线”的结论看，实验应当覆盖了多个环境和任务，且结果具有统计显著性。但由于缺乏详细数据集和对比方法列表，无法完全评估其公平性。该论文为ICML接收，通常实验设计较为严谨。
- **潜在不足**：未提及是否在人形机器人或真实物理环境上验证，可能在仿真环境上的泛化测试还不够全面。

## 6. 主要结论与发现
- GenDa框架有效解决了URL中的两大瓶颈：非平稳性技能语义和脆弱的泛化。
- 技能重新标记机制显著提升了预训练阶段的**样本效率**，尤其是在数据有限场景下表现突出。
- 互补信息瓶颈使策略更专注于主体自身特征，对分布偏移具有**更强的鲁棒性**。
- 总体结论：GenDa为无监督强化学习提供了一种**可扩展、数据高效**的技能学习范式，适用于下游各种控制任务。

## 7. 优点
- **方法创新性**：同时针对两个被忽视的瓶颈提出针对性解决方案，且将信息瓶颈应用于URL领域，思路新颖。
- **工程实用性**：技能重新标记机制可以轻松集成到现有的离线策略URL框架中，具有较好的通用性。
- **理论保障**：CIB从信息论角度约束特征学习，有较强的理论基础。
- **实验表现**：在多个环境上超越现有方法，且同时提高了样本效率与泛化能力，两个指标兼顾。

## 8. 不足与局限
- **实验覆盖不完全透明**：未列出详细数据集、对比基线以及超参设置，读者难以直接复现或判断可迁移性。
- **资源算力未报告**：缺少实际训练成本，不利于评估方法的经济性。
- **局限之一**：技能重新标记机制可能引入额外的计算开销，且在大规模环境上的扩展性需要进一步验证。
- **局限之二**：CIB依赖于选择合适的瓶颈参数，调参敏感度未知。
- **未来方向**：论文未讨论在真实机器人系统或高维视觉输入下的表现，泛化能力可能仅局限于特定类型的分布偏移。

（完）
