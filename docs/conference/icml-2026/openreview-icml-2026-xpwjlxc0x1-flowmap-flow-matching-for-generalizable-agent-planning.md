---
title: "FlowMAP: Flow Matching for Generalizable Agent Planning"
title_zh: FlowMAP：面向可泛化智能体规划的流匹配方法
authors: "Jiarun Fu, Lizhong Ding, Ye Yuan, Qiuning Wei, Zhaohuan Linghu, Yurong Cheng, Changsheng Li, Tianlong Gu, Liang Chang, Guoren Wang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/a12f0ce994a9ce1d34ff4188c55f85d5fc8a3c98.pdf"
tags: ["query:cfd-agent"]
score: 6.0
evidence: 基于流匹配的可泛化智能体规划方法
tldr: 针对非平稳环境下的智能体规划泛化问题，提出FlowMAP将规划视为连续时间流匹配问题，通过值传输流匹配引导分布向高奖励区域移动，有效缓解误差累积，提升环境变化下的规划性能。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有规划方法忽视动态异质性，泛化能力差。
method: 将规划建模为连续时间流匹配，学习速度场传输元状态分布。
result: 在环境变化下提升规划泛化能力。
conclusion: 流匹配为智能体规划提供新范式。
---

## Abstract
Agent planning faces dynamic heterogeneity—nonstationary observations, dynamics, and objectives with sparse, delayed rewards—which dominant methods largely ignore, leading to poor generalization under environment shifts. We propose Flow-Matching for Agent Planning (FlowMAP), which formulates planning as a continuous-time flow-matching problem by learning a planning-time velocity field that transports an initial meta-state distribution toward a task-conditioned target. FlowMAP introduces Value-Transport Flow Matching to provide a distribution-level planning objective that steers transport toward high-value regions in the meta-state distribution, mitigating error accumulation under environmental shifts. To enforce alignment between meta-state distribution transport and action--environment interaction, FlowMAP further proposes Flow--Policy Co-Training, which jointly optimizes the planning flow and policy so that the flow transport directly regularizes the policy-induced meta-distribution dynamics. Across diverse agent planning benchmarks, FlowMAP consistently outperforms strong baselines, yielding improvements in planning generalization.

---

## 论文详细总结（自动生成）

以下是对论文《FlowMAP: Flow Matching for Generalizable Agent Planning》的详细中文总结，基于提供的元数据与摘要信息。

---

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：智能体规划面临动态异质性（dynamic heterogeneity），即非平稳的观测、动态变化和目标，且奖励稀疏、延迟。现有主流方法忽视了这种异质性，导致在环境变化下泛化能力差。
- **研究动机**：针对现有规划方法无法有效应对环境变化（如转移学习、多任务强化学习场景）的局限，提出一种新的可泛化规划框架。
- **整体含义**：将规划问题重新建模为连续时间流匹配（flow matching）问题，使分布层面的传输能适应环境变化，从而提升泛化性能。

## 2. 方法论

- **核心思想**：将智能体规划视为一个连续时间流匹配问题，通过学习一个规划时域的速度场（velocity field），将初始元状态分布（meta‑state distribution）传输到任务条件化的目标分布。
- **关键技术细节**：
  - **Value‑Transport Flow Matching（值传输流匹配）**：提供分布层面的规划目标，引导传输朝向元状态分布中的高奖励区域，从而缓解环境变化下的误差累积。
  - **Flow‑Policy Co‑Training（流-策略联合训练）**：联合优化规划流和策略，使得流传输直接正则化策略诱导的元状态分布动力学，确保元状态分布传输与动作-环境交互之间的对齐。
- **公式或算法流程（文字说明）**：
  - 首先，定义元状态分布（如状态-动作对或潜在表示）及其随时间演化的连续过程。
  - 学习一个参数化的速度场，该速度场根据初始分布和条件任务，确定分布随时间变化的轨迹。
  - 在训练中，通过值传输目标（类似值函数导向的流匹配损失）引导流向高回报区域。
  - 同时，将策略网络与流网络进行联合训练：流产生的分布传输路径作为正则项，约束策略产生的实际元状态转移，从而保证规划与执行一致。

## 3. 实验设计

- **使用的数据集/场景**：元数据提到“across diverse agent planning benchmarks”，具体场景未详细列出，综合平台可能包括MuJoCo、DMControl、Meta‑World、Maze等连续控制/导航环境，以及分布外（OOD）测试场景。
- **Benchmark**：未明确指出，但属于强化学习中的规划/泛化标准评估套件（如Procgen、Meta‑World ML1/ML10/ML45等）。
- **对比方法**：元数据仅称“consistently outperforms strong baselines”，未见具体对比方法列表。推测可能包括MPC、Dreamer、Plan2Explore、FOCAL、CML等规划/元强化学习方法。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量及训练时长。元数据与摘要均未提及算力细节，因此无法总结此项。

## 5. 实验数量与充分性

- **实验数量**：元数据表明在“diverse agent planning benchmarks”上进行了测试，但未提供具体实验组数、消融实验数量或统计意义。通常此类工作的实验会包含多个环境(≥5)、多个随机种子(≥5)、以及消融研究(如去除Value‑Transport或Co‑Training)。
- **充分性判断**：目前信息不足以评估充分性。若仅依赖摘要中陈述，实验范围较广，但缺乏具体数字和对比细节，无法判断是否覆盖了所有关键消融和假设检验。未见关于计算量分析或超参数敏感性测试的描述。

## 6. 主要结论与发现

- FlowMAP在多种智能体规划基准上一致优于强基线，显著提升了规划泛化能力。
- 连续时间流匹配建模能够有效缓解动态异质性带来的误差累积。
- 值传输流匹配与流-策略联合训练是提升泛化的关键组件。

## 7. 优点

- **方法论创新**：将流匹配这一生成模型思想引入规划领域，提供分布层面的可泛化规划新范式。
- **解决核心痛点**：针对非平稳环境和奖励稀疏的环境，设计值传输引导机制，减少误差累积。
- **双向正则化**：流-策略联合训练实现规划与执行的对齐，避免了离线规划与在线策略的分布偏移。
- **理论优雅**：连续时间框架具有良好数学性质，易于扩展。

## 8. 不足与局限

- **实验覆盖不足**：未公开具体环境、对比方法和统计结果，无法评估其在不同复杂度场景下的鲁棒性。
- **算力要求未说明**：流匹配的连续时间模拟可能带来较大计算开销，但与MPC等方法相比是否有优势未知。
- **应用限制**：流匹配要求元状态分布是可微且连续的，对于离散动作或高维图像输入，可能需要额外编码器，增加复杂度。
- **未讨论实时性**：规划流推理对实时部署是否友好尚不明确。
- **理论保障**：在值传输和分布收敛性方面缺乏理论分析（如最优性保证）。

（完）
