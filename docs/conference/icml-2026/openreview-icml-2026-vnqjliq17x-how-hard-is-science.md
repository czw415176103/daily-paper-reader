---
title: How Hard Is Science?
title_zh: 科学有多难？
authors: "Adil Soubki, Miles Cranmer"
date: 2026-04-30
pdf: "https://openreview.net/pdf/56f6caa041c942450ef5c3b8a03d30b78e3740c4.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 研究符号回归的计算复杂度
tldr: "该论文针对符号回归（SR）NP难的理论与实际成功之间的差距，采用参数化复杂度理论重新分析SR的可处理性。研究表明，当以表达式深度或树大小为参数时，SR是固定参数可处理的（FPT），这形式化了流行SR算法中有限复杂度搜索所利用的可处理区域。然而，当参数化其他方面时，SR变为W[1]-难。该工作为理解SR的实践可解性提供了理论依据。"
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 符号回归在实践中有效但理论上NP难，需要解释这一矛盾。
method: 使用参数化复杂度理论分析符号回归的可处理性，考虑表达式深度、树大小等参数。
result: "证明了在表达式深度或树大小参数下SR是FPT，在其它参数下是W[1]-难。"
conclusion: 形式化了符号回归实际可解的理论基础，指导算法设计。
---

## Abstract
Symbolic Regression (SR) is the task of finding a closed-form mathematical expression that optimizes some objective. Solving this task is NP-hard. However, SR software routinely discovers accurate, interpretable models without exhaustively searching function space. 
Motivated by this disconnect between worst-case theory and practical success, we study SR through the lens of \emph{parameterized complexity theory}. In particular, we reanalyze tractability with respect to practically relevant parameters like expression depth, tree size, and number of primitives used. We show that SR is fixed-parameter tractable (FPT) when parameterized by expression depth or tree size, formalizing the tractable regime exploited by bounded-complexity search in popular SR algorithms. However, SR becomes W[1]-hard when parameterized by the number of variables or primitives used, identifying selection as a source of intractability. We further find lower bounds under the exponential time hypothesis, prove approximation hardness, and rule out polynomial kernels for the case where the primitive set is part of the input.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：符号回归（SR）的目标是从数据中自动发现封闭形式的数学表达式，这是一个理论上被证明为NP难的问题（最坏情况下计算复杂度极高）。然而，实际中的SR软件（如Eureqa、GPlearn等）却能高效地发现准确且可解释的模型，无需穷举搜索函数空间。这种“理论与实践的矛盾”促使作者重新审视SR的可处理性。
- **背景**：传统复杂度分析基于最坏情况，无法解释实践中SR为何有效。作者引入**参数化复杂度理论**，通过识别实际中影响难度的关键参数，试图刻画“可处理区域”。

## 2. 论文提出的方法论
- **核心思想**：将问题实例中的某些量（如表达式深度、树大小、原始操作符数量等）作为**参数**，研究当这些参数固定时，问题的计算复杂度是否会从NP难降为固定参数可处理（FPT）。
- **关键技术细节**：
  - 定义符号回归的决策问题版本（是否存在表达式使得拟合误差小于阈值）。
  - 选取四个候选参数：表达式深度（depth）、表达式树大小（tree size）、变量个数（number of variables）、原始操作符集合大小（number of primitives）。
  - 分别分析在这些参数化下的复杂度归属：
    - **表达式深度** 和 **树大小** 作为参数时，SR是**FPT**（固定参数可处理），即存在算法复杂度为 \(O(f(k) \cdot n^c)\)，其中k是参数，n是数据规模，f是仅依赖于k的函数。
    - **变量个数** 或 **原始操作符集合大小** 作为参数时，SR变为 **W[1]-难**，表明选择变量或操作符是导致难解性的来源。
  - 进一步给出基于指数时间假设（ETH）的下界，证明近似难解性，并证明当原始操作符集合作为输入一部分时，不存在多项式核（即问题无法被高效压缩）。
- **算法流程**（文字说明）：作者并未提出新算法，而是通过归约证明（如从团问题归约到SR）来建立复杂度结果。

## 3. 实验设计
- **数据集/场景**：本文是**纯理论分析**，未进行任何实际实验或数值模拟。因此没有使用数据集、基准（benchmark）或对比方法。
- **Benchmark与对比**：不适用。论文依靠计算复杂性理论的证明（归约、FPT算法构造等）。

## 4. 资源与算力
- 文章中未提及任何GPU、计算集群或训练时长。作为理论论文，不依赖算力。需要指出：**未明确说明，且不涉及实验资源**。

## 5. 实验数量与充分性
- 本文**没有实验**，全部为数学证明。因此不存在“实验数量”或“充分性”问题。就理论分析而言，作者完成了四个参数化方向的复杂度归类，并给出了ETH下界和近似难解性结果，证明完整且严谨。但需注意：缺乏实际验证（例如与真实SR算法运行时间的对照），这是理论论文的常见特点。

## 6. 论文的主要结论与发现
- **正向结果**：当**表达式深度**或**树大小**作为参数时，符号回归是**FPT**，这形式化了为何有限复杂度搜索（如限制深度或树大小）在实践中有效。
- **负向结果**：当**变量个数**或**原始操作符集合大小**作为参数时，符号回归是**W[1]-难**，表明变量选择或操作符选择是复杂度的主要瓶颈。
- **其他结论**：在ETH假设下，不存在时间复杂度为 \(f(k)n^{o(k)}\) 的算法（其中k是树大小）；近似SR具有难解性；若原始操作符集作为输入一部分，则问题没有多项式核。

## 7. 优点
- **理论贡献清晰**：首次将参数化复杂度分析系统应用于符号回归，弥合了NP难理论与实践性能之间的鸿沟。
- **参数选择合理**：表达式深度、树大小、变量数、操作符数都是实际SR算法中常用的调节参数，具有现实意义。
- **结果平衡**：既证明了可处理区域（FPT），也指出了不可处理来源（W[1]-难），为算法设计提供了指导（例如应避免对变量或操作符进行穷举选择，而应聚焦于控制深度或树大小）。

## 8. 不足与局限
- **缺乏实证验证**：论文未使用任何真实SR算法（如GP、模拟退火、遗传编程）来验证理论结果是否与实用复杂度一致。理论结论虽然严谨，但尚未在数据集上得到支持。
- **理论假设简化**：分析基于特定的决策问题定义，实际SR往往涉及连续优化、噪声、多目标等复杂因素，这些未被捕获。
- **应用限制**：结果主要警告了变量选择和操作符选择的难解性，但并未给出具体可实施的近似或启发式策略，对实践者的直接指导有限。
- **实验覆盖**：无实验，因此无法评估方法在不同数据规模、不同噪声水平下的表现。属于纯理论论文，适用范围受限。

（完）
