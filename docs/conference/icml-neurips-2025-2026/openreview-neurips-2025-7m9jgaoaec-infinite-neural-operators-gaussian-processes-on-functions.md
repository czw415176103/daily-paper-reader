---
title: "Infinite Neural Operators: Gaussian processes on functions"
title_zh: 无限神经算子：函数上的高斯过程
authors: "Daniel Augusto de Souza, Yuchen Zhu, Harry Jake Cunningham, Yuri Saporito, Diego Mesquita, Marc Peter Deisenroth"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=7M9JGaoAeC"
tags: ["query:physics-ml"]
score: 7.0
evidence: 建立了神经算子与高斯过程之间的理论联系
tldr: 本文证明了在何种条件下，任意深度的神经算子会收敛到函数值高斯过程。该结果为使用高斯过程工具对神经算子进行不确定性量化提供了理论基础，并推导了两种常用神经算子（包括傅里叶神经算子）的协方差函数，桥接了深度算子学习与贝叶斯方法。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 神经算子的不确定性量化能力有限，而高斯过程提供了天然的预测分布。
method: 分析神经算子的无限宽度极限，证明其在高斯权重假设下收敛为函数值高斯过程。
result: 推导了傅里叶神经算子等两种参数化下的协方差函数，理论建立联系。
conclusion: 该理论为神经算子注入了贝叶斯机制，但实际应用尚需进一步推进。
---

## Abstract
A variety of infinitely wide neural architectures (e.g., dense NNs, CNNs, and transformers) induce Gaussian process (GP) priors over their outputs.
These relationships provide both an accurate characterization of the prior predictive distribution and enable the use of GP machinery to improve the uncertainty quantification of deep neural networks.
In this work, we extend this connection to neural operators (NOs), a class of models designed to learn mappings between function spaces.
Specifically, we show conditions for when arbitrary-depth NOs with Gaussian-distributed convolution kernels converge to function-valued GPs.
Based on this result, we show how to compute the covariance functions of these NO-GPs for two NO parametrizations, including the popular Fourier neural operator (FNO).
With this, we compute the posteriors of these GPs in regression scenarios, including PDE solution operators.
This work is an important step towards uncovering the inductive biases of current FNO architectures and opens a path to incorporate novel inductive biases for use in kernel-based operator learning methods.

---

## 论文详细总结（自动生成）

# 论文中文总结：Infinite Neural Operators: Gaussian processes on functions

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **动机**：神经算子（Neural Operators, NOs）是一类学习函数空间之间映射的模型，在求解偏微分方程（PDE）等科学计算任务中表现出色，但其不确定性量化能力有限。相比之下，高斯过程（GP）提供了天然的预测分布和不确定性估计。先前工作已证明无限宽度的全连接神经网络、CNN和Transformer会收敛为高斯过程，但这一理论联系尚未拓展至神经算子。
- **背景**：本文旨在建立神经算子与高斯过程之间的理论联系，证明在一定条件下，任意深度的神经算子会收敛到函数值高斯过程（function-valued GP），从而为神经算子注入贝叶斯机制，并推导出常用神经算子（如傅里叶神经算子 FNO）的协方差函数。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：分析神经算子在无限宽度极限下的行为。假设神经算子的卷积核（包括位置空间的核和傅里叶空间的核）服从高斯分布，证明在宽度趋向无穷时，整个算子输出的联合分布收敛为函数值高斯过程。
- **关键技术细节**：
  - 推导了任意深度神经算子收敛为高斯过程的充分条件（例如，卷积核的分布需满足中心极限定理，非线性激活函数需满足某些增长约束）。
  - 基于该结果，针对两种具体神经算子参数化形式（包括标准卷积神经算子和傅里叶神经算子 FNO）计算了其对应的 GP 协方差函数。
  - 利用这些协方差函数，在回归场景（包括 PDE 解算子学习）中计算高斯过程的后验分布，从而实现不确定性量化。
- **算法流程**（文字说明）：首先定义具有高斯分布卷积核的神经算子架构 → 在无限宽度极限下，利用随机过程理论证明其输出收敛为高斯过程 → 推导协方差函数的闭式表达式 → 使用高斯过程后验推断进行预测和不确定性估计。

## 3. 实验设计：数据集/场景、基准、对比方法
- **数据集/场景**：PDE 解算子回归场景，例如学习从初始条件或参数到 PDE 解的映射（具体数据集名称未在提供的文本中详列，仅提及“PDE solution operators”）。
- **基准**：未明确提及具体基准对比方法，但理论上可以与标准神经算子（如 FNO）以及标准高斯过程回归进行对比。
- **对比方法**：由于本文是理论证明为主，实验部分可能旨在展示由理论导出的协方差函数在实际回归任务中的有效性，而非大规模方法对比。提供的文本未给出详细对比列表。

## 4. 资源与算力
- 文中未明确说明使用的 GPU 型号、数量、训练时长等算力信息。仅提及“computing posteriors”和“regression scenarios”，但未涉及大规模训练资源。因此无法总结具体算力，需要指出这一缺失。

## 5. 实验数量与充分性
- **实验数量**：从提供的文本看，主要实验可能包括在 PDE 解算子回归任务上对两种神经算子（标准卷积和 FNO）对应的 GP 后验进行测试，但未说明具体数据集数量或消融实验。推测实验数量有限，侧重于验证理论推导的正确性。
- **充分性与公平性**：实验设计可能较简单，主要用于验证理论结果的可行性，而非全面竞争性评估。未提及与其他方法的公平比较或消融分析，因此充分性一般。

## 6. 论文的主要结论与发现
- 证明在满足高斯分布卷积核和一定条件下，任意深度的神经算子会收敛为函数值高斯过程。
- 推导了傅里叶神经算子（FNO）等两种常用神经算子的协方差函数解析形式。
- 该理论为神经算子提供了贝叶斯解释，使得可以使用高斯过程工具进行不确定性量化，并能揭示当前 FNO 架构的归纳偏差，为基于核的算子学习方法引入新的归纳偏差开辟道路。
- 实验验证了这些协方差函数在 PDE 解算子回归中的后验计算是可行的。

## 7. 优点：方法或实验设计上的亮点
- **理论贡献突出**：首次将无限宽度极限下的高斯过程联系推广到神经算子领域，建立了深度算子学习与贝叶斯方法之间的桥梁。
- **协方差函数可解析计算**：针对 FNO 等实际常用架构给出闭式协方差函数，便于实际应用。
- **提供不确定性量化途径**：使原本缺乏不确定度的神经算子能够获得预测分布，对科学计算和可靠性要求高的场景意义重大。
- **启发新归纳偏差**：结果有助于理解当前 FNO 的设计并设计新的基于核的算子学习方法。

## 8. 不足与局限
- **实际应用距离**：目前仅建立理论联系，并展示了简单的回归场景，距离大规模实际应用尚需推进（如更复杂的 PDE、高维问题、训练效率等）。
- **实验覆盖不够全面**：缺少与其他不确定性量化方法（如贝叶斯神经网络、集成方法）的定量对比，也未涵盖多种不同类型的神经算子。
- **假设限制**：依赖高斯分布卷积核和无限宽度极限，真实有限宽度下近似程度未知，且实际中卷积核往往不是严格高斯分布。
- **算力资源信息缺失**：无法评估方法的计算开销。可能在高维或大量数据时协方差矩阵计算成为瓶颈（标准 GP 的 O(N³) 问题需近似，但文中未提及如何处理）。
- **消融实验缺失**：未探讨不同宽度、深度、激活函数对收敛至高斯过程的影响。

（完）
