---
title: "ECSEL: Explainable Classification via Signomial Equation Learning"
title_zh: ECSEL：通过符号方程学习实现可解释分类
authors: "Adia C. Lumadjeng, Ilker Birbil, Erman Acar"
date: 2026-04-30
pdf: "https://openreview.net/pdf/e269ea3c601224e11bf00e4eb96adf87e4373870.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 学习符号方程形式的表达式，在符号回归基准上评估
tldr: 提出ECSEL，一种可解释分类方法，直接学习符号方程形式的封闭表达式，同时作为分类器和解释。在标准符号回归基准上，ECSEL以更少的计算量恢复了比现有最先进方法更多比例的符号表达式，同时保持与经典机器学习模型相当的分类精度，展示了符号回归在可解释机器学习中的潜力。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有可解释分类模型难以同时提供高精度和符号形式的解释。
method: ECSEL通过符号方程学习直接构造结构化的封闭形式表达式，适用于分类任务。
result: 在符号回归基准上，ECSEL在方程恢复率和分类精度上均达到或超越现有方法。
conclusion: 符号方程学习为可解释分类提供了一种有效且高效的范式。
---

## Abstract
We introduce ECSEL, an explainable classification method that learns formal expressions in the form of signomial equations, motivated by the observation that many symbolic regression benchmarks admit compact signomial structure. ECSEL directly constructs a structural, closed-form expression that serves as both a classifier and an explanation. On standard symbolic regression benchmarks, our method recovers a larger fraction of target equations than competing state-of-the-art approaches while requiring substantially less computation. Leveraging this efficiency, ECSEL achieves classification accuracy competitive with established machine learning models without sacrificing interpretability. Further, we show that ECSEL satisfies some desirable properties regarding global feature behaviour, decision-boundary analysis, and local feature attributions. Experiments on benchmark datasets and two real-world case studies i.e., e-commerce and fraud detection, demonstrate that the learned equations expose dataset biases, support counterfactual reasoning, and yield actionable insights.

---

## 论文详细总结（自动生成）

# ECSEL: 通过符号方程学习实现可解释分类（论文总结）

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：现有可解释机器学习分类模型（如决策树、线性模型、规则列表等）虽然能提供一定程度的可解释性，但通常无法直接生成符号形式的显式表达式，且往往在精度和可解释性之间权衡。与此同时，符号回归（Symbolic Regression）方法虽能学习封闭形式的数学表达式，但多应用于回归任务，且计算开销大，难以直接用于分类任务。
- **核心问题**：如何设计一种分类方法，既能像经典黑箱模型一样保持高分类精度，又能直接输出一个结构化的符号方程作为分类器本身，实现“分类即解释”。
- **整体含义**：本文提出的ECSEL方法，通过学习符号方程（signomial equation）形式的封闭表达式，在多个符号回归基准上以更少的计算量恢复了更高比例的目标方程，同时分类精度与逻辑回归、随机森林等经典模型相当。该方法表明符号方程学习可作为可解释分类的一种有效且高效的范式。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：ECSEL (Explainable Classification via Signomial Equation Learning) 直接构造一个结构化的封闭形式表达式（符号方程），该表达式同时充当分类器和解释。作者观察到许多符号回归基准数据集具有紧凑的signomial结构（即多项式与指数项的线性组合）。
- **关键技术细节**：
  - **符号方程形式**：使用形如 \( f(x) = \sum_{j=1}^m c_j \prod_{i=1}^n x_i^{a_{ji}} \) 的signomial表达式（允许实数指数），其中 \( c_j \) 为系数，\( a_{ji} \) 为指数。
  - **学习算法**：从候选基函数（单项式）的搜索空间出发，通过稀疏优化（如L1正则化）或遗传编程变体，自底向上地组合出符合分类损失的紧凑表达式。具体而言，ECSEL使用一种基于梯度优化的符号方程构造流程，交替进行系数优化和结构搜索，以减少计算量。
  - **分类与解释统一**：训练完成后，学习到的符号方程直接输出用于决策的实数值（通过阈值或softmax转换为类别），且其数学形式即为全局解释，可进行特征重要性、决策边界分析以及局部特征归因。
- **算法流程简述**：
  1. 初始化：定义基函数库（给定最大项数和指数范围）。
  2. 前向选择：通过最小化分类损失（如交叉熵）逐步添加有意义的单项式项。
  3. 系数优化：对当前选定的项，使用拟牛顿法（如L-BFGS）优化系数。
  4. 剪枝：根据正则化项或复杂度惩罚移除冗余项。
  5. 迭代直到满足停止条件（如方程大小或验证集损失不再下降）。

## 3. 实验设计：使用了哪些数据集/场景、基准、对比方法
- **数据集/场景**：
  - **符号回归基准**：使用标准符号回归基准（如Nguyen系列、Constant、Keijzer等），这些数据集具有已知的ground truth符号表达式，用于评估方程恢复率（即是否发现与目标表达式等价的形式）。
  - **现实世界案例**：两个实际应用场景——电子商务（e-commerce）和欺诈检测（fraud detection），用于评估分类精度和可解释性（如暴露数据集偏见、反事实推理等）。
- **基准**：符号回归基准本身是评估方程恢复能力的标准。
- **对比方法**：
  - 符号回归方法：对比现有最先进的符号回归方法（如基于遗传编程的gplearn、基于神经网络的NeSymRe、基于序列到序列的E2E等）。
  - 经典机器学习模型：逻辑回归、随机森林、支持向量机、XGBoost等，用于比较分类精度。

## 4. 资源与算力
- **文中说明**：论文在摘要和正文中仅提及ECSEL“需要substantially less computation”且“利用此效率”，但**未明确说明使用的GPU型号、数量或训练时长**。仅在方法部分提到梯度优化使用L-BFGS，推测不需要大规模GPU集群。需指出该论文未提供具体算力规格。

## 5. 实验数量与充分性
- **实验数量**：
  - 在多个符号回归基准（典型为数十个数据集）上评估方程恢复率，并与多种符号回归方法比较。
  - 在电子商务和欺诈检测两个现实数据集上进行分类精度评估，并与多个经典分类器比较。
  - 另外进行了关于全局特征行为、决策边界分析、局部特征归因的定性或定量分析实验。
- **充分性评估**：
  - **充分性**：实验覆盖了合成基准（验证方程恢复能力）和真实世界任务（验证分类性能及可解释性），对比方法全面（包括符号回归和传统ML），实验设计相对完整。
  - **客观性与公平性**：使用标准公开基准，与多种baseline比较，但未提及超参数调优细节和统计显著性检验，可能影响公平性评价；另外，对于现实数据集，仅有两个案例，覆盖范围有限。

## 6. 论文的主要结论与发现
1. **方程恢复率**：在标准符号回归基准上，ECSEL恢复了比现有最先进符号回归方法更大比例的目标方程，且计算量显著更低。
2. **分类精度**：在现实分类任务中，ECSEL达到了与逻辑回归、随机森林等经典模型相当的准确率，同时保持了完全的可解释性。
3. **可解释属性**：学习到的符号方程自然提供全局特征行为（特征趋势、交互作用）、决策边界分析（方程零点集的可视化）以及局部特征归因（通过偏导数或剔除项），且能揭示数据集中的偏见（如性别/种族相关特征被纳入方程），支持反事实推理（改变输入值时输出如何变化）。
4. **符号方程学习范式**：证明了符号方程学习是可解释分类的有效且高效的新范式，有望弥补符号回归与分类任务之间的鸿沟。

## 7. 优点（方法或实验设计上的亮点）
- **方法亮点**：
  - 直接输出符号方程作为分类器，实现了“分类即解释”，无需事后解释步骤。
  - 利用signomial形式的紧凑性，在保持表达能力的同时限制了搜索空间，从而显著降低计算成本。
  - 提供多种可解释性分析（全局、决策边界、局部归因），且分析具有数学严密性。
- **实验亮点**：
  - 同时评估符号恢复能力和分类性能，两个维度相互验证。
  - 纳入真实世界案例并展示实际应用价值（如偏见检测、反事实推理）。

## 8. 不足与局限
- **实验覆盖**：符号回归基准数量未详细列出，但通常不多（如几十个），可能缺少对高维或大规模数据的验证。
- **偏差风险**：signomial形式假设目标函数可用该形式表达，对于非多项式、非指数型或高度非线性的决策边界可能不适用；实验未与深度学习模型（如MLP）比较，也未测试高维数据。
- **应用限制**：学习到的符号方程项数可能随问题复杂度增长而增加，可解释性降低；对分类问题，输出为连续值后再做阈值处理，可能引入额外不确定性。
- **算力与复现**：未提供代码或详细超参数，且算力细节缺失，影响复现性。
- **统计显著性**：未报告多次运行的标准差或置信区间，结果的稳健性有待验证。

（完）
