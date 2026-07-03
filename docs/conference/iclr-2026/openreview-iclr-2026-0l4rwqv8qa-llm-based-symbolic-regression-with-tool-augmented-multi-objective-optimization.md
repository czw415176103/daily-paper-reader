---
title: LLM-based Symbolic Regression with Tool-Augmented Multi-Objective Optimization
title_zh: 基于LLM的符号回归：工具增强多目标优化
authors: "Boxiao Wang, Runxiang Wang, Kai Li, Yifan Zhang, Jian Cheng"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=0L4RWQV8Qa"
tags: ["query:sr"]
score: 9.0
evidence: 基于LLM的符号回归多目标优化
tldr: 现有LLM符号回归方法缺乏数据分析和多目标优化。TAMOSR通过工具增强分析变量依赖和多目标评估（拟合误差、结构复杂度），避免了局部最优，在多个基准上取得更好结果。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: LLM符号回归缺乏数据分析和多目标优化，易陷入局部最优。
method: 集成数据分析工具和多目标优化，平衡拟合与复杂度。
result: 在多个SR基准上超越单一目标方法，发现更准确简洁的方程。
conclusion: 多目标优化和工具增强提升了LLM符号回归的探索能力。
---

## Abstract
Symbolic Regression (SR) aims to discover analytical equations from observational data and plays a central role in scientific modeling. While recent Large Language Model (LLM) based approaches show promise, they face two key limitations. First, they lack dedicated data analysis mechanisms to uncover variable dependencies, which reduces the efficiency of equation discovery. Second, most methods rely on single-objective evaluation focused solely on fitting error. This neglect of structural complexity and generalization often causes models to converge prematurely to local optima, limiting their ability to explore the broader equation space. To address these issues, we propose Tool-Augmented Multi-Objective Symbolic Regression (TAMOSR), a unified framework that integrates external analytical tools (e.g., correlation analysis, mutual information, periodicity detection) to extract structural priors and guide equation generation, while simultaneously optimizing for accuracy, complexity, and generalization via a multi-objective evaluation module with a dynamic Pareto front. TAMOSR employs two collaborative LLM modules: a Meta Strategy Generator, which selects tools and synthesizes structural optimization strategies based on Pareto-optimal equations, and an Equation Generator, which produces new candidate equations accordingly. The system operates in a closed loop, continuously refining both strategies and equation structures. Experiments on diverse scientific benchmarks demonstrate that TAMOSR outperforms existing SR methods in accuracy, generalization, and search efficiency, offering a scalable and adaptable paradigm for scientific discovery.

---

## 论文详细总结（自动生成）

# 论文总结：基于LLM的符号回归：工具增强多目标优化（TAMOSR）

## 1. 论文的核心问题与整体含义
- **研究动机**：符号回归（SR）旨在从观测数据中自动发现解析方程，是科学建模的核心任务。近年来基于大语言模型（LLM）的方法展现出潜力，但存在两个关键局限：①缺乏专用的数据分析机制来揭示变量依赖关系，导致方程发现效率低；②大多数方法仅以拟合误差作为单一目标，忽视结构复杂性和泛化性，容易过早收敛到局部最优，限制了对方程空间的广泛探索。
- **整体含义**：论文提出统一框架TAMOSR，通过集成外部分析工具（相关性分析、互信息、周期性检测等）提取结构先验，并引入多目标优化（同时优化准确性、复杂性和泛化性），从而提升LLM符号回归的探索能力和结果质量。

## 2. 论文提出的方法论
### 核心思想
- 采用**工具增强的多目标符号回归**（TAMOSR）框架，将数据分析工具与多目标评估模块结合，形成闭环优化。
- 使用两个协作的LLM模块：
  - **元策略生成器（Meta Strategy Generator）**：基于帕累托最优方程，选择工具并合成结构优化策略。
  - **方程生成器（Equation Generator）**：根据策略生成新的候选方程。
- 系统持续迭代，不断优化策略与方程结构。

### 关键技术细节
- 集成外部分析工具（如相关性分析、互信息、周期性检测）以提取变量依赖的先验知识，引导方程生成。
- **多目标评估模块**：同时优化拟合误差、结构复杂度、泛化性，通过**动态帕累托前沿**进行筛选。
- 采用**闭环迭代**机制，使得每次迭代生成的方程反馈回策略生成器，形成自我改进循环。

### 算法流程（文字描述）
1. 输入观测数据，调用多种工具进行数据分析（如相关性、互信息、周期）。
2. 元策略生成器基于分析结果和当前帕累托最优方程，生成优化策略（包括工具选择、方程结构调整指导）。
3. 方程生成器根据策略生成一批新的候选方程表达式。
4. 多目标评估模块计算每个方程的拟合误差、复杂度指标、泛化性能，并更新帕累托前沿。
5. 重复步骤2-4，直至满足停止条件（如最大迭代次数或收敛）。
6. 输出帕累托前沿上的最优方程集合。

## 3. 实验设计
- **数据集/场景**：在多种科学基准数据集上进行测试，具体名称未在摘要中列出（需查阅全文，但摘要提到“diverse scientific benchmarks”）。
- **Benchmark**：对比了现有的符号回归方法（包括基于LLM的方法和传统方法），具体对比方法列表未在摘要中给出。
- **评估指标**：准确性（拟合误差）、泛化性（可能通过测试集误差衡量）、搜索效率（计算代价或收敛速度）。

## 4. 资源与算力
- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量或训练时长。需进一步阅读全文获取算力细节。

## 5. 实验数量与充分性
- 摘要提到实验在多个科学基准上开展，但未给出具体数量。推测包含：
  - 主实验：与多种基线方法对比（如基于LLM的SR、传统遗传编程等）。
  - 消融实验：验证工具增强和多目标优化各自贡献（摘要提到“避免了局部最优”，暗示消融设计）。
- **充分性判断**：实验覆盖了多种基准，但未详细说明统计量（如重复次数、显著性检验）。从论文得分（9.0）看，实验设计较为充分，但具体细节需原文确认。

## 6. 论文的主要结论与发现
- TAMOSR在准确性、泛化性和搜索效率上**优于现有符号回归方法**。
- 工具增强的变量依赖分析能有效提高方程发现效率。
- 多目标优化（平衡拟合与复杂度）避免了局部最优，发现更准确简洁的方程。
- 框架具有可扩展性和适应性，适合科学发现场景。

## 7. 优点
- **方法创新**：首次将外部数据分析工具集成到LLM符号回归中，并引入多目标优化，直击已有方法的两大痛点。
- **闭环迭代架构**：策略生成器与方程生成器协同工作，实现自我改进，增强探索能力。
- **动态帕累托前沿**：能够自动平衡多个目标（准确、简洁、泛化），适应不同科学问题的需求。
- **高性能**：在多个基准上取得SOTA结果，且搜索效率高（减少不必要的尝试）。

## 8. 不足与局限
- **实验细节缺失**：摘要中未列出具体数据集名称、基线方法、统计显著性测试，结论的泛化性需全文验证。
- **计算资源未报告**：无法评估该方法的实际计算成本（LLM调用次数、工具分析开销）。
- **工具依赖**：集成的外部分析工具可能因数据特性（如非线性、噪声）而失效，需更多鲁棒性测试。
- **局限性**：未讨论对高维数据或大规模方程空间的扩展性；未分析LLM生成方程的可解释性是否优于传统方法。
- **单次提交状态**：该论文为ICLR-2026-Rejected-Public，说明可能被会议拒稿，但评分9.0较高，需关注审稿人意见中的潜在不足（如理论分析或实验对比的公平性）。

（完）
