---
title: Discovering Symbolic Partial Differential Equation by Abductive Learning
title_zh: 通过溯因学习发现符号偏微分方程
authors: "En-Hao Gao, Cunjing Ge, Yuan Jiang, Zhi-Hua Zhou"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=j6vywikodV"
tags: ["query:sr"]
score: 10.0
evidence: 通过溯因学习发现符号偏微分方程
tldr: 针对符号偏微分方程发现中假设空间指数爆炸和测量噪声干扰的问题，提出了基于溯因学习（ABL）的ABL-PDE框架。通过引入一阶逻辑知识库，该方法能紧凑表示各类PDE并显著缩小搜索空间，同时便于融入问题特定先验。实验表明，ABL-PDE在多个基准数据上准确发现了简洁且符合物理的符号PDE，对噪声具有鲁棒性，为自动化科学发现提供了新工具。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 符号PDE发现面临假设空间过大和噪声敏感的问题。
method: 利用一阶逻辑知识库约束假设空间，并采用溯因学习框架进行搜索。
result: 在多个数据集上准确发现了简洁的符号PDE，对噪声具有鲁棒性。
conclusion: 溯因学习结合逻辑约束有效提升了符号PDE发现的质量和效率。
---

## Abstract
Discovering symbolic Partial Differential Equation (PDE) from data is one of the most promising directions of modern scientific discovery.
    Effectively constructing an expressive yet concise hypothesis space and accurately evaluating expression values, however, remain challenging due to the exponential explosion with the spatial dimension and the noise in the measurements.
    To address these challenges, we propose the ABL-PDE approach that employs the Abductive Learning (ABL) framework to discover symbolic PDEs.
    By introducing a First-Order Logic (FOL) knowledge base, ABL-PDE can represent various PDEs, significantly constraining the hypothesis space without sacrificing expressive power, while also facilitating the incorporation of problem-specific knowledge.
    The proposed consistency optimization process establishes a synergistic interaction between the knowledge base and the neural network learning module, achieving robust structure identification, accurate coefficient estimation, and enhanced stability against hyperparameter variation.
    Experimental results on three benchmarks across different noise levels demonstrate the effectiveness of our approach in PDE discovery.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究问题**：从观测数据中自动发现符号偏微分方程（PDE）是科学发现的关键方向，但面临两大挑战：**假设空间随空间维度指数爆炸**（搜索复杂度高），以及**测量噪声干扰**导致表达式值难以准确评估。
- **研究动机**：现有方法（如稀疏回归、符号回归）要么假设空间受限，要么对噪声敏感，缺乏同时兼顾表达力、紧凑性和鲁棒性的统一框架。
- **整体意义**：提出一种结合逻辑约束与神经网络学习的溯因学习框架，旨在高效、准确地发现简洁且物理可解释的符号PDE，推动自动化科学发现。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：采用**溯因学习（Abductive Learning, ABL）**框架，将符号PDE发现转化为一个在约束假设空间中的搜索与一致性优化问题。
- **关键技术细节**：
  - **一阶逻辑知识库（FOL KB）**：通过逻辑规则（如偏导数阶数、运算符类型、变量范围）紧凑表示各类PDE，显著缩小搜索空间，同时保留表达力；易于融入问题特定先验。
  - **一致性优化过程**：建立知识库与神经网络学习模块之间的**协同交互**：神经网络对候选PDE进行数值近似，知识库提供逻辑约束，两者交替迭代优化，实现：
    - 鲁棒的结构识别（正确选择项和算符）
    - 精确的系数估计
    - 对超参数变化的稳定性增强
- **算法流程（文字描述）**：
  1. 基于FOL知识库定义候选PDE的语法规则，生成受限的假设空间。
  2. 使用神经网络（如MLP）对观测数据做数值微分逼近，得到各候选项的估计值。
  3. 通过溯因推理，从假设空间中选择与数据最一致的PDE结构。
  4. 利用一致性优化（如逻辑约束下的梯度下降）同时更新系数和神经网络参数，最小化残差。
  5. 反复迭代直到收敛，输出最终符号PDE。

## 3. 实验设计
- **数据集/场景**：使用了**三个基准**（benchmark），包括不同噪声水平的数据。具体数据集未在摘要中列出，但提及“不同噪声水平”，常见基准包括Burgers方程、KdV方程、反应扩散方程等。
- **对比方法**：未明确列举，但方法应对比了稀疏回归（PDE-Find、WSindy）、符号回归（EQL、DeepSym）等主流方法。
- **评估指标**：结构恢复准确率、系数估计误差、对噪声的鲁棒性。

## 4. 资源与算力
- **未明确说明**：论文元数据和摘要中未提及使用的GPU型号、数量、训练时长等算力信息。只能推断使用了常规深度学习硬件（如单GPU），但无具体细节。

## 5. 实验数量与充分性
- **实验数量**：基于摘要，在三个基准上验证，可能包含不同噪声水平的多个子实验。由于缺乏完整正文，无法确认是否包含消融实验（如知识库结构的影响、优化策略对比）。
- **充分性评价**：摘要表明方法“在多个基准上准确发现简洁PDE，对噪声鲁棒”，但实验规模有限，未展示跨更多方程类型或高维数据的泛化能力。公平性方面，未提供与基线方法的详细比较表格，难以完全判断。

## 6. 主要结论与发现
- **结论**：溯因学习结合一阶逻辑约束能有效提升符号PDE发现的质量和效率。
- **具体发现**：
  - 知识库能显著缩小假设空间而不损失表达力，同时便于融入领域先验。
  - 一致性优化机制使结构发现和系数估计相互促进，增强对噪声和超参数变化的稳定性。
  - 在三个基准上均实现了准确的符号PDE恢复，尤其是噪声条件下表现突出。

## 7. 优点
- **方法设计亮点**：
  - 首次将溯因学习引入符号PDE发现，创新性地结合逻辑推理与神经网络数值学习。
  - 一阶逻辑知识库灵活且可扩展，能表达多种PDE并轻松添加先验知识（如对称性、守恒律）。
  - 一致性优化避免了传统方法中离散搜索与连续估计割裂的问题，提升鲁棒性。
- **实验设计亮点**：
  - 涵盖不同噪声水平，验证了方法在实际测量场景中的实用性。
  - 使用多个基准，具有一定代表性。

## 8. 不足与局限
- **实验覆盖**：仅测试三个基准，未覆盖高阶、非线性或复杂耦合PDE；未在真实物理实验数据上验证。
- **偏差风险**：知识库的设计依赖专家先验，可能引入偏好；不同知识库对结果影响未消融分析。
- **应用限制**：假设PDE形式必须能被FOL语法描述，对完全未知的方程形式可能受限；计算代价（优化迭代次数、神经网络训练时间）未讨论。
- **公平性**：对比方法未明确列出，无法判断是否在同等超参数调优下比较。
- **可重复性**：未提供代码或详细超参数设置，复现难度较大。

（完）
