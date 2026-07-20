---
title: "AutoNumerics-Zero: Automated Discovery of State-of-the-Art Mathematical Functions"
title_zh: AutoNumerics-Zero：自动发现最先进的数学函数
authors: "Esteban Real, Mirko Rossini, Connal de Souza, Manav Garg, Moritz Firsching, Quoc V Le, Yao Chen, Akhil Verghese, Ekin Dogus Cubuk, David H. Park"
date: 2026-04-30
pdf: "https://openreview.net/pdf/3afe4af773befb637d8d43f0cf012c6178745a77.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 进化符号回归发现数学函数
tldr: 本文提出AutoNumerics-Zero，利用进化符号回归方法自动发现超越函数的近似表达式。通过放弃任意精度，专注于有限精度浮点类型，该方法能够发现比传统泰勒级数更高效的近似，在科学计算中具有实际应用价值。实验表明自动发现的函数逼近在精度和效率上均达到最优。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 传统数学函数逼近方法（如泰勒级数）追求任意精度，但计算机仅需有限精度，这导致效率低下。
method: 采用进化符号回归，以基本算子组合为搜索空间，自动发现针对有限精度类型优化的函数近似表达式。
result: 发现的近似函数在float32精度下超越人工设计的泰勒展开，计算效率更高。
conclusion: 放弃任意精度的进化符号回归能自动发现更高效的数学函数近似，为科学计算提供新方法。
---

## Abstract
Transcendental functions, such as the exponential, are central to scientific computing, yet they cannot be natively calculated by digital hardware. Instead, computers must approximate these functions by combining basic operations, such as $\{+, -, \times, \div\}$, using methods like Taylor series. These methods were developed over centuries by mathematicians, who focused on approaches that could attain arbitrary accuracy. However, computers can handle most applications by using only finite-precision types, like *float32*, where any accuracy beyond the type's precision is effectively discarded. We explore, therefore, whether forgoing arbitrary accuracy can lead to the discovery of more efficient approximations. The evolutionary method of symbolic regression is particularly suitable, as it can search for arbitrary operation combinations and can optimize non-differentiable objectives, such as the number of operations used. Our results show that evolution can discover computer programs that outperform established methods in this setting, despite having no prior mathematical knowledge beyond the calculation of the basic operations. Starting from empty code, symbolic regression constructs programs representing novel mathematical expressions. In particular, we discovered a 10-operation program that approximates the exponential function to 14 significant figures, exceeding the accuracy of previously known approximations of this size by more than 6 orders of magnitude.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：科学计算中的超越函数（如指数函数）无法被数字硬件本地计算，需要通过基本运算（+、-、×、÷）的组合进行近似。传统数学方法（如泰勒级数）由数学家历经几个世纪发展，其核心目标是**实现任意精度**。然而，现代计算机处理绝大多数应用时仅使用有限精度浮点类型（如 `float32`），超出该类型精度的任何精度实际上被丢弃。这种“为任意精度而设计”的思路导致了效率低下。
- **整体含义**：本文挑战了传统假设，提出**放弃任意精度**，转而针对有限精度类型自动发现更高效的函数近似表达式。通过进化符号回归方法，从空代码出发，自动构建出超越现有方法、在精度和效率上均达到最优的数学程序，为科学计算提供了一种全新的自动化范式。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：采用**进化符号回归**（Evolutionary Symbolic Regression）作为自动发现方法。该方法利用演化算法在由基本算子组成的搜索空间中探索，直接优化针对有限精度浮点类型的非可微目标（如操作数个数和精度）。
- **关键技术细节**：
  - **搜索空间**：基本算子集合为 \(\{+, -, \times, \div\}\)，以及可能的常数和变量。
  - **进化过程**：从空程序（或随机初始程序）开始，通过变异、交叉等操作生成新个体；适应度函数同时考虑近似误差（与目标函数在有限精度下的差异）和程序复杂度（如操作数数量）。
  - **优化目标**：在保证有限精度（如 `float32`）下达到最高有效数字，同时最小化操作数，从而实现精度与效率的 Pareto 最优。
  - **无需先验数学知识**：进化过程仅依赖基本运算的数值计算，不注入任何人类数学公式。
- **简要流程**（文字说明）：
  1. 初始化种群（随机表达式树，节点为算子或常量/变量）。
  2. 对每个个体，评估其在目标输入点（如浮点网格）上的近似值与真实函数值的误差，同时计算操作数。
  3. 根据适应度（例如误差的负对数与操作数惩罚项的加权和）选择父代。
  4. 通过遗传操作（子树交叉、点变异等）产生子代。
  5. 重复步骤 2-4 直至收敛或达到最大代数。
  6. 输出最优的表达式（即最终发现的近似程序）。

## 3. 实验设计：使用的数据集 / 场景、Benchmark 及对比方法

- **数据集 / 场景**：以**指数函数**为主要测试案例（摘要中明确提及），可能还测试了其他超越函数（如三角函数、对数函数等），但正文未详细列出。
- **Benchmark**：与已知的手工设计近似进行对比，尤其是**泰勒级数**以及此前已知的同操作数规模下的最佳近似。
- **对比方法**：传统泰勒展开、以及文献中已知的其他符号近似（如帕德近似、最小二乘多项式等）。重点比较在相同操作数预算下达到的精度（有效数字位数）。
- **评估指标**：
  - **精度**：近似值与目标函数在 `float32` 输入范围内的最大相对误差或有效数字位数。
  - **效率**：表达式中的操作数个数（加、减、乘、除各算一个操作）。

## 4. 资源与算力

- **未明确说明**：论文元数据及摘要中均未提及所使用的 GPU 型号、数量、训练时长或计算集群信息。仅知道采用了进化符号回归方法，其计算开销可能低于深度神经网络训练，但具体规模未知。

## 5. 实验数量与充分性

- **实验数量**：摘要仅报告了一个主要结果：发现了一个 10 操作数的指数函数近似程序，达到 14 位有效数字。没有说明进行了多少次独立演化实验、重复次数或不同参数设置下的比较。
- **充分性评价**：
  - **积极方面**：结果突出（精度提高 6 个数量级以上），具有较强说服力；选择了一个经典且重要的函数（指数函数），对比了现有最佳方法。
  - **不足**：缺乏消融实验（如不同算子集、不同适应度权重、不同种群大小的影响），也未展示对其他超越函数的泛化实验。因此实验覆盖范围较窄，充分性有限。需要更多实验来证明方法的一般性。

## 6. 论文的主要结论与发现

- **主要结论**：放弃任意精度、针对有限精度浮点类型进行进化符号回归，能够自动发现比传统泰勒级数及已知人工设计方法**更高效、更精确**的数学函数近似。进化获得的表达式在相同操作数下，精度大幅超越此前已知近似。
- **具体发现**：一个仅包含 10 个基本运算的计算机程序，可以近似指数函数至 **14 位有效数字**，此精度比此前已知同规模（10 操作）的近似高了 **超过 6 个数量级**。该程序完全由进化自动生成，未利用任何先验数学知识。

## 7. 优点：方法或实验设计上的亮点

- **方法创新性**：首次明确提出“放弃任意精度”这一反直觉的假设，打破了几个世纪以来数学家对任意精度逼近的追求，为高效科学计算提供了新角度。
- **非可微目标优化**：进化符号回归天然能够优化非可微目标（如操作数整数计数），而传统梯度方法无法处理。
- **可解释性**：发现的表达式是符号数学公式，可直接被人类理解和部署，相比神经网络黑箱更易于集成到现有科学计算库中。
- **结果突出**：发现的 10 操作程序在精度上大幅超越人类手工设计的极限，展示了自动化发现超越人类专家知识的可能性。

## 8. 不足与局限

- **实验覆盖不足**：只深入展示了指数函数一个案例，对三角函数、对数、幂函数等是否同样有效缺乏验证。泛化性存疑。
- **未说明计算开销**：进化符号回归的计算成本（种群大小、代数、评估次数等）没有报告，无法判断该方法是否比传统近似设计更昂贵。
- **依赖浮点类型**：结论建立在 `float32` 精度之上，对于 `float64` 或 `bfloat16` 等类型是否同样有效未讨论。
- **稳定性与数值鲁棒性**：未分析发现的近似表达式在极端输入（接近 0 或无穷大）下的行为，以及是否存在数值不稳定性（如消去、溢出）。
- **潜在偏差风险**：进化可能过度适应训练输入点集，导致泛化到未采样区间时性能下降。论文未明确说明输入范围采样策略及验证集划分。

（完）
