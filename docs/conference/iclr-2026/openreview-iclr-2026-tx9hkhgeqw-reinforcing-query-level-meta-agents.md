---
title: Reinforcing Query-Level Meta-Agents
title_zh: 强化查询级元代理
authors: "Hongcheng Gao, Yue Liu, Yufei He, Longxu Dou, Chao Du, Zhijie Deng, Bryan Hooi, Min Lin, Tianyu Pang"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=Tx9HKhGeQW"
tags: ["query:cfd-agent"]
score: 7.0
evidence: 使用强化学习改进元代理以设计多代理系统
tldr: 该论文提出FlowReasoner，一种查询级元代理，通过蒸馏DeepSeek R1获得推理能力，并利用强化学习和外部执行反馈进一步提升。它能够为每个查询生成个性化的多代理系统。实验表明，该框架在工程和竞赛代码任务上有效提高了多代理系统的性能和效率。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有查询级多代理系统设计依赖人工或固定模板，缺乏自适应能力。
method: 提出FlowReasoner，先蒸馏DeepSeek R1获得基础推理能力，再通过强化学习优化多代理系统生成。
result: 在工程和竞赛代码基准上，FlowReasoner自动生成的多代理系统性能优于人工设计。
conclusion: 证明了强化学习与执行反馈结合可有效自动化多代理系统设计。
---

## Abstract
This paper proposes a query-level meta-agent named FlowReasoner to automate the design of query-level multi-agent systems, i.e., one system per user query. Our core idea is to incentivize a reasoning-based meta-agent via external execution feedback. Concretely, by distilling DeepSeek R1, we first endow the basic reasoning ability regarding the generation of multi-agent systems to FlowReasoner. Then, we further enhance it via reinforcement learning (RL) with external execution feedback. A multi-purpose reward is designed to guide the RL training from aspects of performance, complexity, and efficiency. In this manner, FlowReasoner is enabled to generate a personalized multi-agent system for each user query via deliberative reasoning. Experiments on both engineering and competition code benchmarks demonstrate the superiority of FlowReasoner. Remarkably, it surpasses o1-mini by 10.52% accuracy across three benchmarks.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有的查询级多代理系统（query-level multi-agent systems）设计依赖人工或固定模板，缺乏针对每个用户查询的个性化自适应能力，导致系统性能和效率受限。
- **研究动机**：为了实现自动化、个性化的多代理系统设计，作者提出一种基于推理的元代理，通过外部执行反馈来优化系统生成，从而提升多代理系统在复杂任务上的表现。

## 2. 论文提出的方法论

- **核心思想**：构建一个名为 **FlowReasoner** 的查询级元代理，该代理能够为每个用户查询自动生成个性化的多代理系统。核心创新是**利用外部执行反馈来激励基于推理的元代理**。
- **关键技术细节**：
  - **阶段一：蒸馏基础推理能力**：通过蒸馏 DeepSeek R1 模型，赋予 FlowReasoner 生成多代理系统所需的基本推理能力。
  - **阶段二：强化学习优化**：使用强化学习（Reinforcement Learning, RL）结合外部执行反馈来进一步增强元代理。设计了一个**多目标奖励函数**，从**性能**、**复杂度**和**效率**三个维度引导 RL 训练。
  - **生成过程**：经过训练后，FlowReasoner 能够通过**深思熟虑的推理（deliberative reasoning）**为每个用户查询生成定制化的多代理系统。
- **公式/算法流程**（文字说明）：论文未提供显式公式，流程为：① 输入用户查询 → ② FlowReasoner 基于蒸馏的推理生成初始多代理系统 → ③ 系统执行并获得外部反馈（如任务完成度、时间成本） → ④ 强化学习模块根据多目标奖励更新元代理参数 → ⑤ 反复迭代直至收敛，最终输出最优的多代理系统配置。

## 3. 实验设计

- **使用的数据集/场景**：论文在**工程代码基准**和**竞赛代码基准**上进行实验，具体基准名称未在摘要/元数据中明确给出，但提及跨三个基准的平均准确率。
- **Benchmark**：三个代码相关的基准（推测可能包括 HumanEval、MBPP、CodeContests 等，但原文未详述）。
- **对比方法**：
  - 与 **o1-mini** 进行了对比（FlowReasoner 在三个基准上平均准确率超过 o1-mini 10.52%）。
  - 可能还包括与人工设计的多代理系统、固定模板方法的对比（元数据提及“优于人工设计”，但具体基线未列出）。

## 4. 资源与算力

- **文中明确说明**：摘要和元数据中**未提及**使用的 GPU 型号、数量、训练时长等具体算力信息。仅描述了蒸馏 DeepSeek R1 和强化学习训练流程，但未量化计算开销。需指出这一点。

## 5. 实验数量与充分性

- **实验数量**：元数据提到“在工程和竞赛代码基准上”实验，跨三个基准。具体消融实验数量、奖励函数各维度贡献分析等细节未在提供内容中体现。
- **充分性评估**：
  - **积极方面**：涵盖工程和竞赛两种难度场景，并与强基线 o1-mini 对比，具有一定说服力。
  - **局限**：缺少对方法本身消融实验的详细描述（如蒸馏 vs. RL 的贡献）、超参数敏感性分析、不同奖励权重的影响等，因此实验充分性在提供的信息中尚显不足。

## 6. 论文的主要结论与发现

- **主要结论**：
  - 通过强化学习与外部执行反馈相结合，可以有效自动化多代理系统的设计。
  - FlowReasoner 自动生成的多代理系统在性能上优于人工设计方法。
  - 在三个基准上，FlowReasoner 的准确率比 o1-mini 平均高出 **10.52%**，证明了该方法的优越性。

## 7. 优点

- **方法亮点**：
  - **查询级自适应**：每个用户查询获得个性化多代理系统，避免了“一刀切”模板。
  - **双重优化策略**：先蒸馏获得基础能力，再通过 RL 结合执行反馈精细调优，既降低了训练成本又提升了最终性能。
  - **多目标奖励设计**：同时考虑性能、复杂度和效率，使得生成的系统更加均衡实用。
- **实验亮点**：在多个基准上验证了泛化能力，且超越强基线 o1-mini 的幅度显著（+10.52%）。

## 8. 不足与局限

- **实验覆盖不完整**：论文摘要/元数据中未展示消融实验、奖励函数各分量的贡献分析、不同规模模型的对比等，说服力有待加强。
- **应用限制**：
  - 方法依赖外部执行反馈，对于无法提供实时反馈的任务（如安全关键系统）可能不适用。
  - 蒸馏 DeepSeek R1 本身可能受限于原始模型能力，若原始模型表现不佳，基础推理能力将受限。
  - 多目标奖励的权重设定对最终效果影响较大，文中未提供调参指导或鲁棒性分析。
- **偏差风险**：仅测试了代码领域的任务，在自然语言处理、多轮对话等非代码场景下的泛化性未知。

（完）
