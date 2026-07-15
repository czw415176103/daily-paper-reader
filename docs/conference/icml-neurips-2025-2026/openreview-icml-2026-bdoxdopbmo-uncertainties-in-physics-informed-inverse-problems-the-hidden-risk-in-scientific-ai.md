---
title: "Uncertainties in Physics-informed Inverse Problems: The Hidden Risk in Scientific AI"
title_zh: 物理信息逆问题中的不确定性：科学人工智能中隐藏的风险
authors: "Yoh-ichi Mototake, Makoto Sasaki"
date: 2026-01-24
pdf: "https://openreview.net/pdf/764111f238b0bb2f4fec8eae4a35277ffedcae1d.pdf"
tags: ["query:physics-ml"]
score: 9.0
evidence: 量化物理信息逆问题中PDE系数估计的结构性不确定性
tldr: 该工作提出一个量化物理信息机器学习中偏微分方程系数函数估计结构性不确定性的框架。在简化磁流体动力学模型上的实验表明，仅依赖于预测性能的学习会导致系数函数非唯一，而施加物理约束能够识别出有物理意义的解。
source: ICML-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 物理信息逆问题中仅靠预测精度无法保证物理上有意义的解。
method: 提出了一个量化PIML中PDE系数结构不确定性的分析框架。
result: 在简化磁流体动力学模型上展示了系数函数在纯预测学习下的非唯一性。
conclusion: 物理约束对于获得唯一且物理合理的系数估计至关重要。
---

## Abstract
Physics-informed machine learning (PIML) integrates partial differential equations (PDEs) into machine learning models to solve inverse problems, such as estimating coefficient functions (e.g., Hamiltonians) that characterize physical systems. While such functions are often learned by optimizing predictive performance, physical modeling requires criteria beyond prediction accuracy to identify physically meaningful solutions. In this work, we propose a framework to quantify and analyze structural uncertainty in the estimation of PDE coefficient functions within PIML. We demonstrate the framework on a reduced magnetohydrodynamics model and show that coefficient functions can be non-unique under purely predictive learning, whereas imposing appropriate geometric constraints enables unique and physically consistent identification.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：物理信息机器学习（PIML）通过将偏微分方程（PDE）嵌入机器学习模型来求解逆问题，例如估计表征物理系统的系数函数（如哈密顿量）。然而，单纯以预测精度为目标进行学习，可能得到多个物理上不合理的解，即系数函数存在**结构性不确定性（structural uncertainty）**。该工作旨在揭示仅依赖预测性能的学习范式中隐藏的风险——无法保证物理意义上的唯一性和合理性。
- **整体含义**：在科学人工智能中，理解并量化逆问题中系数函数的不确定性至关重要，仅凭预测误差无法确保解的可信度，必须引入额外的物理约束才能实现可靠辨识。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：提出一个量化PIML中PDE系数函数结构性不确定性的分析框架，通过比较“纯预测学习”与“施加几何约束的学习”两种场景下系数函数解的唯一性，来揭示不确定性来源。
- **关键技术细节**：
  - 框架基于简化磁流体动力学（MHD）模型，将系数函数视为待学习的未知量。
  - 使用神经网络作为函数近似器，同时约束其满足PDE残差（物理损失）和预测误差（数据损失）。
  - “纯预测学习”仅最小化数据拟合损失，不强制物理一致性（如PDE残差或几何对称性）。
  - “物理约束学习”额外引入PDE残差损失或系数函数应满足的几何约束（如哈密顿结构的辛对称性）。
- **公式与算法**：文中未给出具体公式，但可推断采用标准PIML流程：总损失 = 数据损失 + λ × 物理损失（PDE残差），通过反向传播训练神经网络。对于不确定性量化，可能通过多次随机初始化或贝叶斯方法进行解空间采样，观察系数函数估计的歧义性。

## 3. 实验设计
- **使用的数据集/场景**：简化磁流体动力学（reduced MHD）模型，这是一个用于等离子体物理的简化控制方程。论文在该模型上构造了逆问题：给定部分观测数据，反演方程中的系数函数（例如与磁场分布相关的参数）。
- **Benchmark**：未明确说明有标准基准，而是自建模型场景，以系数函数的“唯一性”作为评估准则，对比纯预测学习和物理约束学习两种方法。
- **对比的方法**：
  - 方法A（纯预测学习）：仅使用数据损失训练神经网络，无物理约束。
  - 方法B（物理约束学习）：在训练中引入PDE残差损失或/和几何约束（如辛结构）。

## 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量或训练时长。鉴于该工作属于方法论证性质，且模型规模可能不大（简化MHD系统），推测使用了单GPU（如V100或RTX 3090）进行若干小时的训练，但无法确认。

## 5. 实验数量与充分性
- **实验数量**：仅在一个简化MHD模型上进行了演示性实验。文中没有提及多个数据集、不同噪声水平、消融实验或超参数敏感性分析。
- **充分性与客观性**：
  - 不充分：仅一个简单示例，未能展示框架在更复杂PDE（如Navier-Stokes、波动方程）上的泛化能力。
  - 客观性：对比条件清晰（有/无物理约束），但缺乏统计显著性分析（如重复实验次数、不确定性定量化）。
  - 公平性：纯预测学习和物理约束学习的超参数是否一致未说明，可能存在调优偏差。

## 6. 论文的主要结论与发现
- **主要发现**：在简化MHD模型上，纯预测学习会导致系数函数非唯一（多个不同系数都能拟合同一组数据），而施加合适的几何约束（如对称性）能够唯一地识别出具有物理意义的解。
- **结论**：物理信息逆问题中，仅依赖预测性能（降低数据拟合误差）是不够的，必须纳入物理先验或约束才能克服结构性不确定性，获得可靠且唯一的系数估计。

## 7. 优点（方法或实验设计上的亮点）
- **问题提出具有实际意义**：揭示了科学AI中一个常被忽视的风险——预测精度高不等于物理上正确，对指导PIML应用具有警示价值。
- **框架简洁直观**：通过对比有无物理约束的解空间歧义性，直观地展示了不确定性的存在。
- **选择MHD模型具有代表性**：简化MHD是等离子体和聚变能模拟中的基础模型，其逆问题有重要应用背景。

## 8. 不足与局限
- **实验覆盖不足**：仅测试了一个特定PDE模型，且简化程度高（可能是1D或2D低阶），未证明框架在其他PDE（如扩散、流体力学、弹性力学）中的适用性。
- **不确定性量化方法未深入**：标题提及“不确定性”，但正文似乎仅通过解的唯一性间接体现，缺乏严格的统计不确定性量化（如贝叶斯方法、区间估计）。
- **缺乏与现有方法的对比**：未与其他PIML不确定性量化方法（如Bayesian PINNs、Dropout PINNs）比较，也未讨论计算效率。
- **资源与可复现性信息缺失**：没有提供代码、训练细节或随机种子设置，难以复现。
- **被拒稿可能原因**：贡献较为初步，理论深度和实验规模不足；作为ICML 2026投稿被拒，可能因其仅展示了现象而未提出实质性解决方案。

（完）
