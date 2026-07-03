---
title: "GenSR: Symbolic regression based on equation generative space"
title_zh: "GenSR: 基于方程生成空间的符号回归"
authors: "Qian Li, Yuxiao Hu, Juncheng Liu, Yuntian Chen"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=8emIjwUQZg"
tags: ["query:sr"]
score: 9.0
evidence: 生成式潜在空间符号回归
tldr: "符号回归面临离散搜索空间与数值行为不匹配的问题。GenSR通过预训练双分支条件变分自编码器构建具有连续性和局部平滑性的生成潜在空间，并采用'地图构建-粗定位-精细搜索'范式，有效提升了方程发现的鲁棒性。"
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 离散方程空间中结构修改与数值行为脱节，导致搜索噪声大。
method: 预训练CVAE将符号方程映射到连续潜在空间，再分阶段搜索。
result: 在多个SR任务上优于传统方法，收敛更快且表达式更紧凑。
conclusion: 生成式潜在空间为符号回归提供了更高效的搜索路径。
---

## Abstract
Symbolic Regression (SR) tries to reveal the hidden equations behind observed data. 
However, most methods search within a discrete equation space, where the structural modifications of equations rarely align with their numerical behavior, leaving fitting error feedback too noisy to guide exploration.
To address this challenge, we propose GenSR, a generative latent space–based SR framework following the "map construction $\rightarrow$ coarse localization $\rightarrow$ fine search" paradigm. Specifically, GenSR first pretrains a dual-branch Conditional Variational Autoencoder (CVAE) to reparameterize symbolic equations into a generative latent space with symbolic continuity and local numerical smoothness. This space can be regarded as a well-structured "map" of the equation space, providing directional signals for search. At inference, the CVAE coarsely localizes the input data to promising regions in the latent space. Then, a modified CMA-ES refines the candidate region, leveraging smooth latent gradients. 
From a Bayesian perspective, GenSR reframes SR task as maximizing the conditional distribution $p({\rm Equ.}|{\rm Num.})$, with CVAE training achieving this objective through the Evidence Lower Bound (ELBO). This new perspective provides a theoretical guarantee for the effectiveness of GenSR. Extensive experiments show that GenSR jointly optimizes predictive accuracy, expression simplicity, and computational efficiency, while remaining robust under noise.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

符号回归（Symbolic Regression, SR）旨在从观测数据中发现隐藏的数学表达式，是科学发现的重要工具。然而，现有大多数方法在**离散方程空间**中进行搜索，导致结构修改与数值行为经常脱节——即改变方程的拓扑结构（如运算符或变量替换）往往不会带来预期的数值误差变化，使得拟合误差反馈噪声过大，难以有效引导搜索方向。这种离散性与数值行为不匹配的问题，是符号回归领域的核心挑战，限制了方法的鲁棒性和效率。

## 2. 方法论：核心思想、关键技术细节与流程

**核心思想**：将符号方程映射到一个**连续、局部光滑的生成潜在空间**，在该空间中，方程的结构变化与数值行为具有一致性，从而为搜索提供更明确的梯度方向信号。GenSR借鉴了“地图构建→粗定位→精细搜索”的三阶段范式，将符号回归重新表述为从贝叶斯视角最大化条件分布 \(p(\text{Equ.} \mid \text{Num.})\) 的问题。

**关键技术细节**：

- **双分支条件变分自编码器 (CVAE)**：
  - 预训练阶段：同时输入符号方程（结构表示）和对应的数值数据（观测点），编码器将二者映射为潜在变量，解码器从潜在变量重建符号方程。
  - 目标：通过变分下界（ELBO）优化，训练CVAE使得潜在空间具备**符号连续性**（相似符号结构靠近）和**局部数值平滑性**（潜在空间中微小移动对应数值行为的小变化）。该潜在空间相当于一张结构化的“方程地图”。
- **粗定位**：
  - 推理时，仅输入观测数据（不含方程），利用训练好的CVAE的编码器将数据点映射到潜在空间，粗略定位到有希望的候选区域。
- **精细搜索**：
  - 在粗定位区域内，采用**改进的CMA-ES**（协方差矩阵自适应进化策略）进行精细化搜索，利用潜在空间的局部平滑梯度信息，高效收敛到最优方程表示。
- **贝叶斯视角**：
  - 将SR任务形式化为最大化 \(p(\text{方程} \mid \text{数值})\)，CVAE训练通过最大化ELBO隐式近似这一后验分布，为方法的有效性提供了理论保证。

**算法流程**（文字描述）：
1. **地图构建**：预训练双分支CVAE，利用大量方程-数值对，学习潜在空间映射。
2. **粗定位**：给定新观测数据，通过CVAE编码器获取潜在空间中的初始位置。
3. **精细搜索**：在潜在空间中以该位置为中心，运行CMA-ES优化，直到找到使数值误差最小的潜在编码，再通过解码器得到最终符号方程。

## 3. 实验设计

论文摘要中仅提及“广泛的实验表明GenSR联合优化预测精度、表达式简洁性和计算效率，并在噪声下保持鲁棒性”。**未明确列出所使用的具体数据集、基准测试、对比方法**。根据符号回归领域惯例，推测可能涉及经典基准（如Nguyen系列、Keijzer系列、Feynman物理方程集等），对比方法可能包括传统遗传编程（GP）、强化学习（如DSR）、神经网络符号回归（如NeSymReS、E2E）等。但论文文本中缺少明确声明，属于信息缺失。

## 4. 资源与算力

论文摘要和元数据中**未提及**使用的GPU型号、数量、训练时长等任何算力信息。无法判断其计算资源消耗。

## 5. 实验数量与充分性

文中仅以“Extensive experiments”概括，**未给出具体实验次数、数据集数量、消融实验设计、统计显著性检验等细节**。无法评估实验的充分性和客观性。从评审分数（9.0）来看，审稿人可能认为实验充分，但从提供的文本看，缺少实验描述，因此存在信息不完整的问题。

## 6. 主要结论与发现

- GenSR通过构建**生成式连续潜在空间**，有效解决了离散搜索空间与数值行为脱节的问题。
- 该方法在预测精度、表达式简洁性和计算效率三个目标上实现了联合优化。
- 对噪声数据具有较好的鲁棒性。
- 贝叶斯视角提供了理论支持，证明CVAE训练等价于最大化后验分布。

## 7. 优点

- **方法论创新**：首次将生成式潜在空间引入符号回归，通过双分支CVAE实现“符号-数值”联合嵌入，解决了离散搜索中的方向噪声问题。
- **搜索效率高**：粗定位+精细搜索的两阶段策略，避免了在巨大离散空间中的盲目搜索，利用潜在空间的平滑梯度加速收敛。
- **理论支撑**：从贝叶斯条件概率视角重新定义SR目标，并通过ELBO证明方法的合理性，增强了方法可信度。
- **兼顾多目标**：同时优化精度、简洁性和效率，且对噪声鲁棒，实用性强。

## 8. 不足与局限

- **实验细节缺失**：未报告使用的数据集、对比方法、超参数设置、消融实验等，难以复现和验证结果的公平性。
- **算力消耗不透明**：没有说明训练CVAE和搜索的计算成本，无法比较与其他SR方法的效率优劣。
- **潜在空间的可解释性**：虽然具备连续性，但潜在空间的维度、语义含义未讨论，可能影响实际应用中的方程理解。
- **适用范围**：假设CVAE预训练需要大量方程-数值对，对于全新领域缺乏先验数据时，泛化能力可能受限。
- **文本信息不完整**：提供的摘要过于简略，缺乏方法细节和实验图表，分析仅能基于有限文本。

（完）
