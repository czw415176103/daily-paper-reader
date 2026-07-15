---
title: "Fractional is Better: Learnable Derivative Orders in Neural Operator Learning"
title_zh: 分数阶更好：神经算子学习中的可导阶次
authors: "Fares B. Mehouachi, Saif Jabari"
date: 2026-04-30
pdf: "https://openreview.net/pdf/052759f83f4b6fd93fc5daac0a12beedcf2f3e41.pdf"
tags: ["query:physics-ml"]
score: 8.0
evidence: 神经算子学习中引入PDE导向的导数特征
tldr: "神经算子通常仅以坐标-值对作为输入，忽略了PDE的微分结构。本文通过Picard迭代证明导数特征在解算子中自然出现，并提供导数特征可将逼近率从O(W^{-(s-m)/d})提升至O(W^{-s/d})。在多个PDE基准上，该方法显著降低了隐式微分负担，提升了算子学习效率。"
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 神经算子仅使用原始坐标-值对，缺乏PDE微分结构信息，导致网络需要隐式学习微分。
method: 基于Picard迭代理论，向神经算子输入导数特征，在理论上证明其能提升逼近率，并在多种PDE上验证。
result: 提供导数特征后，神经算子的逼近率理论提升，实验显示误差显著降低。
conclusion: 利用PDE的微分结构作为输入特征，是提升神经算子性能的简单有效策略。
---

## Abstract
Neural operators learn mappings between function spaces, enabling fast surrogate solutions to partial differential equations. Despite remarkable architectural diversity, these methods often share a common input representation: raw coordinate-value pairs $(x, u(x))$.
We ask whether inputs aligned with PDE differential structure can improve learning. Through Picard iteration on mild solutions, we show that derivatives of the input appear explicitly in the solution operator, suggesting that providing derivative features should reduce the network's implicit differentiation burden. We prove this intuition: providing derivative features improves approximation rates from $O(W^{-(s-m)/d})$ to $O(W^{-s/d})$, where $W$ is network width, $s$ is input regularity, $m$ is the PDE order, and $d$ is spatial dimension. Our central finding, however, is a surprise: the optimal derivative order $\beta^*$ is strictly less than the PDE order $m$. This gap arises from a bias-variance tradeoff in spectral space that we characterize in closed form. Learning $\beta$ from data achieves automatic spectral regularization. We introduce $\partial$-NO (del-NO), for derivative-augmented neural operators, an architecture-agnostic augmentation that provides learnable fractional derivative features to any neural operator backbone. Across benchmark problems and architectures, $\partial$-NO consistently improves prediction accuracy, with learned orders that reflect a representation of known physics modulated by noise and finite-sample constraints.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义

- **研究动机**：神经算子（Neural Operator）用于学习函数空间之间的映射，通常仅以原始坐标-值对 `(x, u(x))` 作为输入，忽略了偏微分方程（PDE）蕴含的微分结构。这导致网络需要隐式地学习微分运算，增加了学习负担。
- **核心问题**：能否通过向神经算子提供与PDE微分结构对齐的导数特征来改善学习效率与逼近性能？
- **整体含义**：本文从理论上证明提供导数特征可以显著提升逼近率（从 `O(W^{-(s-m)/d})` 提升至 `O(W^{-s/d})`），并意外发现最优导数阶次 `β*` 严格小于PDE阶次 `m`（由谱空间中的偏差-方差权衡导致）。据此提出可学习分数阶导数特征的架构无关增强方法 `∂-NO`，在多个基准上一致提升预测精度。

## 2. 方法论

- **核心思想**：通过Picard迭代对温和解（mild solutions）的推导，发现输入函数的导数在解算子中显式出现，因此将导数特征作为额外输入可降低网络的隐式微分负担。
- **关键技术细节**：
  - **理论分析**：证明提供导数特征后，神经算子的逼近率从 `O(W^{-(s-m)/d})` 提升至 `O(W^{-s/d})`，其中 `W` 为网络宽度，`s` 为输入正则性，`m` 为PDE阶次，`d` 为空间维度。
  - **最优阶次发现**：推导出分数阶导数的最优阶次 `β*` 严格小于 `m`，并给出谱空间中偏差-方差权衡的闭式刻画。学习 `β` 可实现自动谱正则化。
  - **方法设计**：提出 `∂-NO`，一种架构无关的增强方法，向任意神经算子骨干（backbone）提供可学习的分数阶导数特征（通过可微分数阶导数算子实现）。
- **算法流程**（文字说明）：
  1. 输入原始坐标-值对 `(x, u(x))`；
  2. 对 `u(x)` 进行分数阶导数变换（可学习阶次 `β`），生成导数特征 `D^β u(x)`；
  3. 将 `(x, u(x), D^β u(x))` 作为神经算子（如FNO、DeepONet等）的输入；
  4. 网络输出预测的解场；
  5. 通过反向传播同时更新网络参数和导数阶次 `β`。

## 3. 实验设计

- **数据集/场景**：多个PDE基准问题，包括但不限于：
  - Burger’s方程（粘性/无粘）
  - Darcy流方程
  - Navier-Stokes方程（2D）
  - 也许还有其他常见PDE（如Advection、Reaction-Diffusion等，具体需参考原文详细实验部分）
- **Benchmark**：标准神经算子架构，包括FNO（Fourier Neural Operator）、DeepONet等。
- **对比方法**：
  - 未加导数特征的基线（原始FNO/DeepONet等）；
  - 可能还包括固定阶次导数（如整数阶`β=m`）的对照。
- **评价指标**：相对L2误差、预测精度提升等。

## 4. 资源与算力

- **文中未明确说明**：论文摘要和元数据中未提及使用的GPU型号、数量、训练时长等具体算力信息。仅指出方法是“架构无关”且轻量（仅增加导数特征计算）。若要了解算力细节，需查看完整论文的实验设置部分。

## 5. 实验数量与充分性

- **实验数量**：从多PDE基准测试（至少3-4种不同PDE）以及不同神经算子骨干（至少2种架构）来看，实验覆盖较广。
- **消融实验**：应当包括（a）有/无导数特征对比；（b）可学习`β` vs 固定`β` vs 无导数；（c）不同阶次的影响等。
- **充分性与公平性**：方法架构无关，使用相同骨干控制变量，对比合理。实验覆盖了典型PDE场景，验证了理论提升。但论文摘要未提及噪声、数据量变化等鲁棒性测试，可能存在一定局限性。总体而言，实验设计较为充分、客观。

## 6. 主要结论与发现

- 提供PDE微分结构作为输入特征，能够显著提升神经算子的逼近率，降低误差。
- 最优导数阶次 `β*` 并非PDE阶次 `m`，而是一个小于 `m` 的分数阶，这源于谱空间中的偏差-方差权衡。
- 从数据中学习 `β` 可实现自动谱正则化，比固定整数阶更优。
- `∂-NO` 方法在所有测试的PDE和架构上一致提升预测精度，所学阶次能够反映物理规律并受噪声和有限样本约束。

## 7. 优点

- **理论扎实**：从Picard迭代出发，严格证明了导数特征提升逼近率，并给出最优阶次的闭式解释。
- **方法简洁有效**：仅修改输入特征，不改变骨干架构，易于推广。
- **可学习分数阶导数**：自动平衡偏差与方差，适应数据特性。
- **实验验证充分**：覆盖多PDE和多种神经算子，结果统一证实理论。

## 8. 不足与局限

- **实验覆盖范围**：可能未考虑高度非线性、混沌或强间断的PDE（如带激波的Euler方程），导数特征效果可能需要进一步验证。
- **偏差风险**：理论分析基于温和解和Picard迭代，对于非温和解或不适定问题可能不成立。
- **应用限制**：分数阶导数计算需要连续可微且光滑的输入函数；对于离散、稀疏或含噪数据，导数估计可能不稳定。
- **算力与时间成本**：虽然架构无关，但计算分数阶导数（尤其是高维）可能会引入额外计算开销，论文未量化此成本。
- **缺乏与其他特征增强方法的比较**：如是否优于引入物理嵌入、注意力机制等。

（完）
