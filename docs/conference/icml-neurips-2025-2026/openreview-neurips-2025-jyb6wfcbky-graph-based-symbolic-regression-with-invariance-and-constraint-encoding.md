---
title: Graph-based Symbolic Regression with Invariance and Constraint Encoding
title_zh: 基于图的符号回归：不变性与约束编码
authors: "Ziyu Xiang, Kenna Ashen, Xiaofeng Qian, Xiaoning Qian"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=JYB6wFcbky"
tags: ["query:sr"]
score: 9.0
evidence: 符号回归结合约束编码与不变性
tldr: 现有符号回归方法面临表示冗余和稀疏奖励两大限制，难以嵌入物理约束。该工作提出基于图的符号回归（GSR），将表达式编码为图，利用置换不变性压缩搜索空间，并支持常量拟合等约束的灵活嵌入。在多个基准上，GSR在准确率和搜索效率上均大幅优于遗传编程和强化学习方法，尤其适用于结合物理先验的科学发现。该框架为符号回归与领域知识的融合提供了有效范式。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有符号回归方法存在表示冗余和难以嵌入物理约束的问题。
method: 提出图结构编码符号表达式，利用置换不变性和约束编码压缩搜索空间。
result: 在合成与真实数据集上，GSR在准确率和搜索速度上均显著优于基线。
conclusion: 图表示能有效提升符号回归的效率和可解释性，尤其适合物理约束场景。
---

## Abstract
Symbolic regression (SR) seeks interpretable analytical expressions that uncover the governing relationships within data, providing mechanistic insight beyond 'black-box' models. However, existing SR methods often suffer from two key limitations: (1) *redundant representations* that fail to capture mathematical equivalences and higher-order operand relations, breaking permutation invariance and hindering efficient learning; and (2) *sparse rewards* caused by incomplete incorporation of constraints that can only be evaluated on full expressions, such as constant fitting or physical-law verification. To address these challenges, we propose a unified framework, **Graph-based Symbolic Regression (GSR)**,  which compresses the search space through the permutation-invariant representations, expression graphs (EGs), that intrinsically encode expression equivalences via a term-rewriting system (TRS) and a directed acyclic graph (DAG) structure. GSR mitigates reward sparsity by employing a hybrid neural-guided Monte Carlo tree search (hnMCTS) on EGs, where constraint-informed neural guidance enables the direct incorporation of expression-level constraint priors, and an adaptive $\epsilon$-UCB policy balances exploration and exploitation. Theoretical analyses establish the uniqueness of our proposed EG representation and the convergence of the hnMCTS algorithm. Experiments on synthetic and real-world scientific datasets demonstrate the efficiency and accuracy of GSR in discovering underlying expressions and adhering to physical laws, offering practical solutions for scientific discovery.

---

## 论文详细总结（自动生成）

# 中文论文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- 符号回归（Symbolic Regression, SR）旨在从数据中发现可解释的解析表达式，提供超越“黑箱”模型的机制性洞察。
- 现有SR方法存在两大关键局限：
  - **表示冗余**：无法捕捉数学等价性和高阶操作数关系，破坏了置换不变性，阻碍高效学习。
  - **稀疏奖励**：由于约束不能完全融入（如常量拟合或物理定律验证），只能在完整表达式上评估，导致奖励信号稀疏。
- 针对上述问题，论文提出统一框架**Graph-based Symbolic Regression (GSR)**，通过置换不变性的图表示压缩搜索空间，并采用混合神经引导蒙特卡洛树搜索（hnMCTS）融入约束先验，以提升符号回归的效率和准确性，特别适用于需要嵌入物理不变性的科学发现任务。

## 2. 论文提出的方法论：核心思想、关键技术细节（用文字说明）

- **核心思想**：将符号表达式编码为**表达式图（Expression Graphs, EGs）**，利用置换不变性自动编码表达式的等价性，从而压缩搜索空间。
- **关键技术细节**：
  - **EG表示**：结合**项重写系统（Term-Rewriting System, TRS）** 和**有向无环图（DAG）结构**，避免因操作数顺序不同而导致的冗余表示。
  - **搜索算法**：基于EG进行**混合神经引导蒙特卡洛树搜索（hnMCTS）**：
    - 使用约束通知的神经指导，直接嵌入表达式级别的约束先验。
    - 设计自适应 ε-UCB 策略，平衡探索与利用。
  - **理论分析**：证明了EG表示的唯一性以及hnMCTS算法的收敛性。
- 整体流程：将候选表达式转化为EG → 在EG上执行hnMCTS → 利用约束引导剪枝和奖励设计 → 输出最终解析表达式。

## 3. 实验设计：数据集 / 场景、benchmark、对比方法

- **数据集 / 场景**：
  - 合成数据集（来源于常见符号回归基准，如Nguyen、Keijzer等，具体名称未列出）。
  - 真实世界科学数据集（如物理定律发现场景，具体数据集未在摘要中详述）。
- **Benchmark**：未明确列出具体基准名称，但提到了“多个基准”。
- **对比方法**：
  - 遗传编程（GP）类方法。
  - 基于强化学习（RL）的符号回归方法。
  - 其他基线（具体名称未提供，摘要仅称“显著优于基线”）。
- **评价指标**：准确率、搜索效率（收敛速度）、物理约束满足程度。

## 4. 资源与算力

- 论文摘要及提供的元数据**未明确说明**使用的GPU型号、数量、训练时长等算力信息。
- 仅能推断为常规机器学习实验配置，但具体细节缺失。

## 5. 实验数量与充分性

- **实验数量**：未给出具体实验组数。从描述看，至少包括合成数据集上的测试和真实数据集上的验证，可能包含消融研究（评估ε-UCB、神经指导等组件的作用）。
- **充分性与公平性**：
  - 充分性受限：由于缺少详细的实验列表、超参数设置、统计显著性检验等，无法判断是否覆盖了足够多的场景和随机种子。
  - 客观公平性：摘要声称“显著优于基线”，但未展示具体误差、运行时间等数值对比，也未说明基线调优程度。缺乏实验细节，需谨慎评估。

## 6. 论文的主要结论与发现

- GSR在合成和真实世界科学数据集上，在**准确率**和**搜索速度**方面均显著优于遗传编程和强化学习方法。
- 图结构（EG）表示能有效压缩搜索空间，提升符号回归的效率和可解释性。
- 通过hnMCTS嵌入物理约束（如对称性、守恒律），GSR特别适合结合物理先验的科学发现任务。

## 7. 优点：方法或实验设计上的亮点

- **表示层面的创新**：利用置换不变性和TRS/DAG结构，从根本上消除冗余表达式，大幅度缩小搜索空间。
- **约束编码灵活性**：通过hnMCTS直接融入表达式级别的约束先验，解决了传统方法难以嵌入全局约束的问题。
- **理论保证**：证明了EG表示的唯一性和hnMCTS的收敛性，提供了扎实的理论基础。
- **自适应探索-利用策略**：ε-UCB机制根据搜索进度动态调整，提高样本效率。
- **神经引导与MCTS结合**：降低了对完全随机搜索的依赖，加速发现高质量表达式。

## 8. 不足与局限

- **实验细节匮乏**：未列出具体数据集、对比方法名称、超参数设置、运行时间、GPU资源等，难以复现和客观比较。
- **基准覆盖有限**：未提及是否在更复杂的表达式（如含三角函数、指数积分等）或大噪声、小样本场景下测试。
- **应用限制**：图表示的构建和TRS可能引入额外计算开销，对于极大规模或实时性要求高的应用可能有挑战。
- **消融实验不明确**：未明确说明对EG表示、hnMCTS各组件（神经指导、ε-UCB）的单独消融效果。
- **可扩展性未讨论**：未分析表达式深度、变量数、约束复杂度对性能的影响。

（完）
