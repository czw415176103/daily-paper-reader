---
title: How Hard Is Science?
title_zh: 科学有多难？
authors: "Adil Soubki, Miles Cranmer"
date: 2026-04-30
pdf: "https://openreview.net/pdf/56f6caa041c942450ef5c3b8a03d30b78e3740c4.pdf"
tags: ["query:sr"]
score: 7.0
evidence: 符号回归的参数化复杂度分析
tldr: "符号回归虽然理论上NP难，但实际算法往往高效。本文采用参数化复杂度理论，揭示当参数化为表达式深度或树大小时，SR是固定参数可解的（FPT），为受限搜索的有效性提供了严格理论依据；同时发现参数化为使用原语数量时变为W[1]-难。该分析有助于指导SR算法设计，并解释其为何在实践中可行。"
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 符号回归的实际成功率与理论NP难之间存在差距，需理论解释。
method: 采用参数化复杂度理论分析符号回归在不同参数下的可解性。
result: "证明深度和树大小参数下SR是FPT，而某些组合参数下是W[1]-难。"
conclusion: 该理论为符号回归算法设计提供了指导，解释了实践中的高效性。
---

## Abstract
Symbolic Regression (SR) is the task of finding a closed-form mathematical expression that optimizes some objective. Solving this task is NP-hard. However, SR software routinely discovers accurate, interpretable models without exhaustively searching function space. 
Motivated by this disconnect between worst-case theory and practical success, we study SR through the lens of \emph{parameterized complexity theory}. In particular, we reanalyze tractability with respect to practically relevant parameters like expression depth, tree size, and number of primitives used. We show that SR is fixed-parameter tractable (FPT) when parameterized by expression depth or tree size, formalizing the tractable regime exploited by bounded-complexity search in popular SR algorithms. However, SR becomes W[1]-hard when parameterized by the number of variables or primitives used, identifying selection as a source of intractability. We further find lower bounds under the exponential time hypothesis, prove approximation hardness, and rule out polynomial kernels for the case where the primitive set is part of the input.

---

## 论文详细总结（自动生成）

# 论文总结：《科学有多难？——符号回归的参数化复杂度分析》

## 1. 核心问题与整体含义

- **研究动机**：符号回归（Symbolic Regression, SR）在理论上被证明是NP难问题，但实际算法（如遗传编程、Gaussian process-based 方法等）却常常能够高效地发现准确、可解释的表达式。这种“最坏情况理论”与“实践成功”之间的鸿沟缺乏理论解释。
- **核心问题**：为什么符号回归在实践中可行？哪些因素决定了其计算复杂性的可处理性？
- **整体含义**：通过参数化复杂度理论，揭示符号回归在受限搜索（如限制表达式深度、树大小）下是固定参数可解的（FPT），从而为现有算法的有效性提供严格理论依据，并指明哪些参数会导致本质困难（如使用原语数量、变量个数）。

## 2. 方法论

- **核心思想**：采用参数化复杂度理论（parameterized complexity theory）重新分析符号回归的可处理性。将问题实例中的关键特征（如表达式深度、树大小、使用原语数量）作为参数，而非仅依赖输入规模。
- **关键技术细节**：
  - 定义符号回归问题为：寻找一个封闭形式的数学表达式，优化某个目标函数（例如最小化与数据的误差）。
  - 参数化：将表达式深度（depth）、树大小（tree size）、使用的原语数量（number of primitives used）等作为参数。
  - 固定参数可解（FPT）：若参数固定，算法能在多项式时间内解决问题，且指数部分仅依赖于参数。
  - W[1]-困难：另一种复杂度类别，表明随着参数增长，问题可能仍难以高效求解（类似于NP难）。
- **主要结果**：
  - 当参数化为表达式深度或树大小时，符号回归是FPT。
  - 当参数化为使用的变量或原语数量时，符号回归变为W[1]-困难。
  - 进一步给出基于指数时间假说（ETH）的下界，证明近似难度，并排除了在原始集作为输入情况下的多项式核。
- **公式/算法流程**：摘要未给出具体算法细节，但隐含了参数化归约与复杂度分类的技术（如使用参数化归约证明W[1]-hardness）。

## 3. 实验设计

- **说明**：本文为纯理论论文，未涉及实验。摘要及元数据中未提及任何数据集、基准或对比方法。因此无实验部分可总结。

## 4. 资源与算力

- **说明**：本文为理论分析，不涉及计算实验。文中未提及GPU、训练时长等算力资源。这一点已明确指出。

## 5. 实验数量与充分性

- **说明**：不适用。本文没有进行实验，故无法评价实验数量与充分性。所有结论均基于数学推导与复杂度理论论证。

## 6. 主要结论与发现

- **正面结果**：符号回归在参数化为表达式深度或树大小的情况下是固定参数可解的（FPT）。这直接解释了为何限制复杂度的搜索策略（如许多流行算法中限制表达式深度）在实践中有效。
- **负面结果**：当参数化为使用原语数量或变量个数时，符号回归是W[1]-困难，表明“选择哪些原语/变量”是本质困难来源。
- **进一步结论**：在指数时间假说下无法实现子指数算法；近似困难；若原语集作为输入的一部分，则不存在多项式核（即难以压缩问题规模）。
- **整体意义**：为符号回归算法设计提供了理论指导：应优先控制表达式深度/树大小，而对原语选择需谨慎处理。

## 7. 优点

- **理论创新**：首次将参数化复杂度理论系统应用于符号回归，填补了“NP难”与“实践可行”之间的理论空白。
- **问题精准**：明确了哪些参数决定了可处理性，区分了“可解”与“难解”的边界。
- **结果严谨**：不仅给出FPT结果，还给出了W[1]-困难、ETH下界、近似困难与无多项式核等全方位分析，理论深度高。
- **实践指导性**：直接解释了为什么现有算法限制表达式深度或树大小能成功，并警告了原语选择可能带来的计算爆炸。

## 8. 不足与局限

- **缺乏实证验证**：纯理论工作，未在实际SR算法上验证参数化预测（如测量不同参数下的运行时间），理论与实践的对照仍需实验支撑。
- **参数选择有限**：论文考虑的参数（深度、树大小、原语数量、变量数）虽然关键，但可能遗漏其他实际重要参数（如表达式结构复杂度、目标函数平滑性等）。
- **假设条件**：复杂度分析基于标准计算模型，未考虑诸如随机搜索、启发式、并行化等实际优化手段，这些可能改变实际可解性。
- **内容完整性**：由于仅有摘要提供，无法评估论文是否提供了算法构造细节或完整证明，但理论上应包含严格证明。
- **应用限制**：W[1]-难度结果虽指出了困难，但未提供应对策略（如参数化近似算法或启发式设计建议）。

（完）
