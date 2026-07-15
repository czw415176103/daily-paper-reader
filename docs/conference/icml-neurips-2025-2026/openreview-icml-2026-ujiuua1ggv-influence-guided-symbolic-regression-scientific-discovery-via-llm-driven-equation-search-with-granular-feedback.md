---
title: "Influence-Guided Symbolic Regression: Scientific Discovery via LLM-Driven Equation Search with Granular Feedback"
title_zh: 影响引导的符号回归：通过细粒度反馈实现LLM驱动的方程搜索
authors: "Evgeny Saveliev, Samuel Holt, Nabeel Seedat, David L. Bentley, Jim Weatherall, Mihaela van der Schaar"
date: 2026-04-30
pdf: "https://openreview.net/pdf/954ce510d1a5902db51e49c098151be23811c8df.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 大语言模型驱动的符号回归，具有细粒度反馈
tldr: 大语言模型用于符号回归时，现有方法仅依赖全局MSE反馈，搜索效率低。本文提出IGSR，将方程发现建模为迭代过程：LLM生成候选基函数，然后基于影响（influence）的细粒度反馈评估每个项对误差的贡献，指导下一轮生成。在多个科学数据集上，IGSR比基线方法更快发现准确且简洁的方程。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: LLM用于符号回归时，粗粒度的全局MSE反馈难以识别方程中有效或错误的部分，搜索效率低。
method: IGSR迭代使用LLM生成候选基函数，并利用基于影响（influence）的细粒度反馈评估每个项，引导下一轮搜索。
result: 在多个科学数据集上，IGSR比基线方法更快发现准确且简洁的方程。
conclusion: 细粒度反馈能显著提升LLM在符号回归中的搜索效率和方程质量。
---

## Abstract
Large Language Models (LLMs) offer a promising avenue for scientific discovery, yet their application to symbolic regression is often constrained by inefficient search strategies and coarse feedback signals. Current methods typically guide LLMs using scalar metrics (e.g., global Mean Squared Error), which fail to identify which components of a proposed equation are driving performance or causing error. We introduce \textit{Influence-Guided Symbolic Regression} (IGSR), a method that frames equation discovery as an iterative two-step process combining diverse term generation with rigorous selection: an LLM generates candidate basis functions $\psi_j(\mathbf{x})$ for a linear model, which are then evaluated using granular influence scores $\Delta_j$. These scores quantify each term's marginal contribution to generalization accuracy, enabling an influence-guided pruning process that systematically refines the model structure. Integrating this mechanism into a Monte Carlo Tree Search (MCTS) enables navigating the combinatorial search space while balancing exploration of novel functional forms with exploitation of high-influence components. We demonstrate IGSR's effectiveness on a diverse suite of benchmarks, including LLM-SRBench, pharmacological PKPD models, an epidemiological simulation, and real-world genomic data. Notably, we validate the framework's capacity for genuine discovery in a case study using a high-dimensional biological dataset, in which IGSR identified a novel relationship between DNA methylation and RNA Polymerase II pausing; a hypothesis that was subsequently supported via wet-lab experimentation.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前将大语言模型（LLM）应用于符号回归时，搜索策略效率低下，且反馈信号粗粒度（如仅依赖全局均方误差MSE）。这种粗粒度反馈无法揭示方程中哪些项有效、哪些项导致误差，从而难以指导LLM迭代改进。
- **研究背景**：符号回归旨在从数据中自动发现简洁数学表达式，是科学发现的重要工具。LLM的生成能力为此提供了新途径，但缺乏对生成项的有效评估和搜索引导。
- **整体含义**：本文提出**影响引导的符号回归（IGSR）**，通过细粒度的“影响分数”量化每个项对模型泛化精度的边际贡献，从而实现对LLM生成过程的精准反馈，显著提升搜索效率和方程质量。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：将方程发现建模为迭代两步骤过程：**生成**（LLM产生候选基函数）→ **评估与剪枝**（基于影响力的细粒度反馈筛选有效项），并整合到蒙特卡洛树搜索（MCTS）中平衡探索与利用。
- **关键技术细节**：
  - **候选基函数生成**：LLM根据当前模型结构生成一组基函数 \(\psi_j(\mathbf{x})\)（如多项式、三角函数等）。
  - **影响分数计算**：定义影响分数 \(\Delta_j\)，量化第 \(j\) 个项对模型泛化精度的**边际贡献**（例如通过移除该项后误差变化量）。\(\Delta_j\) 越大，说明该项越重要。
  - **剪枝与模型更新**：基于影响分数排序，保留高影响项，剔除低影响或负影响项，形成新的候选模型。
  - **MCTS集成**：将生成-评估-剪枝循环作为树搜索节点，MCTS负责在组合空间中探索新函数形式（探索）与利用已知高影响成分（利用）。
- **算法流程（文字描述）**：
  1. 初始模型为空或简单基函数。
  2. 当前模型输入LLM，LLM生成一组候选基函数。
  3. 利用数据拟合线性组合 \(\hat{y} = \sum w_j \psi_j(\mathbf{x})\)。
  4. 计算每个 \(\psi_j\) 的 \(\Delta_j\)（如交叉验证或基于影响函数）。
  5. 保留 \(\Delta_j\) 大于阈值的项，移除其余项，更新模型。
  6. 重复步骤2-5直到收敛或达到迭代上限，同时MCTS记录状态转移以指导下一步采样。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **数据集与场景**：
  - LLM-SRBench（标准符号回归基准）
  - 药代动力学/药效学（PKPD）模型
  - 流行病学模拟数据
  - 真实世界基因组数据（DNA甲基化与RNA聚合酶II暂停关系）
- **Benchmark**：主要与现有LLM驱动符号回归方法进行对比（如基于全局MSE反馈的基线），评价指标包括方程准确度（如 \(R^2\)、相对误差）、简洁性（项数）、发现速度（迭代次数或时间）。
- **对比方法**：文中未详细列出具体名称，但应包含至少一种未使用细粒度反馈的LLM符号回归基线，以及可能的传统符号回归方法（如Eureqa、PySR等）。

## 4. 资源与算力

- **文中未明确说明**：未提及所使用的GPU型号、数量、训练时长或总计算预算。仅能从ICML会议级别推测使用了常见研究级GPU（如A100或V100），但具体细节缺失。

## 5. 实验数量与充分性

- **实验组数**：涵盖四个不同领域的数据集（标准基准、药理、流行病学、基因组），并有独立案例研究（wet-lab验证），实验数量中等。
- **消融实验**：摘要未明确提及消融研究（如是否验证不同反馈策略、MCTS必要性），可能论文正文中包含。
- **充分性与公平性**：基于摘要信息，实验覆盖了多样化的科学场景，且基因组案例通过湿实验验证，增加了说服力。但未展示与其他顶尖SR方法的充分对比（如是否在同一数据集上使用相同计算预算），公开信息有限。

## 6. 论文的主要结论与发现

- IGSR比基线方法**更快**发现准确且简洁的方程。
- 细粒度反馈（影响分数）能显著提升LLM在符号回归中的搜索效率。
- 在真实高维生物学数据中，IGSR成功发现DNA甲基化与RNA聚合酶II暂态暂停之间的**新颖关系**，并经湿实验支持，展示了实际科学发现能力。

## 7. 优点：方法或实验设计上的亮点

- **细粒度反馈机制**：提出影响分数 \(\Delta_j\)，超越了全局MSE的粗粒度评估，使LLM能精准修正无效项、保留有效项。
- **与MCTS结合**：在组合搜索空间中平衡探索与利用，避免陷入局部最优。
- **可解释性**：生成的方程为线性组合形式，每个基函数的贡献可量化，易于领域专家理解。
- **真实发现验证**：通过湿实验验证新假设，展示了该方法从数据驱动到科学发现的全链条能力。

## 8. 不足与局限

- **计算资源未公开**：无法评估方法的实际成本与可复现性。
- **线性模型假设**：IGSR假设目标函数可表示为基函数的线性组合，可能限制非线性表达能力（如乘法或嵌套项）。
- **依赖LLM生成质量**：基函数库的质量高度受限于LLM的预训练知识，在未知领域可能生成作用有限的函数形式。
- **实验比较不够透明**：未详细列出对比方法的配置、超参数和统计显著性检验，可能存在偏差风险。
- **适用范围**：主要面向已知符号形式相对简单的科学定律，对于高度复杂或噪声极大的数据可能失效。

（完）
