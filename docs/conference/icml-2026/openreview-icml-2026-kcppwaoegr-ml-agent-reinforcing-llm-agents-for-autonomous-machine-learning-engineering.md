---
title: "ML-Agent: Reinforcing LLM Agents for Autonomous Machine Learning Engineering"
title_zh: ML-Agent：通过强化学习增强LLM智能体实现自主机器学习工程
authors: "Zexi Liu, Jingyi Chai, Xinyu Zhu, Shuo Tang, Rui Ye, Weiyu Ma, Bo Zhang, LEI BAI, Siheng Chen"
date: 2026-04-30
pdf: "https://openreview.net/pdf/aad5c8df9dbe56e96769be57e2b17c6f7444eca5.pdf"
tags: ["query:cfd-agent"]
score: 7.0
evidence: 使用强化学习训练LLM智能体进行机器学习工程
tldr: 针对prompt-based LLM智能体在ML工程中的泛化不足和高开销问题，首次提出基于学习的智能体ML范式，使用在线强化学习训练LLM智能体在交互式ML实验中学习，包含探索增强微调等关键组件，显著提升自主ML工程能力。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 基于提示的LLM智能体泛化能力受限且计算开销大。
method: 采用在线强化学习框架，包含探索增强微调等组件训练LLM智能体。
result: 训练后的智能体在ML实验任务上表现更优。
conclusion: 强化学习可有效提升LLM智能体的自主ML工程能力。
---

## Abstract
The emergence of large language model (LLM)-based agents has significantly advanced the development of autonomous machine learning (ML) engineering. However, the dominant prompt-based paradigm exhibits limitations: smaller models lack the capacity to learn from execution trajectories for generalization, while large proprietary models incur high computational overhead, restricting accessibility and scalability. Focusing on this, for the first time, we explore the paradigm of learning-based agentic ML, where an LLM agent learns through interactive experimentation on ML tasks using online reinforcement learning (RL). To realize this, we propose a novel agentic ML training framework with three key components: (1) exploration-enriched fine-tuning, which enables LLM agents to generate diverse actions for enhanced RL exploration; (2) step-wise RL, which enables training on a single action step, accelerating experience collection and improving training efficiency; (3) an agentic ML-specific reward module, which unifies varied ML feedback signals into consistent rewards for RL optimization. Leveraging this framework, we train ML-Agent, driven by a 7B-sized Qwen-2.5 LLM for autonomous ML. Despite training on only 9 ML tasks, our 7B-sized ML-Agent achieves comparable performance to agents using much larger proprietary LLMs (e.g., GPT-5) but at significantly lower computational cost, demonstrating strong performance and cross-task generalization.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
当前基于大型语言模型（LLM）的智能体在自主机器学习工程（ML Engineering）中取得了显著进展。然而，主流的基于提示（prompt-based）的范式存在两大局限：
- **小模型泛化能力不足**：小规模LLM无法从执行轨迹中有效学习以实现跨任务泛化；
- **大模型计算开销高昂**：大型专有模型（如GPT-5）虽然性能强，但推理成本高，限制了可及性与可扩展性。
为此，论文首次探索**基于学习的智能体ML范式**（learning-based agentic ML），即利用在线强化学习（RL）让LLM智能体在交互式ML实验任务中学习，从而在保持较小模型规模的同时获得接近大型模型的表现，降低计算成本并提升泛化能力。

## 2. 论文提出的方法论：核心思想、关键技术细节
**核心思想**：使用在线强化学习框架训练一个规模适中的LLM（7B参数）作为智能体，使其在ML任务中自主执行实验（如编写代码、运行脚本、分析结果）并从中学习改进策略。

**关键技术细节**：框架包含三个关键组件：
1. **探索增强微调（Exploration-Enriched Fine-tuning）**  
   在RL训练前，先对基础LLM进行微调，使其在动作空间中生成更多样化的动作，从而增强RL阶段的探索能力。
2. **逐步RL（Step-wise RL）**  
   不同于传统RL在整条轨迹后更新策略，该方法允许对单步动作进行训练，加速经验收集，提高训练效率。
3. **智能体ML特定奖励模块（Agentic ML-specific Reward Module）**  
   将ML任务中各种反馈信号（如代码执行结果、模型性能指标等）统一为一致的奖励值，供RL优化使用。

**算法流程（文字说明）**：
- 初始化：使用一个预训练LLM（Qwen-2.5-7B）作为基础。
- 探索增强微调：在少量ML任务数据上对LLM进行监督微调，使其动作分布更宽泛。
- 在线RL训练：智能体在ML实验环境中（每个任务包含编写代码、运行、观察结果）不断尝试，收集动作-奖励数据；使用逐步RL（类似PPO的变体）更新策略。
- 奖励计算：根据实验结果（如测试准确率、训练时间等）通过奖励模块计算标量奖励。
- 重复上述过程直至收敛。

**公式**：论文未在摘要中提供具体公式，但框架可理解为最大化带折扣的累积奖励，奖励函数由自定义模块给出。

## 3. 实验设计：数据集/场景、Benchmark、对比方法
- **训练任务**：仅在 **9个ML任务**上训练ML-Agent（7B大小的Qwen-2.5-LLM）。任务类型未详细说明，推测包括分类、回归、超参数调优等典型ML实验。
- **评估场景**：与使用大型专有LLM（如GPT-5）的agent进行对比，评估其在未见任务上的跨任务泛化能力。
- **对比方法**：主要对比基于提示的LLM agent（如使用GPT-5、GPT-4等）。未提及其他基于学习的agent方法（因为本文是首个）。
- **Benchmark**：未明确命名，但通过比较性能（如准确率、效率等）来证明同等表现下计算成本更低。

## 4. 资源与算力
文中明确提到：ML-Agent基于**7B参数的Qwen-2.5 LLM**，训练仅用了**9个ML任务**。但**未说明具体的GPU型号、数量、训练时长等算力信息**。仅从对比中可知其计算成本显著低于GPT-5等大型模型。

## 5. 实验数量与充分性
- 实验数量：训练集仅9个任务，测试集未明确规模，但强调“跨任务泛化”能力。模型规模（7B）远小于GPT-5，但性能相当。
- 充分性与公平性：由于摘要篇幅有限，未提及消融实验、超参数分析、统计显著性等细节。仅从声称来看，实验设计不够透明：训练任务数量少（9个），可能不足以证明强泛化；对比方法仅为prompt-based大型模型，缺少与相同规模下其他训练范式的对比。**存在评估不充分的风险**。

## 6. 论文的主要结论与发现
- **核心发现**：使用在线强化学习训练的小规模LLM（7B）可以在自主ML工程任务上达到与GPT-5等大型专有LLM agent相当的性能，而计算成本显著降低。
- **有效性验证**：即使训练任务数量少（9个），模型仍能表现出良好的跨任务泛化能力。
- **框架优势**：提出的三个组件（探索增强微调、逐步RL、特定奖励模块）共同作用，使得基于学习的agent范式在实践中可行且高效。

## 7. 优点
- **首次提出基于学习的agentic ML范式**，跳出prompt-based框架，为小模型赋能提供了新思路。
- **模块化设计**：三个组件各自解决一个关键问题（探索、效率、奖励），具有可借鉴性。
- **低成本高性能**：只用7B参数量即达到GPT-5级别效果，对资源受限机构友好。
- **在线RL的应用**：让agent在真实实验环境中迭代学习，符合自动化ML工程的实际需求。

## 8. 不足与局限
- **实验规模小**：仅9个训练任务，测试泛化能力证据不足，可能存在于特定分布过拟合的风险。
- **缺乏详细消融**：未展示每个组件的贡献度，以及逐步RL与完整轨迹RL的对比。
- **对比基线不完整**：未与同样规模的基于学习的agent（如使用其他RL方法或模仿学习）对比，也未与监督微调版本的agent对比。
- **资源信息缺失**：未报告训练成本（GPU时数），无法量化性价比。
- **领域局限**：任务局限于ML工程中的代码编写和实验执行，未涉及更广泛的AI工程任务。
- **奖励设计主观性**：奖励模块将多种信号统一为标量，可能丢失细节信息，且设计是否最优有待验证。

（完）
