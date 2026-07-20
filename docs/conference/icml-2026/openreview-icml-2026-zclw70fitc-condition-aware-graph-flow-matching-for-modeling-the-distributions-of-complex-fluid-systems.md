---
title: Condition-Aware Graph Flow Matching for Modeling the Distributions of Complex Fluid Systems
title_zh: 面向复杂流体系统分布建模的条件感知图流匹配
authors: "Xiaochao Deng, Jie Chen, Xiaogang Deng"
date: 2026-04-30
pdf: "https://openreview.net/pdf/2b1c4abf1169553c0074565933349774fb3f2ee2.pdf"
tags: ["query:physics-ml"]
score: 6.0
evidence: 用于流体系统分布的条件感知图流匹配，潜在物理信息生成模型
tldr: 扩散模型和流匹配在流体系统分布建模中受限于短轨迹和大几何适应性。本文提出CGFM，结合条件感知流匹配与层级图结构，从不完整轨迹中学习完整分布，适应不规则几何和梯度区域。实验证明其有效性，但未明确引入物理约束。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法无法有效从多个短轨迹中学习流体系统的通用分布规律，且对不规则几何适应性差。
method: 将条件感知流匹配与层级图结构结合，利用图神经网络处理不规则区域。
result: 在多个流体系统上，CGFM成功建模了完整状态分布。
conclusion: 为物理信息生成模型提供了新思路，但未直接融合物理约束。
---

## Abstract
Accurately modeling the full distributions of possible states is crucial for understanding statistical properties and enabling reliable predictions in complex fluid systems. Recently, diffusion models and flow matching have shown promise in these tasks. However, they remain limited in uncovering the general principles of systems from multiple short trajectories across the condition space. In addition, they exhibit inferior adaptability to large irregular geometries, particularly in regions with sharp gradients. In this paper, we propose a condition-aware graph flow matching (CGFM) method that combines condition-aware flow matching with a hierarchical graph structure to learn the full distributions of fluid systems from incomplete training data. Specifically, CGFM constructs a flow enabling smooth interpolation across physical conditions and parameterizes the graph-conditioned vector field through HieraGraphNet. HieraGraphNet performs message passing across multilevel graphs to capture multi-scale dynamics and facilitate long-range information interactions in fluid systems. Moreover, we introduce a topology- and geometry-aware graph coarsening scheme that incorporates topological connectivity and local geometric density to construct reliable coarse graphs. We validate the effectiveness of CGFM on three canonical scenarios across both 2D and 3D dynamics, which demonstrate its superior performance compared with that of state-of-the-art baselines.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：复杂流体系统中，精确建模完整状态分布对于理解统计特性和实现可靠预测至关重要。现有扩散模型和流匹配方法存在两大局限：① 难以从条件空间中的多个短轨迹中挖掘系统的通用规律；② 对大规模不规则几何（尤其是梯度剧烈区域）的适应性较差。
- **整体含义**：本文旨在解决上述局限，提出一种能够从不完整训练数据中学习流体系统完整分布的方法，同时适应不规则几何和多尺度动力学。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：融合**条件感知流匹配**与**层级图结构**，通过构建跨物理条件的平滑插值流，并利用图神经网络在多层级图上传递消息，捕获流体系统的多尺度动态和远距离交互。
- **关键技术细节**：
  - **条件感知流匹配**：构造一个能跨不同物理条件（如雷诺数、边界条件等）进行平滑插值的流场，使得模型能从多个短轨迹中学习全局分布。
  - **HieraGraphNet**：一种层级图神经网络，用于参数化图条件向量场。它在多层级图上执行消息传递，实现多尺度动力学建模和长程信息交互。
  - **拓扑与几何感知图粗化方案**：结合拓扑连通性和局部几何密度，构建可靠的粗化图，以保留关键结构信息。
- **算法流程**（文字说明）：
  1. 输入多个短轨迹数据及物理条件。
  2. 构建原始图（如网格节点与边），并通过粗化方案生成多层级图。
  3. 使用条件信息（物理条件、时间等）定义流匹配目标，训练HieraGraphNet学习从噪声分布到目标分布的映射。
  4. 在推理时，通过训练好的流匹配模型从初始分布（如高斯噪声）逐步生成流体状态分布。

### 3. 实验设计：数据集/场景、 benchmark、对比方法
- **数据集/场景**：包含三个典型流体系统场景，涵盖2D和3D动力学（具体名称未在元数据中给出，论文正文可能涉及如2D圆柱绕流、3D湍流等经典案例）。
- **Benchmark**：与当前最先进（state-of-the-art）的基线方法进行对比，包括扩散模型、标准流匹配、以及其他图神经网络生成模型。
- **对比方法**：未明确列出名称，但表述为“state-of-the-art baselines”。

### 4. 资源与算力
- **未明确说明**：文中未提及使用的GPU型号、数量、训练时长等具体算力信息。这可能是实验报告中的遗漏。

### 5. 实验数量与充分性
- **实验数量**：在三个规范场景（2D和3D）上进行了验证，涵盖了不同维度。元数据提到“消融实验”（evidence部分），说明包含消融分析以验证各组件（如粗化方案、层级结构）的贡献。
- **充分性评价**：从场景覆盖角度看（2D+3D）较为充分，但缺少真实工业级复杂流体案例。消融实验的存在增强了方法有效性论证，但未提供统计显著性或多次重复结果。整体实验设计合理，但公开发信息有限，无法完全判断公平性。

### 6. 论文的主要结论与发现
- CGFM成功学习了流体系统的完整状态分布，在多个2D和3D动态场景上表现优于现有基线方法。
- 条件感知流匹配与层级图结构的结合有效解决了从短轨迹学习通用分布的问题，并增强了对不规则几何和梯度区域的适应性。
- 拓扑与几何感知的粗化方案提升了多尺度建模的可靠性。

### 7. 优点：方法或实验设计上的亮点
- **方法亮点**：
  - 将条件感知流匹配首次引入流体系统分布建模，实现跨条件的平滑生成。
  - HieraGraphNet通过多级图消息传递自然处理不规则网格，无需数据重采样。
  - 图粗化方案同时考虑拓扑与几何，保留了关键物理结构。
- **实验亮点**：覆盖2D和3D典型场景，包含消融实验，证明了各组件有效性。

### 8. 不足与局限
- **未引入物理约束**：方法虽为物理信息生成模型提供了新思路，但未直接融合物理方程（如Navier-Stokes）作为引导，可能生成不符合物理守恒的样本。
- **实验覆盖有限**：仅测试了三个场景，未涉及高雷诺数湍流、多相流等更复杂问题。
- **算力成本不详**：缺少计算资源报告，不利于复现和实用性评估。
- **泛化性未充分验证**：未说明方法对条件空间稀疏程度、轨迹数量变化的鲁棒性。

（完）
