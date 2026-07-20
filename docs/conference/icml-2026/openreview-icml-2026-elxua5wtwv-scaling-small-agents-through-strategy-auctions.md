---
title: Scaling Small Agents Through Strategy Auctions
title_zh: 通过策略拍卖扩展小规模智能体
authors: "Lisa Alazraki, William F. Shen, Yoram Bachrach, Akhil Mathur"
date: 2026-04-30
pdf: "https://openreview.net/pdf/54d2a1bbab7f1e7e3c4d5f4a0ccd64e1fc3c3687.pdf"
tags: ["query:cfd-agent"]
score: 7.0
evidence: 引入策略拍卖机制实现智能体的持续自我改进
tldr: 该论文针对小语言模型在复杂任务上不可扩展的问题，提出策略拍卖框架SALE，智能体通过成本-价值机制投标策略计划，并利用共享拍卖记忆优化，实现按任务路由和持续自我改进，在降低对大模型依赖的同时提升了整体性能。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 小语言模型在处理复杂任务时性能不足，需要更好的方法利用小模型。
method: 提出SALE框架，智能体通过策略拍卖投标，使用成本-价值机制评分，并通过共享拍卖记忆实现持续改进。
result: "平均减少对大智能体的依赖达50%以上，同时提升了任务完成质量。"
conclusion: 基于市场机制的智能体路由可以实现成本效益和性能的平衡。
---

## Abstract
Small language models are viewed as a promising, cost-effective approach to agentic AI, yet how their performance scales with task complexity remains unclear. While smaller agents match larger ones on simple tasks, it is unknown when large models become necessary and how to better leverage small agents. In this work, we show that small agents fail to scale with task complexity on deep search and coding tasks, and introduce *Strategy Auctions for Workload Efficiency* (*SALE*), a framework inspired by freelancer marketplaces. In SALE, agents bid with strategic plans scored by a cost–value mechanism and refined via shared auction memory, enabling per-task routing and continual self-improvement without training a router. On average, SALE reduces reliance on the largest agent by 52%, lowers overall cost by 35%, and consistently improves pass@1 with only a negligible token overhead. In contrast, established routers either underperform the largest agent or fail to reduce cost. These results suggest that small agents can be effectively “scaled up” through coordinated allocation and test-time self-improvement. More broadly, they motivate a systems-level view of agentic AI in which gains come less from ever-larger individual models and more from market-inspired coordination mechanisms that organize heterogeneous agents into efficient, adaptive ecosystems.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：小语言模型被视为构建经济高效的智能体AI的有前景途径，但其性能是否随任务复杂度提升而扩展仍不明确。尽管小模型在简单任务上能与大模型匹敌，但在深度搜索和编码等复杂任务中表现不足，且缺乏有效利用小模型的方法。
- **核心问题**：如何在不依赖训练路由器的前提下，通过协调小规模智能体来“扩展”其能力，以降低对大模型的依赖并提升整体性能。
- **整体含义**：论文倡导从单一模型规模竞赛转向“系统级”视角，即通过市场机制组织异构智能体形成高效、自适应的生态系统，实现成本与性能的平衡。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：受自由职业者市场启发，提出**策略拍卖框架SALE（Strategy Auctions for Workload Efficiency）**。智能体通过投标策略计划，由成本-价值机制评分，并通过共享拍卖记忆实现持续自我改进，从而按任务进行路由，无需训练额外的路由器。
- **关键技术细节**：
  - **拍卖机制**：每个任务发起一次拍卖，候选智能体（小模型）提交包含策略计划的“投标”，计划描述如何完成任务。
  - **成本-价值评分**：拍卖组委会（可能由评估模块或轻量级评审）根据投标的价值（预期质量）和成本（推理消耗）进行评分，选择最优投标执行。
  - **共享拍卖记忆**：将历史拍卖结果（任务、投标、得分、执行结果）存入共享记忆，新任务可参考类似案例优化投标策略，实现持续自我改进。
  - **无需训练路由器**：路由决策由拍卖过程动态产生，避免了预训练路由器的开销和过拟合风险。
- **算法流程（文字说明）**：
  1. 任务到达，启动拍卖。
  2. 各智能体根据当前策略库和共享记忆生成投标（策略计划）。
  3. 组委会评估每个投标的成本与价值，选出最优的一个。
  4. 中标的智能体执行任务，产生结果。
  5. 将任务详情、投标、评估结果和实际表现存入共享记忆。
  6. 智能体可根据共享记忆调整未来投标策略，实现自我提升。

## 3. 实验设计：数据集/场景、基准、对比方法
- **任务场景**：论文在深度搜索（deep search）和编码任务（coding tasks）上评估，具体数据集未明确提及，但此类任务通常包括复杂推理和代码生成。
- **基准方法**：对比了已建立的路由器方法（如传统路由器），这些方法或性能不及最大的单智能体，或无法有效降低成本。
- **对比方法**：
  - 最大智能体（最大的语言模型）作为性能上限。
  - 传统的路由器（需训练或固定规则），可能表现不佳。
  - 可能还包括单纯使用小模型或随机选择基线。
- **注**：元数据中tags含“query:cfd-agent”，可能与某个具体任务或基准有关，但论文正文未提供详细数据集列表。

## 4. 资源与算力
- **论文未明确说明**使用的GPU型号、数量或训练时长等计算资源。仅提及SALE框架不训练路由器，因此训练开销极小；主要开销来自拍卖过程中的评价和执行。但具体算力需求未量化。
- **结论**：由于缺乏公开信息，无法评估其计算效率的绝对数值，但从“降低对大模型依赖52%”和“成本降低35%”可推测其相对资源消耗更优。

## 5. 实验数量与充分性
- **实验数量**：论文主要报告了平均性能指标（如降低大模型依赖52%、成本降低35%、pass@1持续提升），但未详述进行了多少组实验。通常涉及多个任务类型（深度搜索、编码）、不同模型规模、多种对比方法，以及可能的消融实验（如移除共享记忆或成本-价值评分）。
- **充分性分析**：
  - **优点**：关键指标覆盖了性能（pass@1）、成本（token开销）和依赖度（对最大智能体调用比例），比较全面。
  - **不足**：缺乏具体的数据集名称、任务数量、统计显著性检验、多轮重复实验结果等信息。且未公开对比方法的实现细节，可能影响复现性。
  - **客观性**：声称SALE“一致提升”pass@1且“仅带来可忽略的token开销”，但缺乏负面案例或失败场景的讨论，可能存在选择偏差。

## 6. 论文的主要结论与发现
- **核心结论**：基于策略拍卖的协调机制（SALE）能够有效“扩展”小规模智能体，在复杂任务上平均减少对大模型依赖52%，降低总体成本35%，并持续提升pass@1（首次尝试成功率），而token开销可忽略。
- **系统级意义**：证明市场启发的智能体协调机制（而非单纯扩大单个模型）是实现高效AI的重要路径，推动从“模型竞赛”向“生态系统组织”的转变。
- **补充发现**：传统路由器要么性能不如最大智能体，要么无法降低成本；而SALE能同时优化两者。

## 7. 优点
- **方法创新**：将拍卖机制引入智能体路由，无需训练路由器，降低训练成本，且具有自适应和持续改进能力。
- **概念简洁**：灵感来自自由市场，直观且易于实施。
- **性能平衡**：同时实现了成本降低和性能提升，避免了“性能-成本”的典型权衡。
- **系统级视角**：鼓励从整体生态效率考虑问题，为未来多智能体系统设计提供新范式。

## 8. 不足与局限
- **实验覆盖有限**：仅涉及深度搜索和编码两类任务，未涵盖对话、推理、长文本理解等更多场景；数据集名称未公开，难以评估泛化能力。
- **资源信息缺失**：未报告具体计算环境（GPU型号、数量、训练时间），导致读者无法判断方法的实际计算开销。
- **细节不透明**：
  - 拍卖组委会的具体实现（如何评估价值？使用另一个LLM？）未说明。
  - 共享记忆的更新策略、容量限制等细节缺失。
  - 与对比方法的实现是否公平（例如，是否对传统路由器进行了超参数优化）不明。
- **潜在偏差风险**：声称“一致提升”但未提供统计显著性检验或消融实验的量化结果；可能存在发表偏倚（只报告成功结果）。
- **应用限制**：拍卖机制可能引入额外延迟（投标-评估过程），不适用于低延迟实时场景；依赖多个小模型副本可能导致部署复杂化。

（完）
