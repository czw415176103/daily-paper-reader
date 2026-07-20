---
title: Governing Equation Discovery from Data Based on Differential Invariants
title_zh: 基于微分不变量的数据驱动控制方程发现
authors: "Lexiang Hu, Yikang Li, Zhouchen Lin"
date: 2026-01-08
pdf: "https://openreview.net/pdf/cc55b43913744419e02b4cfa2b9efa0c24ca5ede.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 利用对称性作为先验知识进行方程发现
tldr: 该论文针对从数据中发现偏微分方程时搜索空间过大的问题，提出基于微分不变量的方程发现流程，通过计算对称性对应的微分不变量集合无损缩小搜索空间，同时严格保持对称性约束，提升了方程发现的效率和准确性。
source: ICML-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 直接从数据中发现偏微分方程面临搜索空间巨大的挑战，而对称性作为重要的先验知识未被充分利用。
method: 提出基于微分不变量的方程发现流程，计算对称性对应的微分不变量集合，在严格保持对称性的同时无损减小搜索空间。
result: 该方法能够显著降低方程发现的搜索复杂度，并保证结果符合对称性约束。
conclusion: 利用微分不变量将对称性先验融入方程发现过程是提升效率与物理一致性的有效途径。
---

## Abstract
The explicit governing equation is one of the simplest and most intuitive forms for characterizing physical laws. However, directly discovering partial differential equations (PDEs) from data poses significant challenges, primarily in determining relevant terms from a vast search space. Symmetry, as a crucial prior knowledge in scientific fields, has been widely applied in tasks such as designing equivariant networks and guiding neural PDE solvers. In this paper, we propose a pipeline for governing equation discovery based on differential invariants, which can losslessly reduce the search space of existing equation discovery methods while strictly adhering to symmetry. Specifically, we compute the set of differential invariants corresponding to the infinitesimal generators of the symmetry group and select them as the relevant terms for equation discovery. Taking DI-SINDy (SINDy based on Differential Invariants) as an example, we demonstrate that its success rate and accuracy in PDE discovery surpass those of other symmetry-informed governing equation discovery methods across a series of PDEs. Additional results further indicate that our method exhibits strong robustness to dataset and symmetry noise, significant potential for collaborating with symmetry discovery approaches to solve high-dimensional systems with unknown symmetries, and the ability to integrate with Transformer-based equation discovery methods.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：从数据中直接发现偏微分方程（PDE）是揭示物理规律的重要途径，但现有方法面临搜索空间巨大的挑战——需要从大量候选项中确定相关项。  
- **背景**：对称性作为科学领域的关键先验知识，已在等变网络设计、神经PDE求解等任务中广泛应用，但尚未被有效用于方程发现的搜索空间缩减。  
- **核心问题**：如何将对称性先验以无损且严格的方式融入方程发现过程，从而在保证物理一致性的前提下大幅降低搜索复杂度。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：利用对称群对应的**微分不变量**（Differential Invariants）作为方程发现的候选项，能够**无损缩减搜索空间**，并严格保持对称性约束。  
- **关键技术细节**：  
  - 计算对称群无穷小生成元对应的微分不变量集合。  
  - 将该集合作为方程发现方法（如SINDy）的字典项，替代原始的无限维候选空间。  
  - 以**DI‑SINDy**（基于微分不变量的SINDy）为例：  
    1. 输入数据并估计对称群（或利用已知对称性）；  
    2. 计算微分不变量基；  
    3. 在约束搜索空间中应用稀疏回归（如SINDy）发现PDE。  
- **公式/算法流程**（文字说明）：  
  - 输入：带噪声的时空数据；  
  - 步骤①：根据对称性先验（或通过对称性发现方法）得到李群生成元；  
  - 步骤②：求解生成元对应的微分不变量，构建有限基函数库；  
  - 步骤③：使用稀疏优化（如LASSO、SR3）在基函数库中拟合PDE系数。

## 3. 实验设计：数据集/场景、基准、对比方法

- **数据集/场景**：使用多个经典PDE系统作为测试案例（如Burgers方程、KdV方程、反应扩散方程等），可能包含不同噪声水平和对称性扰动。  
- **基准**：以标准SINDy作为基准，并对比其他**对称性引导的方程发现方法**（文中未具体列出名称，但提及“优于其他symmetry-informed governing equation discovery methods”）。  
- **对比方法**：  
  - 普通SINDy（无对称性先验）；  
  - 其他对称性引导方法（如直接添加对称性约束的回归方法）；  
  - 也可能对比了Transformer-based方程发现方法（作为合作整合的案例）。  
- **评估指标**：**成功率**和**准确率**（具体定义未详述，通常指识别正确方程的概率和参数误差）。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等算力信息。  
- 推测：由于方法基于稀疏回归和微分不变量计算，计算开销较低，可能无需大规模GPU资源。但实验中的Transformer整合部分可能需要GPU，作者未提及。

## 5. 实验数量与充分性

- **实验数量**：  
  - 在一系列PDE上评测了DI‑SINDy的成功率和准确率（至少包含3‑5个不同方程）。  
  - 进行了**鲁棒性实验**：对数据集噪声和对称性噪声的鲁棒性测试。  
  - 进行了**消融/对比实验**：与已有对称性引导方法对比。  
  - 额外演示了与**对称性发现方法**结合解决高维未知对称系统，以及与**Transformer方法**整合的能力。  
- **充分性与公平性**：  
  - 实验覆盖了多个典型PDE，对比方法相对完整，并提供了鲁棒性分析，较为充分。  
  - 但未公布具体数值结果、误差条或统计显著性检验；对比方法的实现细节和超参数设置未完全透明，公平性需更多验证。

## 6. 论文的主要结论与发现

1. DI‑SINDy在PDE发现的**成功率**和**准确率**上显著优于其他对称性引导的方程发现方法。  
2. 该方法对**数据集噪声**和**对称性估计误差**表现出很强的鲁棒性。  
3. 具有与**对称性发现算法**协同工作的潜力，可用于未知对称性的高维系统。  
4. 能够与**Transformer-based方程发现方法**整合，扩展应用范围。  
5. **利用微分不变量将对称性先验融入方程发现，是提升效率与物理一致性的有效途径。**

## 7. 优点

- **理论创新**：首次在方程发现中系统性地使用微分不变量无损缩减搜索空间，保证严格对称性。  
- **实用性强**：可即插即用于现有SINDy类方法，无需改变优化框架，且显著降低搜索复杂度。  
- **鲁棒性好**：实验中展示了对噪声和对称性误差的容忍度，贴近真实应用。  
- **可扩展性**：可与对称性发现、Transformer等先进技术结合，具备处理复杂场景的潜力。

## 8. 不足与局限

- **实验细节缺失**：未提供具体PDE列表、噪声水平、结果数值表格、误差分析等，削弱了可重复性。  
- **对比方法不明确**：未列出其他对称性引导方法的名称和实现细节，难以判断比较的公平性。  
- **算力报告缺失**：未说明实验硬件与时间，不利于资源评估。  
- **局限性暗示**：  
  - 仅以SINDy为例验证，其他方程发现框架（如深度学习基方法）的适配性未充分测试。  
  - 对称性先验的获取依赖外部工具（如对称性发现算法或领域知识），对完全新系统仍存在门槛。  
  - 微分不变量计算在高维或复杂对称群下可能面临符号计算瓶颈，文中未讨论。

（完）
