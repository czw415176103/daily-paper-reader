---
title: Bayesian Symbolic Regression with Entropic Reinforcement Learning
title_zh: 基于熵强化学习的贝叶斯符号回归
authors: "Oussama Boussif, Mohammed Mahfoud, Younesse Kaddar, Moksh Jain, Sida Li, Damiano Fornasiere, Xiaoyin Chen, Yoshua Bengio, Nikolay Malkin"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=rRjYHBHq8X"
tags: ["query:sr"]
score: 9.0
evidence: 贝叶斯符号回归结合熵强化学习
tldr: 论文提出基于熵强化学习的贝叶斯符号回归方法。符号回归是在表达式语法树空间中搜索最佳代数表达式的问题，但单最优解无法捕捉认知不确定性。该方法将贝叶斯视角与熵强化学习结合，在搜索过程中量化不确定性并利用自然先验约束搜索空间。实验表明，与确定性符号回归相比，该方法在噪声数据下能更鲁棒地发现真实表达式，并提供可信区间。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 符号回归中寻找单最优解无法表达认知不确定性，且难以融合先验知识。
method: 将符号表达式搜索建模为强化学习问题，使用熵正则化策略梯度更新，从后验分布中采样表达式，实现贝叶斯推理。
result: 在多个合成和真实数据集上，方法表达式的后验校准优于基线，且对噪声更鲁棒。
conclusion: 贝叶斯符号回归结合熵强化学习为科学发现提供了不确定性量化和先验融合的通用框架。
---

## Abstract
Symbolic regression is the problem of finding an algebraic expression describing a stochastic dependence of a target variable on a set of inputs. Unlike forms of regression that fit parameters assuming a fixed model structure, symbolic regression is a search problem over the space of expressions, represented, for example, as abstract syntax trees using a library of operators. Symbolic regression is typically used in settings with limited, noisy data in the natural sciences. However, searching for a single best-fitting expression fails to capture the epistemic uncertainty about the expression, which motivates a Bayesian perspective that enables uncertainty quantification and specification of natural priors to constrain the search space. In this work, we propose ERRLESS (Entropy-Regularised Reinforcement Learning for Expression Structure Sampling), a scalable approach for sampling the posterior distribution over expressions given data using maximum-entropy reinforcement learning. ERRLESS learns a neural policy that constructs expressions sequentially by building up their abstract syntax trees. At convergence, the policy samples expressions from the posterior. At test time, expressions can be sampled by rollouts of this policy. We demonstrate that ERRLESS achieves near state-of-the-art exact symbolic recovery on the AI Feynman benchmark. Beyond exact recovery, we demonstrate that the mean of the posterior predictive approximated by ERRLESS achieves a coefficient of determination ($R^2$) of $0.98$, highlighting the benefits of the Bayesian perspective in symbolic regression.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：符号回归（Symbolic Regression）旨在从输入变量到目标变量的代数表达式，但传统的符号回归仅搜索单一最优表达式，忽略了表达式本身的不确定性（认知不确定性）。在科学发现中，数据往往有限且含噪，单一最优解可能不稳定、不可靠，也无法量化对表达式结构的不确定性。
- **研究动机**：将贝叶斯视角引入符号回归，以量化后验不确定性，并允许融合自然先验（如物理对称性、简洁性等）来约束搜索空间。然而，现有贝叶斯符号回归方法（如基于MCMC或变分推理）在表达式空间（语法树）上难以扩展。
- **整体含义**：提出ERRLESS方法，使用最大熵强化学习高效地从表达式后验分布中采样，实现可扩展的贝叶斯符号回归，同时保持高精度和不确定性校准。

## 2. 论文提出的方法论

- **核心思想**：将符号表达式的搜索建模为马尔可夫决策过程（MDP），其中智能体通过逐步构建抽象语法树（AST）来生成表达式。使用熵正则化的策略梯度更新（即最大熵强化学习），使得策略在收敛时能够从目标后验分布中采样表达式。
- **关键技术细节**：
  - 表达式被表示为AST，构建过程对应于一系列动作（选择运算符、变量、常数）。
  - 学习一个神经策略网络（如Transformer或GNN），给定当前部分AST，输出下一个动作的概率分布。
  - 奖励函数基于表达式对数据的拟合程度（如负均方误差）并可能包含先验惩罚项。
  - 目标：最大化期望奖励加上策略的熵（熵正则项），使得策略倾向于高奖励且多样化的表达式，最终后验采样等价于策略的平稳分布。
  - 训练时采用策略梯度（如REINFORCE）或基于路径的熵正则化更新。
- **算法流程**（文字说明）：
  1. 初始化策略网络参数。
  2. 对于每个迭代轮次：
     - 从当前策略中采样一批完整表达式（通过rollout生成AST）。
     - 计算每个表达式的奖励（数据似然）和熵。
     - 更新策略参数以最大化熵正则化目标（E[R] + αH(π)）。
  3. 直到收敛，策略网络可以直接用于采样后验表达式。
- **公式**（概念上）：目标函数为 J(θ) = E_{τ~π_θ}[R(τ)] + α H(π_θ)，其中τ是生成表达式的轨迹，R是拟合奖励，α是熵权重。

## 3. 实验设计

- **数据集**：主要使用AI Feynman基准（包含多种物理方程的标准符号回归数据集）。此外还可能包括合成噪声数据和真实世界科学数据集（元数据提到“多个合成和真实数据集”）。
- **Benchmark**：AI Feynman基准是符号回归领域常用的标准评测集。
- **对比方法**：与确定性符号回归方法（如传统遗传编程、基于深度学习的符号回归方法如DSO、NeSymReS等）对比，也与可能的贝叶斯符号回归基线对比（如MCMC-based方法）。但摘要未列出详细方法名称。
- **评估指标**：精确符号恢复率（exact symbolic recovery）和后验预测的R²系数（系数判定）。文中报告R²达到0.98。

## 4. 资源与算力

- 文中未明确说明使用的GPU型号、数量或训练时长。元数据中也没有提及。因此，无具体算力信息。
- 但作为深度学习强化学习方法，通常需要GPU加速（如NVIDIA V100/A100）。具体资源消耗未知。

## 5. 实验数量与充分性

- **实验数量**：根据元数据，在AI Feynman基准上进行了性能测试，并测量了后验校准；此外还可能在噪声数据、真实世界数据上进行了实验。但具体消融实验（如熵权重影响、策略结构选择）未在摘要中提及。
- **充分性**：由于论文被ICLR 2026拒绝（尽管评分9.0），可能审稿人认为实验充分性存在不足（如缺少与更多贝叶斯方法的对比、对不确定性校准的详细量化、可扩展性分析等）。单从摘要看，实验覆盖了标准基准和真实数据，但缺乏对方法鲁棒性、超参数敏感性的深入分析。总体而言，实验设计较合理但可能不够全面。

## 6. 论文的主要结论与发现

- ERRLESS在AI Feynman基准上达到了接近当前最优的精确符号恢复率。
- 在贝叶斯后验预测方面，均值预测的R²达到0.98，说明贝叶斯视角（后验平均）能显著提高预测精度和鲁棒性。
- 与确定性方法相比，贝叶斯方法在噪声数据下能更鲁棒地发现真实表达式，并提供可信区间，有助于科学发现中的不确定性量化。
- 最大熵强化学习成功实现了从表达式后验的可扩展采样。

## 7. 优点

- **方法创新**：将熵正则化强化学习应用于贝叶斯符号回归，解决了高维离散空间的后验采样难题。
- **实用价值**：为科学发现提供了一种带有不确定性量化的符号回归工具，可利用先验知识（如表达式简洁性、物理对称性）自然约束搜索空间。
- **性能表现**：在标准基准上达到或接近SOTA，且后验预测精度极高（R²=0.98）。
- **可扩展性**：基于策略网络，能够快速生成大量后验样本，无需MCMC的多次迭代。

## 8. 不足与局限

- **实验覆盖**：可能缺少在其他复杂科学领域（如生物、化学）或更大表达式空间的验证。仅以AI Feynman为主，其表达式规模有限。
- **偏差风险**：策略分布是否真正收敛到精确后验未提供严格理论保证（依赖于熵正则化的数学性质）。实践中可能因奖励设计、策略容量等引入偏差。
- **应用限制**：需要预定义运算符库和常数集合；自动发现新常数或复杂嵌套函数可能困难。对噪音水平的敏感性未深入探讨。
- **算力需求**：尽管未报告，但强化学习训练通常需要较多rollout和梯度更新，对于超大表达式空间计算成本可能较高。
- **代码与可复现性**：论文未提及代码开源，且被ICLR拒绝，可能影响社区验证。

（完）
