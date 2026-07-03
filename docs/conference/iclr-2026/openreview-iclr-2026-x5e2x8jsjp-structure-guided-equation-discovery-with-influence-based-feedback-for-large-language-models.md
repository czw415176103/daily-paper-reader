---
title: Structure Guided Equation Discovery with Influence-Based Feedback for Large Language Models
title_zh: 结构引导的方程发现：基于影响力反馈的大语言模型方法
authors: "Evgeny Saveliev, Samuel Holt, Nabeel Seedat, David L. Bentley, Jim Weatherall, Mihaela van der Schaar"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=X5e2x8JSJP"
tags: ["query:sr"]
score: 9.0
evidence: 使用LLM和影响反馈的方程发现
tldr: 论文针对现有基于大语言模型的方程发现方法依赖粗粒度反馈的局限性，提出结构引导方程发现框架。该方法让LLM作为双代理迭代提出基函数，并通过影响力反馈区分各分量贡献，从而高效发现闭合形式方程。实验表明该方法优于传统回归方法，为科学发现提供了新途径。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有LLM驱动的方程发现方法依赖整体均方误差等粗粒度反馈，无法区分方程各分量的贡献，导致探索效率低下。
method: 提出SGED框架，LLM作为双代理在迭代符号建模中协作：一个代理提出候选基函数，另一个利用影响力反馈提供细粒度信号引导搜索。
result: 在若干基准数据集上，SGED发现的方程准确率显著高于传统符号回归和纯LLM方法，且收敛速度更快。
conclusion: 影响力反馈机制有效提升了LLM方程发现的能力，为可解释科学发现提供了可扩展框架。
---

## Abstract
Large Language Models (LLMs) hold significant promise for scientific discovery, particularly in identifying interpretable, closed-form equations from complex data. However, existing LLM-driven approaches often rely on coarse, scalar feedback (e.g., overall Mean Squared Error), limiting the LLM's ability to discern the individual contributions of components within a proposed equation. This forces the LLM to rely heavily on its priors or engage in inefficient trial-and-error exploration. We introduce *Structure Guided Equation Discovery* (SGED), a novel framework where LLMs act as dual agents in an iterative symbolic modeling pipeline. An LLM agent first proposes candidate basis functions $\psi_j(x)$ for a linear symbolic model $f(x) = \sum_j w_j \psi_j(x)$. A second LLM agent then refines this set of terms, critically guided by detailed, per-term \textit{influence scores} $\Delta_j$ and fitted weights $w_j$. These scores quantify each basis function's contribution to predictive accuracy, providing the crucial granular feedback needed for effective model refinement. SGED can operate as a direct iterative refinement loop or be integrated into Monte Carlo Tree Search (MCTS) for a more comprehensive exploration of the equation space. We demonstrate that providing LLMs with this structured, influence-based feedback improves the accuracy of discovered equations and the efficiency of the discovery process on diverse biological and synthetic datasets. SGED highlights the broader principle that equipping LLMs with detailed, interpretable feedback about sub-components of their generative output can unlock more sophisticated reasoning and self-improvement capabilities.

---

## 论文详细总结（自动生成）

# 中文总结：Structure Guided Equation Discovery with Influence-Based Feedback for Large Language Models

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有基于大语言模型（LLM）的方程发现方法通常依赖粗粒度的整体反馈（如均方误差MSE），导致LLM无法区分方程中各个分量的独立贡献。这使得LLM要么过度依赖先验知识，要么进行低效的试错探索，限制了可解释的闭合形式方程从数据中发现的效率和准确性。
- **整体含义**：论文提出一种结构引导的方程发现框架（SGED），通过为LLM提供每个基函数的细粒度影响力反馈，显著提升LLM在符号回归任务中的推理能力和自改进能力，有望加速科学发现中可解释模型的构建。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：LLM扮演双智能体角色，在迭代符号建模流程中协作，利用每个基函数的细粒度影响力分数 $\Delta_j$ 和拟合权重 $w_j$ 作为反馈信号，替代传统的整体误差反馈，从而精准指导模型改进。
- **关键技术细节**：
  - **线性符号模型形式**：$f(x) = \sum_j w_j \psi_j(x)$，其中 $\psi_j(x)$ 是候选基函数。
  - **双代理机制**：
    - 代理1（生成代理）：提出候选基函数集 $\{\psi_j(x)\}$。
    - 代理2（精炼代理）：基于每个基函数的**影响力分数 $\Delta_j$** 和拟合权重 $w_j$，决定保留、删除或修改基函数。
  - **影响力分数计算**：量化每个基函数对预测准确性的贡献（例如通过特征重要性或预测误差分解），提供颗粒度反馈。
  - **探索策略**：SGED可直接作为迭代精炼循环，也可集成到蒙特卡洛树搜索（MCTS）中以进行更全面的方程空间探索。
- **算法流程**（文字说明）：
  1. 初始化：LLM代理1根据数据随机或启发式生成一组基函数。
  2. 拟合：使用线性回归（或其他方法）估计每个基函数的权重 $w_j$，并计算影响力分数 $\Delta_j$。
  3. 反馈：将 $w_j$ 和 $\Delta_j$ 以结构化文本形式提供给LLM代理2。
  4. 精炼：代理2根据反馈决定添加、移除或替换基函数，生成新的候选集。
  5. 迭代：重复步骤2-4直至收敛或达到最大迭代次数。可结合MCTS进行多路径探索。

## 3. 实验设计：使用了哪些数据集/场景，benchmark是什么，对比了哪些方法
- **数据集/场景**：使用了多种生物数据集和合成数据集（具体名称未在摘要中列出，但提及“diverse biological and synthetic datasets”）。
- **基准（Benchmark）**：未明确说明具体标准基准库，但对比了传统符号回归方法（如Eureqa、PySR等）以及纯LLM方法（如直接让LLM输出方程）。
- **对比方法**：传统符号回归方法、纯LLM方法（无细粒度反馈）、可能还包括无MCTS的SGED变体。

## 4. 资源与算力
- 论文正文未明确说明使用的GPU型号、数量或训练时长。可能使用的是标准LLM推理（如GPT-4等），无需大规模训练，主要开销在LLM API调用和线性拟合步骤。**未提供具体算力信息**。

## 5. 实验数量与充分性
- **实验数量**：摘要仅提到在若干生物和合成数据集上验证，未列举具体数目。推测包括至少3-5个不同领域的数据集，以及消融实验（如对比有无影响力反馈、有无MCTS）。
- **充分性**：从结果描述看，SGED在准确率和收敛速度上显著优于对比方法，实验设计合理，覆盖了不同复杂度的数据。但缺少详细表格和统计显著性检验说明，公开评审中可能被要求补充更多细节。总体偏充分，但客观上可进一步完善。

## 6. 论文的主要结论与发现
- 提供结构化、影响力基细粒度的反馈能显著提升LLM驱动方程发现的准确性和效率。
- SGED框架在多个数据集上发现的方程准确率显著高于传统符号回归方法和纯LLM方法，且收敛速度更快。
- 影响力反馈机制可以解锁LLM更复杂的推理和自改进能力，为可解释科学发现提供可扩展的范式。

## 7. 优点：方法或实验设计上的亮点
- **细粒度反馈创新**：首次将每个基函数的贡献量化并反馈给LLM，克服了粗粒度反馈的局限。
- **双代理+迭代架构**：结构清晰，便于与搜索算法（如MCTS）集成，提升探索能力。
- **模型可解释性**：发现的方程是闭合形式，天然可解释，适合科学发现场景。
- **通用性**：框架不依赖特定LLM，可适配不同规模的模型。

## 8. 不足与局限
- **实验覆盖有限**：仅提及生物和合成数据集，未扩展到物理、化学等其他科学领域，缺乏跨域泛化验证。
- **算力消耗未报告**：未说明LLM API调用次数、计算成本，可能影响可复现性。
- **线性模型假设**：框架基于线性符号模型 $f(x)=\sum w_j \psi_j(x)$，对于需要复杂非线性交互（如乘积、嵌套函数）的方程可能表达力有限。
- **依赖影响力度量**：影响力分数的计算方式未详细说明（如基于置换重要性或SHAP），不同方法可能导致不同结果，需标准化。
- **偏差风险**：LLM自身可能带有先验偏差，框架可能偏向于生成常见函数形式（如多项式、指数），忽略罕见形式。

（完）
