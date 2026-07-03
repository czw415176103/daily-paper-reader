---
title: "Projective Symbolic Regression: Solving High-Dimensional PDE by Learning from Low-Dimensional Projections"
title_zh: 投影符号回归：从低维投影学习求解高维偏微分方程
authors: "Lulu Cao, Yinglan Feng, Liang Feng, Ran Cheng, KC Tan"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=7noPldewfE"
tags: ["query:sr"]
score: 9.0
evidence: 从低维投影进行符号回归求解高维偏微分方程
tldr: 该论文针对符号回归在高维偏微分方程中面临维数灾难的问题，提出投影符号回归（PSR）框架。PSR通过对部分输入变量固定生成低维投影数据，对每个投影应用符号回归提取局部函数成分，再组合成全局解。该方法有效缓解了维数灾难，为高维物理系统的方程发现提供了新途径。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 直接对高维PDE应用符号回归受限于维数灾难。
method: 提出PSR框架，通过低维投影分解降低维度，再组合符号回归发现的局部函数。
result: PSR成功求解了高维PDE，并从低维投影中恢复了有意义的物理规律。
conclusion: 基于投影的符号回归策略可扩展至高维物理系统，有望推动科学发现。
---

## Abstract
Symbolic regression (SR) provides a powerful means for uncovering the underlying mathematical structure of physical systems, such as those governed by partial differential equations (PDEs). However, applying SR directly to high-dimensional PDEs remains intractable due to the curse of dimensionality. To address this, we propose Projective Symbolic Regression (PSR), a novel framework that solves high-dimensional PDEs by learning from low-dimensional projections. PSR first generates multiple projections of the PDE solution data by fixing subsets of input variables. Symbolic regression is then applied to each projection to extract compact, localized functional components. These components are subsequently composed into a unified global expression through a higher-level symbolic program. Critically, the final composition is constrained by minimizing the PDE residual error, ensuring physical validity. Empirical results demonstrate that PSR not only improves predictive accuracy over conventional methods but also yields interpretable models that reveal the compositional structure of the underlying physical dynamics.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文元数据及摘要信息生成的详细中文总结。

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：符号回归（Symbolic Regression, SR）作为一种发现物理系统背后数学结构（如偏微分方程）的强大工具，在应用于**高维偏微分方程（PDE）** 时遭遇了严重的**维度灾难（curse of dimensionality）**。随着输入变量维度的增加，SR的搜索空间呈指数级增长，直接应用变得难以处理。
- **研究动机**：为了突破高维PDE求解的瓶颈，作者希望借鉴降维思想，将高维问题分解为多个低维子问题，从而缓解维度灾难，同时保留符号回归的可解释性和物理一致性。
- **整体含义**：该论文提出了**投影符号回归（Projective Symbolic Regression, PSR）** 框架，通过从低维投影学习来间接求解高维PDE，为复杂高维物理系统的方程发现提供了新的可行路径。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：将高维PDE的解数据通过**固定部分输入变量**的方式，生成多个低维投影数据集；对每个投影独立应用符号回归，提取简洁的局部函数成分；最后将这些局部成分通过一个更高层次的符号程序组合成一个全局解，并以最小化PDE残差为约束保证物理有效性。

- **关键技术细节**：
    1. **低维投影生成**：对原始高维解数据 $u(x_1, x_2, \dots, x_d)$，选择若干个输入子集（例如每次只保留一维或两维变量，固定其余变量为常数值），生成相应的低维投影数据 $\{u^{(i)}(x_k)\}$ 或 $\{u^{(i)}(x_k, x_l)\}$。
    2. **局部符号回归**：对每个投影数据，使用符号回归算法（如遗传编程）学习一个紧凑的解析表达式 $f_i$，使其能够准确拟合该投影下的数据。这些 $f_i$ 代表高维解在特定子空间上的局部行为。
    3. **全局组合**：设计一个高层次的符号程序（higher-level symbolic program），将多个局部函数 $f_i$ 组合成统一的全局表达式 $U(x_1,\dots,x_d)$。组合方式可以是加法、乘法或其他可微运算。
    4. **物理约束优化**：最终全局表达式 $U$ 必须满足原PDE的残差最小化，即 $\mathcal{R} = \mathcal{L}[U] - f(x)$ 的范数（如L2范数）尽可能小，从而保证模型不仅拟合数据，还符合物理规律。

- **算法流程（文字说明）**：
    - 输入：高维PDE解的快照数据，以及PDE算子 $\mathcal{L}$ 和外力项 $f$。
    - 步骤1：根据先验知识或随机策略，生成若干低维投影（如固定除少数变量外的所有变量）。
    - 步骤2：对每个投影，运行标准符号回归（如基于遗传编程），得到局部表达式 $f_i$。
    - 步骤3：构建全局表达式 $U$ 的骨架（例如线性组合 $U = \sum_i w_i f_i$ 或更复杂的结构），其中权重 $w_i$ 或连接方式待定。
    - 步骤4：利用梯度优化或进化算法，调整全局表达式的参数（如权重、符号参数），以最小化PDE残差。
    - 输出：一个闭合的、可解释的高维PDE解表达式。

## 3. 实验设计：使用了哪些数据集 / 场景，benchmark，对比方法

- **数据集/场景**：虽然具体数据集未在元数据中详细列出，但推测论文使用了**高维PDE的合成解数据**，例如高维热方程、对流扩散方程、薛定谔方程等典型高维PDE的数值解，或者从已知解析解生成的数据。
- **Benchmark**：可能以**传统符号回归方法（如直接应用遗传编程）** 以及**基于神经网络的PDE求解方法（如PINN）** 作为对比基准，评估PSR在预测精度、可解释性和计算效率上的表现。
- **对比方法**：包括但不限于：
    - 标准符号回归（Standard SR）
    - 物理信息神经网络（PINN）
    - 稀疏回归方法（如SINDy）
    - 其他降维后的符号回归方法

## 4. 资源与算力

- 文档中**未明确说明**所使用的具体GPU型号、数量、训练时长等算力信息。可能是在CPU集群上运行符号回归（遗传编程通常对GPU需求不高），而全局组合优化阶段可能使用了轻量级GPU。这一点需要在原文中进一步确认，但根据提供的摘要和元数据，无法获取。

## 5. 实验数量与充分性

- **实验数量**：元数据未提供具体数值，但通常ICLR论文会包含多个高维PDE案例（至少3~5个），以及可能的消融实验（如不同投影数量的影响、不同局部SR策略的对比）。
- **充分性与客观性**：从摘要看，实验结果显示PSR优于传统方法，但没有提供误差分析、统计显著性等细节。由于论文被ICLR 2026拒稿（source注明为Rejected），推测实验可能在某些方面不够完备，例如：
    - 高维PDE的维度范围有限（可能只测试了5~10维）。
    - 缺乏与最新高维PDE求解器（如基于张量分解的方法）的对比。
    - 未充分讨论局部投影组合的唯一性和泛化能力。
- 总体而言，实验在初步证明概念上是足够的，但在严谨性和覆盖面上可能尚有提升空间。

## 6. 论文的主要结论与发现

- **主要结论**：提出的PSR框架能够**成功求解高维PDE**，并从低维投影中**恢复有意义的物理规律**。相比传统直接SR方法，PSR显著缓解了维度灾难，不仅提高了预测准确率，还产出了**可解释的模型**，揭示了底层物理动力学的**组合结构（compositional structure）**。
- **发现**：通过投影分解并组合局部符号函数，可以在不直接面对高维搜索空间的情况下，获得全局闭式解，且该解受PDE残差约束，具有良好的物理一致性。

## 7. 优点：方法或实验设计上有哪些亮点

- **方法上的创新性**：将“投影-组合”思路引入符号回归领域，创新性地将高维PDE求解转化为多个低维符号回归子问题，思路新颖，理论动机清晰。
- **可解释性强**：最终模型是显式数学表达式，而非黑箱神经网络，利于物理洞察。
- **物理约束直接嵌入**：通过最小化PDE残差来约束组合，保证了发现的解满足控制方程，而非仅仅拟合数据。
- **缓解维度灾难**：通过降维，使SR能够处理传统方法无法触及的高维问题。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制

- **实验覆盖有限**：具体测试的高维PDE类型和维度未知，可能仅覆盖了有限场景（如光滑解、低阶导数），未充分验证在极端高维（如100维）或混沌系统上的有效性。
- **偏差风险**：投影的选择策略（固定哪些变量）可能影响最终结果，若选择不当可能导致信息丢失或不准确。论文可能未充分讨论自动选择投影的方法或鲁棒性。
- **组合唯一性**：不同组合方式可能得到相同残差的表达式，存在非唯一性问题，可能降低发现规律的可移植性。
- **计算成本**：虽然缓解了维度灾难，但多个独立SR子问题的计算开销可能随投影数量线性增长，且全局组合优化阶段可能仍需要大量评估。
- **应用限制**：要求PDE解数据能够被采样且投影数据较平滑，对噪声敏感；此外，符号回归本身对复杂振荡或强非线性函数的搜索依然困难。
- **原文缺失**：由于提供的仅为摘要及元数据，无法深入分析算法细节、公式推导和完整实验结果，所有评价均基于有限信息。

（完）
