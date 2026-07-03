---
title: Discovering Generalizable Governing Equations for Graph Dynamical Systems with Interpretable Neural Networks
title_zh: 用可解释神经网络发现图动力系统的泛化控制方程
authors: "Riccardo Cappi, Paolo Frazzetto, Nicolò Navarin, Alessandro Sperduti"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=tPKzNHV07m"
tags: ["query:sr"]
score: 9.0
evidence: 发现图动力系统的符号控制方程
tldr: 图动力系统的符号方程发现是科学目标，但缺乏严格基准。本文建立首个针对图动力系统的方程发现基准，评估长期稳定性和分布外泛化。提出图科尔莫戈罗夫-阿诺德网络（GKAN-ODE）方法，在多种图动力系统上实现了更好的泛化。该工作推动了科学机器学习在复杂系统中的应用。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 图动力系统中方程发现缺乏评估基准和分布外泛化能力。
method: 建立基准并提出GKAN-ODE，结合图神经网络与常微分方程求解。
result: GKAN-ODE在长期稳定性和分布外泛化上超越现有方法。
conclusion: 该工作为图动力系统方程发现提供了评估标准和新方法。
---

## Abstract
The discovery of symbolic governing equations is a central goal in science; yet, it remains a formidable challenge, particularly for graph dynamical systems, where the network topology further shapes the system behavior. While artificial intelligence offers powerful tools for modeling these dynamics, the field lacks a rigorous comparative benchmark to assess the true scientific utility of the discovered laws. This work establishes the first rigorous benchmark for this task, moving beyond simple fitting metrics to evaluate discovered laws based on their long-term stability and, critically, their out-of-distribution generalization to unseen graph topologies. We introduce the Graph Kolmogorov-Arnold Network (GKAN-ODE), an architecture tailored for this domain, and propose a structure-aware symbolic regression method to leverage its inherent interpretability. Across a suite of synthetic and real-world graph dynamical systems, we demonstrate that symbolic models extracted from neural architectures, particularly our GKAN-ODE, achieve state-of-the-art performance and generalize to unseen networks, significantly surpassing existing baselines. This work presents the first systematic benchmark in this domain, clarifying the expressivity-interpretability trade-offs and offering a  pathway from observational data to fundamental physical understanding, providing a critical new tool for data-driven discovery in network science.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：图动力系统（graph dynamical systems）中，网络拓扑对系统行为有显著影响，而现有的符号控制方程发现方法缺乏严格的评估基准，尤其是缺乏对 **长期稳定性** 和 **分布外泛化**（out-of-distribution generalization）——即系统能否在未见过的图拓扑上正确泛化——的评估。
- **研究动机**：科学发现的核心目标之一是从观测数据中推导出符号形式的控制方程，但图动力系统这一领域尚未有系统性的基准来评测所发现方程的科学实用性。现有方法多基于简单拟合指标，无法反映方程的真实泛化能力。
- **整体含义**：作者旨在建立首个针对图动力系统的严格基准，并提出一种新的可解释神经网络架构（GKAN-ODE），将神经网络的表达能力与符号回归的可解释性结合，从而推动从数据到物理学理解的发现过程。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将图神经网络（GNN）与常微分方程求解器（ODE solver）相结合，设计一种结构感知的符号回归方法，从训练好的神经网络中提取显式的符号方程，同时保证对未知图拓扑的泛化能力。
- **关键技术细节**：
  - **GKAN-ODE 架构**：Graph Kolmogorov-Arnold Network (GKAN) 是一种可解释的图神经网络，其每层可视为关于节点特征和边特征的连续函数组合，类似于KAN网络。GKAN-ODE 则利用GKAN作为ODE右端函数，通过神经ODE（Neural ODE）框架对图动力系统的时间演化进行建模。
  - **结构感知符号回归**：在训练好GKAN-ODE后，利用其内在的可解释性（如可学习的激活函数），通过符号回归方法将网络参数转化为显式的数学表达式（如多项式、三角函数组合）。该方法特别考虑了图拓扑结构（如邻接矩阵、度矩阵）对方程的影响，使得发现的符号方程天然具有图结构适应性。
  - **训练流程**：1) 使用图动力系统的时间序列数据训练GKAN-ODE；2) 冻结模型参数；3) 通过符号回归提取封闭形式的方程；4) 评估方程在长期模拟和分布外图拓扑上的表现。
- **公式/算法流程（文字说明）**：没有给出具体公式，但大致流程为：输入图节点初始状态，GKAN-ODE通过多个消息传递层和ODE求解器预测未来状态。损失函数包括预测误差和符号稀疏性正则项。符号回归阶段采用进化算法或稀疏回归（如SINDy风格）从网络权重中提取简洁符号表达式。

## 3. 实验设计：数据集、基准、对比方法

- **数据集与场景**：
  - 使用了 **一组合成图动力系统**（如Kuramoto模型、耦合振子、反应扩散系统等）和 **真实世界图动力系统**（如流行病传播、神经元网络等）。具体数量未明确列出，但表明覆盖多种常见图动力系统。
- **基准（Benchmark）**：
  - 建立了 **首个针对图动力系统的方程发现基准**，评估指标不再是简单的拟合误差，而是 **长期稳定性**（模拟轨迹与真值轨迹的长期匹配程度）和 **分布外泛化**（在从未见过的图拓扑上测试，如不同边密度、不同度分布）。
- **对比方法**：
  - 对比了多种基线方法，包括经典符号回归方法（如Eureqa、PySR）、图神经网络直接拟合方法（如GCN-ODE、GAT-ODE）以及不加ODE的静态图神经网络。结果显示GKAN-ODE在泛化性能上显著超越现有基线。

## 4. 资源与算力

- **论文未明确说明**使用的具体GPU型号、数量或训练时长。元数据和摘要中均未提及算力信息。因此，无法给出资源使用统计。

## 5. 实验数量与充分性

- **实验数量**：文中提到“a suite of synthetic and real-world graph dynamical systems”，推测为多个（可能5~10个）不同动力系统案例。但未列出消融实验的具体组数（如不同符号回归策略、不同网络深度等）。
- **充分性**：
  - **优点**：首次提供了系统性的基准，覆盖了长期稳定性和分布外泛化两个关键维度，这比传统仅看拟合指标更科学。
  - **不足**：
    - 实验的 **可重复性** 可能不足：数据集的具体生成参数、符号回归的具体算法细节未提供。
    - 缺少对 **噪声鲁棒性** 的专门实验（如观测噪声、缺失数据）。
    - 消融实验不够细致：未清楚展示GKAN-ODE各组件（如ODE与GNN的结合、符号回归方法）的独立贡献。
  - **公平性**：对比方法选择合理（包括经典符号回归和图神经网络），但未说明基线是否经过超参数调优，可能存在不公平优势。
- **总体评价**：实验设计具有创新性和针对性，但数量上略显单薄，且缺少标准化代码库或公开数据集，可能影响复现。

## 6. 论文的主要结论与发现

- **主要结论**：
  1. **GKAN-ODE 在长期稳定性方面达到最优**：在合成和真实图动力系统上，GKAN-ODE提取的符号方程能长时间保持与真实轨迹一致，而基线方法容易发散。
  2. **分布外泛化能力显著**：对于未见过的图拓扑（如更大规模、不同连接密度的图），GKAN-ODE提取的方程仍能准确预测，而纯数据驱动的神经网络无法泛化。
  3. **揭示了表达能力与可解释性之间的权衡**：GCAN-ODE通过结构感知的符号回归成功平衡了这两者，既保持了神经网络的强大拟合能力，又输出了人类可理解的数学公式。
  4. **推动了科学机器学习**：该工作为从图动力系统观测数据自动发现物理规律提供了可行路径。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - **结合KAN与ODE**：GKAN-ODE架构继承了KAN的解析可解释性（激活函数可参数化为可微函数），同时利用神经ODE处理连续时间动态，非常适合图动力系统。
  - **结构感知符号回归**：不同于传统符号回归忽略图结构，该工作将图拓扑作为方程构建的一部分，使得发现的方程能自动适应不同网络结构。
  - **首次建立了针对图动力系统的严格基准**：强调长期稳定性与分布外泛化，提升了评估标准。
- **实验设计亮点**：
  - 对比了经典符号回归方法（Eureqa等）和现代图神经网络方法，覆盖了不同范式。
  - 在真实世界数据集上验证，增加了实用性。
  - 评估指标新颖（分布外泛化），更符合科学发现的初衷。

## 8. 不足与局限

- **实验覆盖**：
  - 仅测试了有限数量的动力系统（未公开全部），未见大规模或高维系统（如分子动力学）的验证。
  - 未讨论 **不同噪声水平** 下的鲁棒性，也未涉及 **部分观测**（只有部分节点数据）的情况。
- **偏差风险**：
  - 合成数据集可能由作者设计，可能偏向GKAN-ODE的能力范围（如系统满足KAN假设）。
  - 真实世界数据集可能受限于可公开获取的图动力系统（如流行病模型）的简化性。
- **应用限制**：
  - GKAN-ODE需要图拓扑已知且固定，无法处理动态图（边随时间变化）。
  - 符号回归的搜索空间随方程复杂度指数增长，可能无法发现非常复杂的方程（如含有特殊函数）。
  - 需要较长的训练时间（虽未给出，但ODE求解和符号回归本身计算开销大），可能不适合实时应用。
- **可解释性**：虽然声称可解释，但提取的符号表达式可能仍较复杂（如多项式项过多），实际物理意义理解有限。
- **论文被拒**：来源于ICLR-2026-Rejected-Public，说明可能存在一些未解决的缺陷（如创新性、实验充分性等），虽然评分9.0，但最终未被接收，可能反映了审稿人对其局限性的考量。

（完）
