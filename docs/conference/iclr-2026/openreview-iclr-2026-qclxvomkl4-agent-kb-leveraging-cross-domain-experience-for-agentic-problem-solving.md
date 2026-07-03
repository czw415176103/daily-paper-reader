---
title: "Agent KB: Leveraging Cross-Domain Experience for Agentic Problem Solving"
title_zh: 智能体知识库：利用跨领域经验进行智能体问题求解
authors: "Xiangru Tang, Tianrui Qin, Daniel Shao, Ziyang Zhou, Jiapeng Chen, Tianhao Peng, Tingting Du, Peng Xia, Fang Wu, He Zhu, Jiaheng Liu, Xingyao Wang, Sirui Hong, Chenglin Wu, Hao Cheng, Chi Wang, Wangchunshu Zhou"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=QCLXVOMkl4"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 通用记忆基础设施实现跨架构智能体知识转移和技能共享
tldr: 针对AI智能体框架孤立运行、经验无法共享的问题，本文提出Agent KB，一种通用记忆基础设施。它通过聚合轨迹构建结构化知识库并提供轻量API，实现异构框架间的经验共享，无需重新训练。该方法提升了智能体跨域问题求解能力。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有AI智能体框架相互孤立，导致知识无法共享，重复犯错。
method: 构建通用记忆基础设施，聚合轨迹为结构化知识库，提供API实现跨框架共享。
result: 无需重新训练即可在不同Agent框架间共享经验，提升问题求解效率。
conclusion: Agent KB实现了跨架构的集体智能，为智能体技能进化提供基础架构。
---

## Abstract
AI agent frameworks operate in isolation, forcing agents to rediscover solutions and repeat mistakes across different systems. Despite valuable problem-solving experiences accumulated by frameworks like smolagents, OpenHands, and OWL, this knowledge remains trapped within individual systems, preventing the emergence of collective intelligence. Current memory systems focus on individual agents or framework-specific demonstrations, failing to enable cross-architecture knowledge transfer. We introduce AGENT KB, a universal memory infrastructure enabling seamless experience sharing across heterogeneous agent frameworks without retraining. AGENT KB aggregates trajectories into a structured knowledge base and serves lightweight APIs. At inference time, hybrid retrieval operates through two stages: planning seeds agents with cross-domain workflows, while feedback applies targeted diagnostic fixes. A disagreement gate ensures retrieved knowledge enhances rather than disrupts reasoning, addressing knowledge interference in cross-framework transfer. We validate AGENT KB across major frameworks on GAIA, Humanity’s Last Exam, GPQA, and SWE-bench. Results show substantial improvements across diverse model families: compared to baseline pass@1, smolagents with AGENT KB achieve up to 18.7pp gains at pass@3 (55.2% → 73.9%), while OpenHands improves 4.0pp on SWE-bench pass@1 (24.3% → 28.3%). Similar improvements are observed across all base model families. Ablations confirm that hybrid retrieval and feedback stages are essential, with automatically generated experiences matching manual curation. This establishes the foundation for collective agent intelligence through shared memory infrastructures.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：当前 AI 智能体框架（如 smolagents、OpenHands、OWL 等）相互孤立运行，导致智能体在不同系统间重复犯错、重复发现解决方案，宝贵的经验知识无法跨架构共享，缺乏集体智能的涌现基础。
- **整体含义**：论文旨在打破这种孤立状态，通过构建通用的记忆基础设施（Agent KB），实现异构智能体框架之间的经验共享与知识转移，从而提升智能体在多样化任务上的问题求解能力。

## 2. 论文提出的方法论
- **核心思想**：设计一个**通用记忆基础设施**（Agent KB），将不同框架的轨迹（trajectories）聚合并结构化，形成知识库，并通过轻量 API 提供服务，使任何智能体框架无需重新训练即可调用共享经验。
- **关键技术细节**：
  - **知识库构建**：聚合来自不同框架的智能体执行轨迹，将其转化为结构化的经验条目（如问题描述、解决方案步骤、成败反馈等）。
  - **混合检索（Hybrid Retrieval）**：在推理时分为两个阶段：
    - **规划阶段（Planning Stage）**：利用检索到的跨域工作流（workflows）来启发智能体的初始规划。
    - **反馈阶段（Feedback Stage）**：针对当前执行中的具体诊断问题，检索并应用目标性的修复（diagnostic fixes）。
  - **分歧门控（Disagreement Gate）**：一个判别机制，确保检索到的知识能够增强而非干扰智能体的原有推理过程，从而解决跨框架知识迁移中的干扰问题（knowledge interference）。
- **算法流程（文字说明）**：
  1. 离线阶段：收集多个智能体框架的运行轨迹，清洗、结构化后存入 Agent KB 知识库。
  2. 在线推理阶段：给定新任务，先通过规划阶段检索相关跨域工作流，生成初始计划；在执行过程中，若遇到具体错误或异常，进入反馈阶段检索对应诊断修复；最终结果经过分歧门控过滤，仅采纳与当前推理兼容的知识。

## 3. 实验设计
- **使用的数据集/场景**：GAIA、Humanity’s Last Exam、GPQA、SWE-bench 四个基准测试。
  - GAIA：通用 AI 助手评估集。
  - Humanity’s Last Exam：高难度综合题目。
  - GPQA：研究生级问答。
  - SWE-bench：软件工程任务（如代码修复）。
- **Benchmark**：以上四个标准 benchmark，覆盖通用问答、知识推理、代码修复等场景。
- **对比方法**：
  - 基线方法：各智能体框架自身的原始表现（无 Agent KB）。
  - 主要对比框架：smolagents、OpenHands（以及可能隐含的 OWL 等），并涵盖多种基座模型家族（如不同规模的 LLM）。

## 4. 资源与算力
- 论文摘要及元数据**未明确说明**使用的 GPU 型号、数量、训练时长等具体算力信息。文中只提到“无需重新训练”（retraining-free），但其知识库构建可能需要离线处理轨迹数据，算力需求相对较低。实验部分也未提及训练或推理硬件配置，因此无法量化。

## 5. 实验数量与充分性
- **实验数量**：主要报告了 smolagents 和 OpenHands 在多个 benchmark 上的结果，并进行了消融实验（ablation on hybrid retrieval and feedback stages）。
  - 具体结果：smolagents 在 GAIA（?）上 pass@3 提升 18.7pp（55.2% → 73.9%）；OpenHands 在 SWE-bench 上 pass@1 提升 4.0pp（24.3% → 28.3%）。其他 benchmark 也有类似改进。
  - 消融实验证实：混合检索和反馈阶段都是必要的；自动生成的经验（自动整理）与手工整理的效果相当。
- **充分性与公平性**：
  - 覆盖了多个主流基准和两种典型框架（开源 smolagents 和 OpenHands），并验证了不同基座模型家族，具有一定代表性。
  - 但实验仅展示了绝对准确率提升，未详细报告不同框架间的方差、统计显著性，也未与已有的记忆增强方法（如单个智能体记忆系统）进行直接对比。因此公平性尚有提升空间。

## 6. 论文的主要结论与发现
- **Agent KB 能有效实现跨架构经验共享**：无需重新训练即可让不同智能体框架受益于其他框架积累的知识。
- **显著提升问题求解性能**：在多个 benchmark 上均获得稳定提升，尤其对弱基线（如 smolagents）提升幅度大。
- **混合检索与分歧门控机制关键**：规划阶段和反馈阶段各自贡献，门控机制防止负面迁移。
- **自动整理经验可媲美手工整理**：降低了构建高质量知识库的成本。

## 7. 优点：方法或实验设计上的亮点
- **通用性**：方法不依赖特定框架或模型，可插拔式集成，具有实际部署潜力。
- **免训练特性**：相较于需要重新微调的方案，Agent KB 更轻量、易扩展。
- **分层检索设计**：规划+反馈两阶段，兼顾宏观规划与微观修正，逻辑合理。
- **分歧门控**：创新性地解决跨框架知识干扰问题，是技术亮点。
- **自动经验整理达到手工水平**：证明数据驱动构建的可行性。

## 8. 不足与局限
- **实验覆盖不全面**：仅测试了 smolagents 和 OpenHands 两种框架，未验证对其他主流框架（如 LangChain、AutoGPT 等）的兼容性。
- **缺少与同类记忆系统对比**：未与现有的框架内记忆方法（如 MemGPT 的长期记忆、LangChain 的 memory 模块）进行横向比较。
- **算力/资源开销未报告**：虽然声称免训练，但构建知识库需要多框架轨迹数据收集，实际成本未量化。
- **可能存在的偏差风险**：基准任务多为公开评测集，可能过拟合；真实复杂场景下的泛化能力未知。
- **知识库规模与更新机制未深入讨论**：随着新框架加入，知识库的维护和老化问题未涉及。
- **仅报告 pass@1、pass@3 等简单指标**，缺乏成功率、鲁棒性（如对抗扰动）等多维度评估。

（完）
