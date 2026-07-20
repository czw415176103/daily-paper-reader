---
title: "Uncertainties in Physics-informed Inverse Problems: The Hidden Risk in Scientific AI"
title_zh: 物理信息逆问题中的不确定性：科学人工智能的隐藏风险
authors: "Yoh-ichi Mototake, Makoto Sasaki"
date: 2026-01-24
pdf: "https://openreview.net/pdf/764111f238b0bb2f4fec8eae4a35277ffedcae1d.pdf"
tags: ["query:physics-ml"]
score: 7.0
evidence: 量化物理信息逆问题中PDE系数的结构不确定性
tldr: 物理信息机器学习在估计PDE系数时仅优化预测性能可能导致非物理解。本文提出一个框架量化PIML中系数估计的结构不确定性，在简化磁流体动力学模型上展示纯预测学习下系数不唯一，而引入物理约束可缓解该问题，强调物理建模需要超越预测准确性的标准。
source: ICML-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 物理信息机器学习求解逆问题时，纯预测优化可能产生非物理但高精度的解。
method: 提出结构不确定性量化框架，分析PIML中PDE系数估计的非唯一性。
result: 在磁流体动力学模型上证明系数非唯一性，并展示物理约束可稳定估计。
conclusion: 物理信息逆问题需要同时考虑预测准确性和物理合理性，不确定性量化是关键工具。
---

## Abstract
Physics-informed machine learning (PIML) integrates partial differential equations (PDEs) into machine learning models to solve inverse problems, such as estimating coefficient functions (e.g., Hamiltonians) that characterize physical systems. While such functions are often learned by optimizing predictive performance, physical modeling requires criteria beyond prediction accuracy to identify physically meaningful solutions. In this work, we propose a framework to quantify and analyze structural uncertainty in the estimation of PDE coefficient functions within PIML. We demonstrate the framework on a reduced magnetohydrodynamics model and show that coefficient functions can be non-unique under purely predictive learning, whereas imposing appropriate geometric constraints enables unique and physically consistent identification.

---

## 论文详细总结（自动生成）

# 中文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：物理信息机器学习（PIML）将偏微分方程（PDE）嵌入机器学习模型，用于求解逆问题（如估计哈密顿量等表征物理系统的系数函数）。然而，传统方法仅通过优化预测性能（如降低预测误差）来学习这些系数，忽略了物理建模中更关键的物理合理性标准。这可能导致虽然预测精度高，但系数函数却失去物理意义（非物理解），构成科学AI中的隐藏风险。
- **整体含义**：本文指出，在PIML逆问题中，仅关注预测准确性可能带来系数估计的**结构不确定性**（非唯一性），即多个不同的系数函数都能产生同样精准的预测结果，但其中只有部分符合物理约束。因此，需要引入超越预测准确性的准则，并量化这种不确定性，以确保得到的解具有物理一致性。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：提出一个**结构不确定性量化框架**，专门用于分析和量化PIML中PDE系数估计的非唯一性问题。
- **关键技术细节**：
  - 框架将系数函数的不确定性分为**可约不确定性**（可由更多数据/更好模型减少）和**结构不确定性**（由问题本身非适定性导致，即使无限数据也无法消除）。本文聚焦后者。
  - 采用**贝叶斯推断或集成方法**（文中未明确说明具体技术，但提及“量化框架”）来表征系数函数后验分布，从而揭示不同系数组合在给定预测目标下等价的可能性。
  - 通过施加**几何约束**（如对称性、守恒律等物理先验）来缩小可行解空间，使系数函数唯一且物理一致。
- **公式/算法流程**（以文字说明）：
  1. 定义PIML逆问题：给定观测数据，学习PDE中的未知系数函数 \( f(x) \)，使得PDE解与观测匹配。
  2. 标准做法：最小化预测误差 \( \mathcal{L}_{\text{pred}} \) 求解 \( f \)。
  3. 本文扩展：在优化目标中加入物理约束正则项 \( \mathcal{L}_{\text{phys}} \)（如几何不变性、散度条件等）。
  4. 量化不确定性：使用多初始点或贝叶斯采样估计系数后验的多样性，计算系数在参数空间中的“等价集”大小，作为结构不确定性指标。
  5. 验证：若等价集中不同系数均达到相同预测误差，则表明存在结构不确定性。

## 3. 实验设计
- **使用的数据集/场景**：基于一个**简化磁流体动力学（MHD）模型**。该模型描述等离子体行为，具有非线性PDE和多个系数函数。采用合成数据（由已知系数生成模拟观测）进行验证。
- **基准（Benchmark）**：无明确外部基准，主要是自我对比：比较**纯预测学习**（仅优化预测损失）与**带物理约束的学习**（加入几何约束）两种情况下的系数估计结果。
- **对比方法**：未与现有其他PIML方法（如PINN、DeepONet等）进行横向对比，只聚焦于本文提出的框架内两种策略的对比。

## 4. 资源与算力
- 论文未明确说明所使用的GPU型号、数量、训练时长等算力资源。可能实验规模较小（简化MHD模型），仅需要单GPU或CPU即可完成。**不足之处**：缺乏算力报告，不利于其他研究者复现。

## 5. 实验数量与充分性
- **实验组数**：从摘要和元数据看，论文主要在一个简化MHD模型上进行了两组实验：
  - 实验1：纯预测学习，展示系数非唯一性。
  - 实验2：加入几何约束，展示唯一且物理一致的识别。
- **充分性评估**：
  - **不足**：实验仅覆盖一个模型（简化MHD），未在多个不同PDE系统（如Navier-Stokes、热传导、波动方程等）上验证；未进行消融实验分析不同约束的效果；未与多种不确定性量化方法（如Dropout、Ensemble、贝叶斯神经网络等）比较。
  - **客观性**：合成数据环境可控，结论可信但泛化性存疑。实验设计基本公平，但缺少统计显著性检验和鲁棒性测试。

## 6. 论文的主要结论与发现
- **主要结论**：
  1. 物理信息逆问题中，仅通过优化预测性能学习PDE系数，会导致**系数函数非唯一**，即多个不同的系数组合都能达到几乎相同的预测精度，从而产生非物理解。
  2. 通过施加**合适的几何约束**（如磁流体动力学中的散度自由条件等），可以有效消除这种非唯一性，使系数估计唯一且符合物理直觉。
  3. 研究强调：物理建模需要超越预测准确性的标准，**不确定性量化是识别和规避结构不确定性的关键工具**。
- **发现**：结构不确定性是PIML逆问题的固有风险，不能通过增加数据量或模型复杂度完全消除，必须依靠物理先验解决。

## 7. 优点
- **方法论亮点**：
  - 首次明确区分PIML逆问题中系数估计的**结构不确定性**与通常的随机不确定性，并针对前者提出量化框架。
  - 提出使用**几何约束**作为物理正则化手段，思路清晰，易于在现有PIML框架中实现。
- **实验设计亮点**：
  - 选择磁流体动力学模型，该模型具有复杂物理结构，能有效暴露非唯一性问题，案例典型。
  - 可视化系数等价集，直观展示预测精度相同但系数不同的现象，说服力强。
- **理论贡献**：为科学AI领域敲响警钟，促使社区重新审视仅以预测误差作为评价标准的风险。

## 8. 不足与局限
- **实验覆盖不足**：仅在一个简化MHD模型上验证，未推广到其他非线性PDE系统（如混沌系统、生物物理模型等），结果泛化性存疑。
- **方法局限性**：
  - 如何自动发现合适的几何约束？本文依赖领域知识手动施加，缺乏通用性。
  - 框架未提供量化结构不确定性的具体算法（如后验采样方法），实现细节不够透明。
- **偏差风险**：合成数据可能过于理想，未考虑实际观测噪声、数据稀疏性等，结论可能高估了结构不确定性的影响。
- **应用限制**：对于高维PDE或复杂三维模型，计算量化框架的代价可能巨大，本文未讨论可扩展性。
- **算力未报告**：缺少资源信息，影响复现和效率评估。

（完）
