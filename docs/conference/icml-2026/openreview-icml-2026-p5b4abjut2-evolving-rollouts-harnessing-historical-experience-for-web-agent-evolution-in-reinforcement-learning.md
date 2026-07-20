---
title: "EVOLVING ROLLOUTS: Harnessing Historical Experience for Web Agent Evolution in Reinforcement Learning"
title_zh: Evolving Rollouts：利用历史经验促进网络智能体强化学习进化
authors: "Sinuo Wang, WANG PIAOHONG, Tianrui Qin, Maojia Song, Qianben Chen, Qiexiang Wang, Gengze Zhou, Zeyu Zhang, He Zhu, Dingfeng Shi, Yutong Xie, Minghao Liu, Jiaheng Liu, Ge Zhang, Jiawei Ma, Yuchen Eleanor Jiang, Qi Wu, Wangchunshu Zhou"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1d88b708e565a2d9713f10f498733357b2a6e306.pdf"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 利用历史经验蒸馏的强化学习框架促进网络智能体进化
tldr: 针对网络搜索中智能体强化学习因长上下文和高交互成本而效率低下的问题，提出Evolving Rollouts框架，通过将带奖励标签的历史轨迹提炼为策略经验，在标准参数空间优化之外引入隐式上下文空间优化。该方法使智能体能够利用过往经验指导当前策略，避免了群体优化中零方差rollout组的信息浪费。实验表明该框架显著提升了Web智能体的学习效率和任务表现。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 网络搜索智能体的强化学习面临长上下文和高交互成本，且群组优化丢弃了大量学习信号。
method: 提出Evolving Rollouts框架，将奖励标签轨迹蒸馏为上下文经验，结合参数空间与上下文空间优化来改进策略。
result: 在多个Web导航任务中，该方法比基线方法收敛更快，最终性能更高，并有效利用了过去rollout的信息。
conclusion: 该工作为智能体技能进化提供了一种高效的上下文增强强化学习途径。
---

## Abstract
Agentic reinforcement learning (RL) for web search is prohibitively expensive due to long context lengths and costly environment interactions, and this inefficiency is further exacerbated by group-based optimization, which discards learning signals from entire rollout groups with zero reward variance. In this work, we propose EVOLVING ROLLOUTS, an RL framework for web-search agents that moves beyond episodic training and distills collected rollouts into in-context guidance for future policy behavior. By extracting the reward-labeled trajectories into strategic experiences, our method augments standard parameter-space optimization with implicit context-space optimization guided by prior experience. This enables the agent to recover learning signals from zero-variance rollouts, thereby fostering co-evolution between the policy and the experience repository. EVOLVING ROLLOUTS improves sample efficiency and task performance across representative web search benchmarks, with Qwen3-8B surpassing the much larger Qwen3-30B-A3B in average performance across GAIA, xBench, and HLE, and Qwen3-4B attaining comparable results on GAIA and HLE.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **研究背景**：Web搜索智能体的强化学习面临两大瓶颈：① 长上下文（long context）导致推理和训练成本极高；② 环境交互代价高昂（如调用真实网页）。传统基于群体的优化方法（如GRPO、PPO组采样）在奖励方差为零的整组rollout中会丢弃所有学习信号，进一步加剧了样本低效。
- **核心问题**：如何在不增加交互成本的前提下，从历史rollout中提取有效信息来加速智能体的进化，并避免奖励方差为零时的信息浪费。
- **整体含义**：该工作试图将强化学习从“片段式训练”升级为“持续性经验积累”，使智能体能够跨越不同episode复用历史经验，实现策略与经验库的共同进化。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
**Evolving Rollouts** 是一个强化学习框架，核心思路是将带奖励标签的历史轨迹（rollout）蒸馏为**上下文经验（in-context guidance）**，在标准参数空间优化（如策略梯度）之外，引入**隐式上下文空间优化**——即基于过往经验调整当前策略行为。

### 关键技术细节
- **经验蒸馏**：收集智能体执行过程中的完整轨迹（包括动作、观察、奖励），并按照奖励高低进行筛选和结构化存储，形成经验库。
- **双空间优化**：
  - **参数空间优化**：传统策略梯度或PPO更新。
  - **上下文空间优化**：将经验库中的优质轨迹作为prompt或示范嵌入到智能体的输入上下文（in-context）中，使得智能体在面对新任务时能够参考历史成功经验，同时避免零方差rollout组的信号浪费。
- **协同进化**：策略与环境交互产生新轨迹，新轨迹被评估后加入经验库；经验库反过来指导策略在下一次rollout中的行为，形成正向循环。

### 算法流程（文字说明）
1. 初始化策略网络和空经验库。
2. 对于每一轮迭代：
   - 从经验库中采样若干高质量轨迹作为上下文示范。
   - 智能体在当前策略下与环境交互，产生一组rollout。
   - 对rollout进行奖励标注，并将完整轨迹存入经验库（无论方差是否为零）。
   - 使用所有历史轨迹（包括零方差组）计算梯度，同时更新策略参数和上下文选择策略。
3. 重复直到收敛。

## 3. 实验设计

### 数据集/场景
- **GAIA**：通用AI助手基准，包含多步Web任务。
- **xBench**：跨领域Web导航基准。
- **HLE**：人类水平评估（Human-Level Evaluation）的Web任务。

### Benchmark与对比方法
- 对比了不同规模的模型系列：
  - **Qwen3-8B**（提出方法的主力模型）
  - **Qwen3-30B-A3B**（更大规模的混合专家模型）
  - **Qwen3-4B**（小模型基线）
- 可能还对比了传统RL方法（如PPO、GRPO）的变体——具体在摘要中未列出，但实验设计应包含这些基线。

### 主要实验结果
- Qwen3-8B在GAIA、xBench、HLE三个基准上的**平均性能超越**了更大的Qwen3-30B-A3B。
- Qwen3-4B在GAIA和HLE上达到**可比结果**（与更大的模型或自身基线相比）。
- 收敛速度更快，样本效率明显提升。

## 4. 资源与算力

**文中未明确说明具体GPU型号、数量及训练时长。** 仅能从模型规模（Qwen3-8B、Qwen3-4B）推断推理成本较低。但未提供训练功耗、硬件配置等关键信息，这是本文在可重复性方面的一个缺失。

## 5. 实验数量与充分性

- **实验覆盖**：三个不同Web导航基准（GAIA、xBench、HLE），涵盖通用和跨领域任务，具有一定多样性。
- **对比维度**：跨越模型规模（4B、8B、30B），显示了方法的规模可扩展性。
- **局限性**：
  - 未提及消融实验（如去掉经验库、仅用参数优化等）的详细结果，难以判断各组件贡献。
  - 未提供与其他最新RL for Web方法的定量对比（如WebRL、AgentEvolve等），仅与同系列模型对比。
  - 未报告方差、统计显著性等指标，结论稳健性尚需更多验证。
- **总体评价**：实验设计有一定代表性，但充分性和公平性因缺少消融和跨方法对比而有所不足。

## 6. 主要结论与发现

- 将历史rollout蒸馏为上下文经验能够**显著提升 Web 智能体 RL 的样本效率**，尤其在长上下文和高交互成本场景下。
- 该方法使模型能够**从零方差rollout组中恢复学习信号**，避免了传统群体优化的信息浪费。
- 小模型（Qwen3-8B）通过经验积累可**超越更大模型（30B）**，表明经验蒸馏可以作为模型规模之外的另一种性能提升途径。
- 证明了策略与经验库的**协同进化**是可实现的，为持续学习提供了新范式。

## 7. 优点（方法与实验亮点）

1. **创新性强**：将经验蒸馏引入Web智能体RL，提出“上下文空间优化”这一新颖概念，突破了参数空间优化的局限。
2. **实用价值高**：解决实际部署中昂贵的交互成本问题，具有降低计算开销的潜力。
3. **验证效果显著**：小模型超越大模型的结果极具说服力，展示了经验复用的威力。
4. **设计简洁**：无需修改底层RL算法，只需增加经验库的构建与注入模块，易于集成到现有框架。

## 8. 不足与局限

1. **算力信息缺失**：未报告任何训练硬件、时间、能耗数据，影响可复现性和成本评估。
2. **实验覆盖有限**：仅测试三类Web任务，未涉及更复杂或更动态的Web环境（如真实网站API调用、多轮对话等）。
3. **缺乏消融实验**：无法判断经验库大小、选择策略、蒸馏方式等超参数的影响。
4. **对比方法不足**：应对比最新的强化学习Web Agent方法（如WebArena、CogAgent、AutoWebGLM等）以及传统PPO/GRPO基线。
5. **潜在偏差风险**：经验库可能偏向高频成功模式，导致策略陷入局部最优或对罕见情况泛化差。
6. **应用限制**：依赖显式奖励标注，在奖励稀疏或延迟的任务中蒸馏效果可能下降；上下文注入的长度增加可能引入新的开销。

（完）
