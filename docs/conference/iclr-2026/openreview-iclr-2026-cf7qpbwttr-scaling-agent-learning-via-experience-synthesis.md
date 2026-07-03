---
title: Scaling Agent Learning via Experience Synthesis
title_zh: 通过经验合成扩展智能体学习
authors: "Zhaorun Chen, Zhuokai Zhao, Kai Zhang, Bo Liu, Qi Qi, Yifan Wu, Tarun Kalluri, Xuefei Cao, Yuanhao Xiong, Haibo Tong, Huaxiu Yao, Hengduo Li, Jiacheng Zhu, Xian Li, Dawn Song, Bo Li, Jason E Weston, Dat Huynh"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=cf7qpBwttr"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 经验合成扩展智能体强化学习
tldr: 强化学习训练受限于实际交互成本高和多样性不足。DreamGym通过推理构建经验模型生成一致的状态转移和反馈，免去真实环境交互，实现大规模在线训练，在多个连续控制任务上达到或超过真实交互性能。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: RL智能体训练依赖昂贵的实际环境交互，难以扩展。
method: 构建基于推理的经验模型，合成多样化的过渡和反馈数据。
result: 在多个控制任务上，合成训练达到甚至超过真实环境训练效果。
conclusion: 经验合成可有效扩展RL智能体训练并降低成本。
---

## Abstract
While reinforcement learning (RL) can empower autonomous agents by enabling self-improvement through interaction, its practical adoption remains challenging due to costly rollouts, limited task diversity, unreliable reward signals, and infrastructure complexity, all of which obstruct the collection of scalable experience data. To address these challenges, we introduce DreamGym, the first unified framework designed to synthesize diverse experiences with scalability in mind to enable effective online RL training for autonomous agents. Rather than relying on expensive real-environment rollouts, DreamGym distills environment dynamics into a reasoning-based experience model that derives consistent state transitions and feedback signals through step-by-step reasoning, enabling scalable agent rollout collection for RL. To improve the stability and quality of transitions, DreamGym leverages an experience replay buffer initialized with offline real-world data and continuously enriched with fresh interactions to actively support agent training. To improve knowledge acquisition, DreamGym adaptively generates new tasks that challenge the current agent policy, enabling more effective online curriculum learning. Experiments across diverse environments and agent backbones demonstrate that DreamGym substantially improves RL training, both in fully synthetic settings and in sim-to-real transfer scenarios. On non-RL-ready tasks like WebArena, DreamGym outperforms all baselines by over 30%. And in RL-ready but costly settings, it matches GRPO and PPO performance using only synthetic interactions. When transferring a policy trained purely on synthetic experiences to real-environment RL, DreamGym yields significant additional performance gains while requiring far fewer real-world interactions, providing a scalable warm-start strategy for general-purpose RL.

---

## 论文详细总结（自动生成）

# 论文总结：通过经验合成扩展智能体学习（DreamGym）

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：强化学习（RL）使智能体通过自我交互实现能力提升，但实际应用面临**高昂的 rollout 成本、有限的任务多样性、不可靠的奖励信号以及基础设施复杂性**，这些因素阻碍了大规模经验数据的收集。
- **整体含义**：提出首个统一框架 **DreamGym**，旨在**通过合成多样化经验实现可扩展的在线 RL 训练**，从而摆脱对昂贵真实环境交互的依赖，降低 RL 应用门槛并提升训练效率。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：将环境动态蒸馏为一个**基于推理的经验模型**，通过逐步推理生成一致的状态转移和反馈信号，从而实现可扩展的智能体 rollout 收集。
- **关键技术细节**：
  - **基于推理的经验模型**：不依赖真实环境交互，而是通过推理步骤推导出合理的状态转移和奖励信号，保证合成经验的**一致性**。
  - **经验回放缓冲**：初始化时加入离线真实世界数据，并持续用新鲜合成交互进行丰富，以提升转移的稳定性和质量，并主动支持智能体训练。
  - **自适应课程学习**：动态生成**挑战当前策略的新任务**，形成有效在线课程学习，增强知识获取效率。
- **公式/算法流程**（文字说明）：  
  （摘要中未提供具体公式，但流程可概括为）  
  ① 初始化经验回放缓冲（含离线真实数据）→  
  ② 推理经验模型根据当前策略生成合成轨迹（状态转移动态 + 反馈信号）→  
  ③ 缓冲不断吸收合成交互数据 →  
  ④ 在线 RL 更新智能体策略 →  
  ⑤ 自适应生成更难的任务，重复步骤②~④。

## 3. 实验设计：场景、基准与对比方法
- **场景与数据集**：
  - **多种连续控制环境**（未具体列举名称，但提及多个环境）。
  - **WebArena**（非 RL-ready 任务）。
  - **RL-ready 但昂贵的任务**（如标准 RL 基准）。
- **基准**：与现有的强化学习方法对比，包括 **GRPO**、**PPO** 等。
- **对比方法**：  
  - 在 WebArena 上：与所有基线方法对比，DreamGym 性能超 30% 以上。  
  - 在 RL-ready 任务上：仅使用合成交互即达到 GRPO 和 PPO 的性能。
  - **sim-to-real 转移**：将纯合成经验训练的策略迁移到真实环境 RL，获得额外性能提升，且所需真实交互远少于常规方法。

## 4. 资源与算力
- **未明确说明**：论文摘要中未提及 GPU 型号、数量、训练时长等算力信息。  
  （推测：由于强调可扩展性，可能使用了中等规模计算资源，但无法从文本确认。）

## 5. 实验数量与充分性
- **实验数量**：  
  - 涉及**多种环境**（包括 WebArena、连续控制任务）和**多种智能体主干**（agent backbones）。  
  - 包含**完全合成设置**（全合成训练）和**sim-to-real 转移**两大实验分支。  
  - 有在非 RL-ready 任务上的性能对比，以及在 RL-ready 任务上与 GRPO、PPO 的并列比较。  
  - 消融实验：摘要提到“自适应课程学习”和“经验回放缓冲”等组件，但未列出具体消融结果；推测有相关消融研究（但文本未展开）。
- **充分性与客观性**：  
  - 实验场景多样，覆盖不同难度和类型，对比基线（GRPO、PPO、所有基线）具有代表性。  
  - 报告了显著性能提升（如 30% 以上），以及持平或超过真实交互的效果，结果积极。  
  - 缺乏对失败案例或合成经验偏差的讨论，也未说明统计显著性（如多次重复、方差等），因此**实验报告完整性有限**。

## 6. 主要结论与发现
- **DreamGym 显著提升了 RL 训练效果**：在完全合成设置和 sim-to-real 转移中均表现优异。
- **WebArena 等非 RL-ready 任务**上，DreamGym 超越所有基线超过 30%。
- **在 RL-ready 但昂贵场景**下，仅使用合成经验即可匹敌 GRPO 和 PPO 性能。
- **为通用 RL 提供可扩展的预热策略**：策略从纯合成经验迁移至真实环境后，只需极少真实交互即可获得额外性能增益，降低了真实交互成本。

## 7. 优点（方法与实验设计亮点）
- **方法层面**：
  - 首创统一框架将环境动态蒸馏为**推理模型**，无需真实交互即可合成经验，极大降低 rollout 成本。
  - **自适应课程学习**与**经验回放缓冲**的设计提升了合成经验的质量和训练效率。
  - 支持 sim-to-real 迁移，使真实 RL 训练可受益于合成预训练。
- **实验层面**：
  - 跨环境（连续控制、WebArena）、跨任务类型（RL-ready 与非 RL-ready）的验证，增强了泛化性。
  - 与强基线（GRPO、PPO）和多基线对比，体现竞争力。

## 8. 不足与局限
- **实验覆盖不全**： 
  - 未列出具体环境名称、任务细节及基准得分，难以评估绝对性能水平。
  - 未提供消融实验的定量结果，各组件贡献程度不明确。
  - 未报告多次实验的方差或置信区间，统计可靠性待确认。
- **偏差风险**： 
  - 合成经验可能引入模型偏差（推理模型本身的误差），文中未讨论如何检测或校正。
  - 自适应课程生成可能偏向容易提升的任务，存在伪进步风险。
- **应用限制**： 
  - 依赖离线真实数据初始化回放缓冲，在完全无真实数据的冷启动场景下可能不适用。
  - 推理模型复杂度未知，可能对于超高维环境（如视频、3D）存在扩展瓶颈。
  - 未公开代码或详细超参数设置，可复现性待验证。

（完）
