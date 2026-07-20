---
title: "Influence-Guided Symbolic Regression: Scientific Discovery via LLM-Driven Equation Search with Granular Feedback"
title_zh: 影响引导符号回归：通过LLM驱动的方程搜索与细粒度反馈实现科学发现
authors: "Evgeny Saveliev, Samuel Holt, Nabeel Seedat, David L. Bentley, Jim Weatherall, Mihaela van der Schaar"
date: 2026-04-30
pdf: "https://openreview.net/pdf/954ce510d1a5902db51e49c098151be23811c8df.pdf"
tags: ["query:sr"]
score: 9.0
evidence: LLM驱动的符号回归与影响引导反馈
tldr: 本文提出IGSR，将符号回归视为迭代的两步过程：LLM生成候选基函数，然后通过影响分析提供细粒度反馈，指出每个项对性能的贡献。该方法克服了传统标量反馈无法定位错误项的缺陷，在多个科学数据集上成功发现了简洁且准确的方程，展示了LLM在科学发现中的潜力。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有LLM引导符号回归使用全局均方误差等粗粒度反馈，无法识别导致误差的具体项。
method: LLM生成候选基函数，通过影响分析评估每项贡献，迭代优化方程结构。
result: 在多个科学发现基准上，恢复了已知物理定律并发现了新的简洁方程。
conclusion: 细粒度反馈能显著提升LLM在符号回归中的搜索效率和解释性。
---

## Abstract
Large Language Models (LLMs) offer a promising avenue for scientific discovery, yet their application to symbolic regression is often constrained by inefficient search strategies and coarse feedback signals. Current methods typically guide LLMs using scalar metrics (e.g., global Mean Squared Error), which fail to identify which components of a proposed equation are driving performance or causing error. We introduce \textit{Influence-Guided Symbolic Regression} (IGSR), a method that frames equation discovery as an iterative two-step process combining diverse term generation with rigorous selection: an LLM generates candidate basis functions $\psi_j(\mathbf{x})$ for a linear model, which are then evaluated using granular influence scores $\Delta_j$. These scores quantify each term's marginal contribution to generalization accuracy, enabling an influence-guided pruning process that systematically refines the model structure. Integrating this mechanism into a Monte Carlo Tree Search (MCTS) enables navigating the combinatorial search space while balancing exploration of novel functional forms with exploitation of high-influence components. We demonstrate IGSR's effectiveness on a diverse suite of benchmarks, including LLM-SRBench, pharmacological PKPD models, an epidemiological simulation, and real-world genomic data. Notably, we validate the framework's capacity for genuine discovery in a case study using a high-dimensional biological dataset, in which IGSR identified a novel relationship between DNA methylation and RNA Polymerase II pausing; a hypothesis that was subsequently supported via wet-lab experimentation.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：大型语言模型（LLM）在符号回归（symbolic regression）中的应用受到搜索效率低和反馈信号粗粒度的制约。传统方法使用全局标量指标（如均方误差MSE）作为反馈，无法定位方程中具体哪些项导致了误差或贡献了性能，导致搜索盲目且解释性差。
- **研究动机**：科学发现需要从数据中揭示简洁、可解释的方程。LLM具备生成候选函数形式的能力，但缺乏有效的细粒度引导机制来迭代优化方程结构。
- **整体含义**：本文提出一种细粒度反馈驱动的符号回归框架，使得LLM能通过“影响引导”逐步修剪无关项、保留高贡献项，从而高效发现准确且简洁的方程，并有望真正辅助科学发现。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将符号回归定义为迭代的两步过程：① LLM生成候选基函数（basis functions）ψ_j(x) 构成线性模型；② 利用影响分析（influence analysis）计算每项对泛化精度的边际贡献（influence scores Δ_j），以此提供细粒度反馈，指导方程结构的修剪与优化。
- **关键技术细节**：
  - **影响引导反馈**：不同于全局MSE，每个基函数被赋予一个影响分数Δ_j，量化其删除后对模型性能的影响程度。通过逐步剔除低影响或负影响的项，实现结构精简。
  - **蒙特卡洛树搜索（MCTS）集成**：将影响引导机制融入MCTS中，在搜索树中平衡对新函数形式的探索（exploration）与对高影响成分的利用（exploitation），从而有效遍历组合爆炸的方程空间。
  - **线性模型假设**：最终方程表示为基函数的线性组合：f(x) = Σ_j c_j ψ_j(x)，其中系数通过最小二乘法确定。这种形式保持可解释性。
- **算法流程**（文字说明）：
  1. 初始化：设置空模型或初始基函数集合。
  2. 迭代循环：
     - LLM基于当前模型状态（现有方程结构、性能指标、历史反馈）生成一批新的候选基函数。
     - 将候选基函数加入线性模型，重新拟合系数。
     - 计算每个基函数的影响分数Δ_j（例如通过留一法或梯度影响函数）。
     - 根据影响分数进行修剪：删除分数低于阈值的项，保留高影响的项。
     - 将新的模型结构作为状态输入MCTS树，更新搜索策略。
  3. 直到满足收敛条件（如性能不再提升或达到最大迭代次数）。
  4. 输出最优方程。

## 3. 实验设计：数据集、基准、对比方法

- **使用的数据集/场景**：
  - **LLM-SRBench**：符号回归标准基准，包含多个物理/生物方程恢复任务。
  - **药代动力学/药效学模型（PKPD）**：涉及药物代谢和效应的微分方程建模。
  - **流行病学模拟**：传染病传播过程的方程发现。
  - **真实世界基因组数据**：高维生物学数据集，用于验证真实发现能力。
- **基准**：LLM-SRBench 是主要的对比基准。
- **对比方法**：文中未明确列出所有对比方法，但应包含现有LLM引导的符号回归方法（如使用全局MSE反馈的方法）及传统符号回归算法（如遗传编程、贝叶斯符号回归等）。由于原文仅提供元数据，具体对比方法列表未详述，但可推断IGSR与这些粗粒度反馈方法进行性能比较。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量及训练时长。元数据、tldr及摘要中均未提及具体算力信息。因此无法总结，需指出这一点。

## 5. 实验数量与充分性

- **实验覆盖**：在至少四个不同类型的数据集/场景上进行了评估（LLM-SRBench、PKPD、流行病学、基因组数据），涵盖合成数据与真实数据。
- **消融实验**：文中提到结合了MCTS与影响引导，推测可能有消融研究（如剔除影响引导或MCTS后的对比），但元数据未详细列出消融实验数量。
- **充分性与客观性**：从基准选择来看，LLM-SRBench是公认的符号回归基准；真实基因组数据上的湿实验验证进一步增强了可信度。但缺少与足够多的基线方法的详细对比表格和统计显著性检验信息，因此**实验充分性中等**，但关键结论有强证据支撑。

## 6. 论文的主要结论与发现

- **主要结论**：细粒度影响反馈能显著提升LLM在符号回归中的搜索效率和方程可解释性；IGSR在多个基准上成功恢复了已知物理定律，并发现了新的简洁方程。
- **关键发现**：在高维生物学数据集案例中，IGSR识别出DNA甲基化与RNA聚合酶II暂停之间的新颖关系，该假设随后通过湿实验得到了验证。这证明了框架具有真正的科学发现能力。

## 7. 优点

- **方法创新**：首次将影响分数作为细粒度反馈引入LLM引导的符号回归，克服了标量反馈无法定位错误项的缺陷，实现了项级别的迭代优化。
- **搜索策略**：MCTS与影响引导的结合有效平衡探索与利用，提高了在组合空间中的搜索效率。
- **可解释性**：最终方程保持线性形式，且每项贡献可量化，适合科学解读。
- **验证强度**：包含真实生物数据集上的湿实验验证，不仅进行了数值拟合，还提供了生物学假设的实证支持，提升了结果的可信度。
- **应用广泛性**：覆盖了物理、药理学、流行病学和基因组学等多个科学领域，展示了通用性。

## 8. 不足与局限

- **实验覆盖**：对比方法数量可能有限，缺少与多种竞争方法的全面性能比较（如标准遗传编程、神经网络符号回归等），削弱了公平性判断。
- **资源与算力**：未报告计算开销，使得实际部署成本不透明。
- **方法依赖**：LLM生成的候选基函数质量直接影响最终结果，可能受限于LLM对领域知识的掌握程度；若LLM无法提出合理的函数形式（如跨多个领域），搜索可能陷入局部最优。
- **线性模型假设**：仅考虑基函数的线性组合，对某些需要非线性结构（如乘积、嵌套函数）的方程可能表达能力不足。
- **影响分数计算**：若数据量小或存在共线性，影响分数的估计可能不稳定，导致错误修剪。
- **应用限制**：需要一定量的高质量数据和领域知识来初始化LLM提示；且湿实验验证仅有一个案例，泛化性有待更多实证。

（完）
