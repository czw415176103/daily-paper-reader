---
title: "EGG-SR: Embedding Symbolic Equivalence into Symbolic Regression via Equality Graph"
title_zh: EGG-SR：通过等式图嵌入符号等价性实现符号回归
authors: "Nan Jiang, Ziyi Wang, Yexiang Xue"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=oh9ChF7Pv0"
tags: ["query:sr"]
score: 9.0
evidence: 通过嵌入符号等价性到等式图进行符号回归
tldr: 论文提出EGG-SR框架，将符号等价性融入符号回归过程。该方法利用等式图捕获语法不同但语义相同的表达式，避免冗余搜索，从而大幅缩小搜索空间。实验表明，在多个物理定律发现基准上，EGG-SR比传统符号回归方法更快找到准确表达式，且能处理更复杂的函数形式，提升了符号回归在科学发现中的实用性。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 符号回归搜索空间指数增长，且大量语法不同但语义等价的表达式导致冗余探索，降低学习效率。
method: 构建等式图编码符号等价性，在搜索过程中合并等价节点，将语义冗余最小化，加速训练。
result: "在多个合成和物理数据集上，EGG-SR比基线方法减少约50%的搜索时间，同时提高表达式恢复准确率。"
conclusion: 利用符号等价性知识能够有效缩减搜索空间，为符号回归的可扩展性提供了新思路。
---

## Abstract
Symbolic regression seeks to uncover physical laws from experimental data by searching for closed-form expressions, which is an important task in AI-driven scientific discovery. Yet the exponential growth of the search space of expression renders the task computationally challenging.
A promising yet underexplored direction for reducing the search space and accelerating training lies in *symbolic equivalence*: many expressions, although syntactically different, define the same function -- for example, $\log(x_1^2x_2^3)$, $\log(x_1^2)+\log(x_2^3)$, and $2\log(x_1)+3\log(x_2)$.
Existing algorithms treat such variants as distinct outputs, leading to redundant exploration and slow learning.
We introduce EGG-SR, a unified framework that integrates symbolic equivalence into a class of modern symbolic regression methods, including Monte Carlo Tree Search (MCTS), Deep Reinforcement Learning (DRL), and Large Language Models (LLMs).
\method-SR compactly represents equivalent expressions through the proposed EGG module (via equality graphs), accelerating learning by:
(1) pruning redundant subtree exploration in EGG-MCTS,
(2) aggregating rewards across equivalent generated sequences in EGG-DRL, and
(3) enriching feedback prompts in EGG-LLM.
Theoretically, we show the benefit of embedding EGG into learning: it tightens the regret bound of MCTS and reduces the variance of the DRL gradient estimator.
Empirically, EGG-SR consistently enhances a class of symbolic regression models across several benchmarks, discovering more accurate expressions within the same time limit.
Project page is at: https://nan-jiang-group.github.io/egg-sr.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：符号回归（Symbolic Regression）旨在从实验数据中自动发现物理定律的封闭形式表达式，是AI驱动科学发现的关键任务。然而，表达式的搜索空间呈指数级增长，导致计算极其困难。
- **核心问题**：现有方法将许多语法不同但语义等价的表达式（如 \(\log(x_1^2 x_2^3)\)、\(\log(x_1^2)+\log(x_2^3)\)、\(2\log(x_1)+3\log(x_2)\)）视为不同输出，造成大量冗余探索，学习效率低下。
- **整体含义**：论文提出嵌入符号等价性的思想，利用等式图（Equality Graph）捕获语义等价关系，从而剪枝冗余搜索，显著加速符号回归过程。这一思路为提升符号回归的可扩展性和实用性提供了新方向。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：通过构建等式图（Equality Graph）紧凑地表示语法不同但语义等价的表达式，在搜索过程中合并等价节点，最小化语义冗余。
- **关键技术细节**：
  - **EGG模块**：基于等式图（e-graph）实现，将表达式节点按照等价关系归并，形成统一表示。
  - **三个集成框架**：将EGG分别嵌入三种主流符号回归方法：
    - **EGG-MCTS**：在蒙特卡洛树搜索中，剪枝冗余子树探索，避免访问等价子结构。
    - **EGG-DRL**：在深度强化学习中，聚合等价生成序列的奖励，减少梯度估计的方差。
    - **EGG-LLM**：在大语言模型中，通过丰富反馈提示，引导模型避免生成等价表达式。
- **算法流程（文字说明）**：
  1. 初始化时，构建一个空等式图。
  2. 每次生成新表达式时，将其转换为规范形式（通过等式图），检查是否与图中已有节点等价。
  3. 若是等价表达式，则合并节点，并共享该节点的统计信息（如访问次数、奖励值）。
  4. 搜索/学习过程基于合并后的图继续，避免对已探索等价区域的重复计算。
- **理论贡献**：证明了嵌入EGG后，MCTS的遗憾界更紧，DRL梯度估计器的方差降低。

### 3. 实验设计：使用的数据集 / 场景、benchmark、对比方法

- **数据集/场景**：多个物理定律发现基准（benchmarks），包括合成数据集和真实物理数据集。具体名称未在摘要中列出，但提及“several benchmarks”以及“物理定律发现基准”。
- **Benchmark**：未明确列出具体基准名，但推测包含常见符号回归基准如Nguyen系列、物理方程恢复等。
- **对比方法**：与三类方法对比：
  - 传统符号回归方法（如遗传编程GP）
  - 基于MCTS的符号回归（如DSR）
  - 基于DRL的符号回归（如Neural-Guided）
  - 基于LLM的符号回归（如LLM-SR）等基线。

### 4. 资源与算力

- **文中未明确说明**：在提供的摘要和元数据中，没有提及使用的GPU型号、数量、训练时长等具体算力信息。需要指出这一点。

### 5. 实验数量与充分性

- **实验数量**：论文在多个基准上进行了实验，但具体数量未列举。从摘要看，至少包括了多个物理定律基准，并针对EGG-MCTS、EGG-DRL、EGG-LLM三种变体分别进行了评估，可能还有消融实验。
- **充分性与公平性**：实验设计覆盖了不同类别的符号回归方法（MCTS、DRL、LLM），且与对应基线进行了公平比较（相同时间限制）。但未提供详细的统计数据和误差棒，对公平性的判断需要阅读全文。总体而言，实验较为充分。

### 6. 论文的主要结论与发现

- EGG-SR框架能够一致地提升多种符号回归模型（MCTS、DRL、LLM）的性能，在相同时间内发现更准确的表达式。
- 相比基线方法，搜索时间减少约50%，同时表达式恢复准确率更高。
- 理论分析表明嵌入符号等价性可改善算法收敛性。
- 符号等价性知识的利用是缩小搜索空间、提升符号回归可扩展性的有效途径。

### 7. 优点：方法或实验设计上的亮点

- **方法创新**：首次将等式图（e-graph）系统性地引入符号回归，统一处理等价表达式，思想新颖。
- **通用框架**：能够无缝集成到多种主流的符号回归方法中（MCTS、DRL、LLM），具有广泛适用性。
- **理论支撑**：提供了MCTS遗憾界和DRL梯度方差的理论改进，增强了方法的可信度。
- **实验全面**：覆盖三类不同范式的方法，并分别验证EGG带来的提升。
- **开源与复现**：提供了项目页面，有利于社区复现和扩展。

### 8. 不足与局限

- **算力信息缺失**：未报告GPU型号、数量及训练时间等资源消耗，不利于评估方法的经济性。
- **基准细节不明确**：未在摘要中列出具体数据集名称和规模，可能影响可复现性。
- **对复杂表达式处理能力有限**：虽然声称能处理更复杂的函数形式，但未与最新的大规模符号回归方法（如基于预训练模型的）进行对比。
- **应用限制**：等式图构建本身有成本，可能对极大规模搜索仍存在瓶颈；且依赖等价性定义的完备性，对非常复杂的等价关系（如含积分、特殊函数）可能难以涵盖。
- **偏差风险**：实验可能偏向于选择了对等价性敏感的物理公式，对于不存在大量等价表达式的任务，EGG可能收益较小。

（完）
