---
title: "Darwinian Memory: A Training-Free Self-Regulating Memory System for GUI Agent Evolution"
title_zh: 达尔文记忆：一种无需训练的自调节记忆系统用于GUI智能体进化
authors: "Hongze Mi, Yibo Feng, WenJie Lu, Song Cao, Jinyuan Li, Yanming Li, Xuelin Zhang, Haotian Luo, Songyang Peng, He Cui, Tengfei Tian, Jun Fang, Hua Chai, Naiqiang Tan"
date: 2026-04-30
pdf: "https://openreview.net/pdf/db9700e3ce01368639c8b929484815d1e360f418.pdf"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 通过自调节记忆系统实现GUI智能体进化
tldr: 针对多模态大语言模型智能体在长周期跨应用GUI任务中上下文窗口有限的问题，提出达尔文记忆系统（DMS），将记忆构建为动态生态系统，遵循适者生存法则自动调节记忆内容。DMS无需训练即可自适应演化，有效缓解了上下文污染和粒度不匹配问题。实验表明该方法显著提升了智能体在复杂GUI任务中的表现，为智能体技能进化提供了轻量级解决方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有记忆系统难以适应动态GUI环境，存在上下文污染和粒度不匹配问题，限制智能体在长周期任务中的表现。
method: 提出达尔文记忆系统（DMS），将记忆建模为动态生态系统，通过适者生存法则自动进化记忆内容，无需额外训练。
result: 在多项GUI自动化基准测试中，DMS在不需要训练的情况下显著优于现有记忆系统，提升任务完成率和鲁棒性。
conclusion: DMS为智能体记忆管理提供了一种高效、自适应的进化范式，可推广至各类需要长期记忆的智能体应用。
---

## Abstract
Multimodal Large Language Model (MLLM) agents facilitate Graphical User Interface (GUI) automation but struggle with long-horizon, cross-application tasks due to limited context windows. While memory systems provide a viable solution, existing paradigms struggle to adapt to dynamic GUI environments, suffering from a granularity mismatch between high-level intent and low-level execution, and context pollution where the static accumulation of outdated experiences drives agents into hallucination.  
  To address these bottlenecks, we propose the Darwinian Memory System (DMS), a self-evolving architecture that constructs memory as a dynamic ecosystem governed by the law of "survival of the fittest." DMS decomposes complex trajectories into independent, reusable units for compositional flexibility, and implements Utility-driven Natural Selection to track survival value, actively pruning suboptimal paths and inhibiting high-risk plans. This evolutionary pressure compels the agent to derive superior strategies. Extensive experiments on real-world multi-app benchmarks validate that DMS boosts general-purpose MLLMs without training costs or architectural overhead, achieving average gains of 18.0% in success rate and 33.9% in execution stability, while reducing task latency, establishing it as an effective self-evolving memory system for GUI tasks. The implementation is available at https://anonymous.4open.science/r/DMS-C48C.

---

## 论文详细总结（自动生成）

# 中文总结：达尔文记忆：一种无需训练的自调节记忆系统用于GUI智能体进化

## 1. 论文的核心问题与整体含义（研究动机与背景）

- **研究动机**：多模态大语言模型（MLLM）智能体在图形用户界面（GUI）自动化任务中表现出潜力，但在处理长周期、跨应用的复杂任务时，受限于有限的上下文窗口。
- **现有问题**：已有记忆系统难以适应动态GUI环境，存在两个关键瓶颈：
  - **粒度不匹配**：高层意图与底层执行细节之间缺乏对应。
  - **上下文污染**：静态积累的过时经验导致智能体产生幻觉。
- **核心目标**：设计一种无需额外训练、能自调节、自进化的记忆系统，提升GUI智能体在长周期任务中的表现。

## 2. 论文提出的方法论

- **核心思想**：将记忆构建为一个动态生态系统，遵循“适者生存”法则，实现记忆内容的自调节与进化。
- **关键技术细节**：
  - **轨迹分解**：将复杂轨迹分解为独立、可复用的单元，支持组合灵活性。
  - **效用驱动的自然选择**：跟踪每个记忆单元的生存价值，主动剪除次优路径，抑制高风险计划。
  - **进化压力**：推动智能体推导出更优策略，无需训练或架构修改。
- **算法流程（文字说明）**：
  1. 收集智能体在GUI任务中的历史轨迹。
  2. 将轨迹分解为独立的基本单元（如子任务步骤）。
  3. 初始化每个单元的生存价值（utility）。
  4. 在后续任务执行中，根据执行结果更新单元的效用值（成功率、耗时等）。
  5. 定期淘汰低效用单元，保留高效用单元，并允许新单元通过组合生成。
  6. 智能体在执行任务时优先从记忆生态系统中选择高效用单元。
- **特点**：无需训练、无需架构调整、轻量级、自适应演化。

## 3. 实验设计

- **数据集/场景**：真实世界的多应用GUI基准测试（multi-app benchmarks），具体名称未在摘要中给出，但提到是“wide range of real-world multi-app benchmarks”。
- **Benchmark**：未明确列出具体基准名称，但评估指标包括成功率（success rate）、执行稳定性（execution stability）和任务延迟（task latency）。
- **对比方法**：对比了现有记忆系统（existing memory systems）以及通用MLLM基线（general-purpose MLLMs without training costs or architectural overhead）。未列出具体对比方法名称。

## 4. 资源与算力

- **文中未明确说明**：未提及使用的GPU型号、数量、训练时长或推理算力消耗。
- **仅提到**：该方法无需训练成本（without training costs），因此算力开销主要集中在推理阶段，但具体数值未给出。

## 5. 实验数量与充分性

- **实验数量**：摘要中仅提到“extensive experiments”，但未列出具体实验组数。从评估指标（成功率、执行稳定性、延迟）来看，至少包含主实验和性能对比。
- **充分性**：
  - **主实验**：对比了DMS与现有记忆系统及基线，显示平均成功率提升18.0%，执行稳定性提升33.9%，任务延迟降低。
  - **消融实验**：未明确提及，但“Utility-driven Natural Selection”是核心机制，可能存在消融研究。
  - **客观性**：结果报告了具体的百分比增益，但缺少统计显著性检验和误差条。实验数据未公开更多细节，客观性有待进一步验证。

## 6. 论文的主要结论与发现

- DMS能够有效提升通用MLLM在GUI自动化任务中的表现，无需训练成本或架构改动。
- 平均成功率提升18.0%，执行稳定性提升33.9%，任务延迟降低。
- DMS通过自进化记忆系统缓解了上下文污染和粒度不匹配问题。
- 该方法为智能体技能进化提供了轻量级、可推广的解决方案。

## 7. 优点

- **无需训练**：不依赖额外训练数据和计算资源，可零成本集成到现有MLLM智能体中。
- **自适应进化**：记忆系统根据生存价值动态调整，适应动态GUI环境。
- **轻量级**：无需修改模型架构，易于部署和扩展。
- **实验增益显著**：在多个真实世界基准上取得明显提升，且同时改善成功率、稳定性和延迟。

## 8. 不足与局限

- **实验细节不充分**：未说明具体的基准名称、数据集规模、对比方法全名、消融实验设计、统计显著性等，导致复现和公平性评估困难。
- **算力资源缺失**：未报告推理阶段的GPU型号、时间或内存开销，难以评估实际部署成本。
- **应用限制**：仅验证了GUI任务，未讨论在其他领域（如机器人、对话系统）的通用性。
- **潜在偏差风险**：记忆系统的演化可能依赖初始种子经验，若初始经验偏差，可能导致收敛到次优策略。
- **未见错误分析**：未讨论失败案例或DMS在何种情况下可能失效。

（完）
