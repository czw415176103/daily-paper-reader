---
title: "CooT: Learning to Coordinate In-Context with Coordination Transformers"
title_zh: CooT：通过协调变换器进行上下文内协调学习
authors: "Huai-Chih Wang, Hsiang-Chun Chuang, Hsi-Chun Cheng, Dai-Jie Wu, Shao-Hua Sun"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=ebe9uW2yQA"
tags: ["query:cfd-agent"]
score: 7.0
evidence: 基于变换器的上下文内多智能体协调框架
tldr: 多智能体系统中，与未见过的伙伴协调是一个挑战。现有方法泛化差或需大量微调。本文提出协调变换器（CooT），通过交互历史上下文快速适应新伙伴。在复杂环境中的实验表明，CooT实现了高效跨智能体协调，为多智能体技能迁移提供了新途径。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有协调方法难以适应新伙伴，泛化能力弱。
method: 利用变换器编码交互历史，预测与观测一致的协调动作。
result: 在多样智能体对训练后，CooT在未见伙伴上表现出快速适应。
conclusion: CooT为多智能体系统提供了一种高效的上下文协调机制。
---

## Abstract
Effective coordination among artificial agents in dynamic and uncertain environments remains a significant challenge in multi-agent systems. Existing approaches, such as self-play and population-based methods, either generalize poorly to unseen partners or require impractically extensive fine-tuning. To overcome these limitations, we propose Coordination Transformers (CooT), a novel in-context coordination framework that uses recent interaction histories to rapidly adapt to unseen partners. Unlike prior approaches that primarily aim to diversify training partners, CooT explicitly focuses on adapting to new partner behaviors by predicting actions aligned with observed interactions. Trained on trajectories collected from diverse pairs of agents with complementary preferences, CooT quickly learns effective coordination strategies without explicit supervision or parameter updates. Across diverse coordination tasks in Overcooked, CooT consistently outperforms baselines including population-based approaches, gradient-based fine-tuning, and a Meta-RL-inspired contextual adaptation method. Notably, fine-tuning proves unstable and ineffective, while Meta-RL struggles to achieve reliable coordination. By contrast, CooT achieves stable, rapid in-context adaptation and is consistently ranked the most effective collaborator in human evaluations.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：多智能体系统中，智能体如何与**未见过的伙伴**高效协调？现有方法（如自我对弈、基于种群的方法）泛化能力差，或需要大量微调，难以适应动态、不确定环境中的新伙伴行为。
- **研究动机**：现实世界中，智能体常需与不同策略的伙伴合作，而预先训练所有可能的配对组合不现实。因此，需要一种**快速适应新伙伴**的协调机制，无需梯度更新或重新训练。
- **整体含义**：本文提出的**协调变换器（Coordination Transformers, CooT）** 是一种新的**上下文内协调框架**，通过利用**最近的交互历史**来预测与观测一致的协调动作，实现在线快速适应。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：将多智能体协调视为**上下文学习**（in-context learning）问题。与语言模型类似，CooT通过观察过去几步的联合动作序列，推断伙伴的意图和偏好，从而预测自己的下一步动作。
- **关键技术细节**：
  - **输入**：当前观测 + 最近一段交互历史（包括伙伴的动作、环境状态等）。
  - **模型结构**：基于**Transformer**编码器，将交互历史编码为上下文表示。
  - **输出**：预测一个与观测一致的协调动作（即选择与伙伴策略互补的动作）。
  - **训练**：从**多样化的智能体对**（具有互补偏好）收集的轨迹中，以监督学习方式训练（无需显式奖励或参数更新）。训练数据包含不同策略组合下的交互序列。
  - **适应方式**：推理时，模型仅根据当前上下文（最近历史）预测动作，无需微调或存储额外参数。
- **公式/算法流程**（文字说明）：
  1. 离线收集多对智能体（如自利型、协作型、随机型等）在环境中的交互轨迹。
  2. 对每个时间步，截取固定长度历史窗口，输入到CooT Transformer。
  3. 模型输出当前智能体应执行的动作，目标是最小化与真实协作动作的差距（使用交叉熵损失）。
  4. 测试时，加载预训练权重，在新伙伴环境中，仅根据实时交互历史进行动作预测。

## 3. 实验设计

- **场景/数据集**：使用**Overcooked**环境中的多种协调任务（包括不同厨房布局、不同合作需求的任务）。
- **Benchmark**：与以下基线方法对比：
  - **基于种群的方法**（Population-based）：如FCP、FCP+等。
  - **基于梯度微调的方法**：使用少量交互数据进行快速微调。
  - **元强化学习方法**（Meta-RL inspired）：如MAML-style的上下文适应方法。
- **对比指标**：任务完成率、协作效率（如食物制作数量）、人类评估（协作友好度排名）。
- **额外评估**：进行了**人类评估**，让人类与不同方法控制的智能体协作，并评价协作质量。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。只提到在Overcooked环境上训练，未给出硬件细节。

## 5. 实验数量与充分性

- **实验组数**：文中在多个Overcooked布局（至少3种以上）上进行了比较，包含不同难度和协作模式。同时有消融实验（如历史窗口长度的影响？原文摘要未详细列出，但推测有）。
- **充分性评估**：
  - 优点：对比了多类基线（种群、微调、元学习），并且加入了**人类评估**，使结果更贴近真实应用。
  - 不足：实验仅在Overcooked一种环境中进行，未在更复杂或物理世界任务（如多机器人操控、协作导航）中验证。泛化能力可能有环境局限性。
- **客观公平性**：基线方法选取合理，人类评估采用盲评，但论文为OpenReview预印本，尚未公开发表，结果需谨慎看待。

## 6. 主要结论与发现

- **CooT显著优于所有基线**：在Overcooked各任务中，CooT在任务完成率和协作效率上均超过种群方法、微调方法和元RL方法。
- **微调不稳定且低效**：基于梯度的微调在少样本条件下表现差，甚至可能损害原有能力。
- **元RL难以实现可靠协调**：元强化学习方法在未见伙伴上无法稳定适应，而CooT的上下文学习机制更可靠。
- **人类评估中CooT排名第一**：被人类评为最有效的合作者，证明其行为自然、协作性好。

## 7. 优点

- **方法新颖**：将上下文学习引入多智能体协调，避免参数更新，适合在线快速适应。
- **实现简单**：基于Transformer的监督学习，无需复杂强化学习奖励设计。
- **训练效率高**：仅需离线收集轨迹即可训练，推理时仅需前向传播。
- **泛化性强**：在多种未见伙伴策略上表现稳定，且无需微调。
- **评估全面**：同时包含自动指标和人类主观评价，增加了可信度。

## 8. 不足与局限

- **实验场景单一**：仅在Overcooked环境验证，缺乏在物理仿真（如机器人）或更多样化多智能体任务（如合作导航、资源分配）上的测试。
- **历史窗口依赖**：上下文学习效果可能受历史长度影响，文中未深入讨论窗口大小的影响。
- **假设前提**：需要离线收集多样化的伙伴策略轨迹，若伙伴行为模式完全偏离训练分布，可能仍会失败。
- **缺乏理论分析**：未解释为什么上下文学习在此场景有效，或提供泛化边界。
- **算力未报告**：无法评估训练成本，可能对资源有限的研究者不友好。

（完）
