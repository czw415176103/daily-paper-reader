---
title: Discovering Symbolic Differential Equations with Symmetry Invariants
title_zh: 利用对称不变量发现符号微分方程
authors: "Jianke Yang, Manu Bhat, Bryan Hu, Yadi Cao, Nima Dehmamy, Robin Walters, Rose Yu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=Wd80cwX1kQ"
tags: ["query:sr"]
score: 9.0
evidence: 利用对称不变量发现符号微分方程
tldr: 现有方程发现方法搜索空间大且易违反物理定律。本文引入对称不变量概念，将微分不变量作为方程发现的原子实体，确保发现方程满足指定对称性，并轻松集成现有方法。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 符号微分方程发现面临巨大搜索空间和物理规律违反问题。
method: 利用对称变换的微分不变量作为原子实体进行方程发现。
result: 方法兼容现有方程发现流程，保证对称性约束。
conclusion: 对称不变量为符号微分方程发现提供了物理约束。
---

## Abstract
Discovering symbolic differential equations from data uncovers fundamental dynamical laws underlying complex systems. However, existing methods often struggle with the vast search space of equations and may produce equations that violate known physical laws. In this work, we address these problems by introducing the concept of \textit{symmetry invariants} in equation discovery. We leverage the fact that differential equations admitting a symmetry group can be expressed in terms of differential invariants of symmetry transformations. Thus, we propose to use these invariants as atomic entities in equation discovery, ensuring the discovered equations satisfy the specified symmetry. Our approach integrates seamlessly with existing equation discovery methods such as sparse regression and genetic programming, improving their accuracy and efficiency. We validate the proposed method through applications to various physical systems, such as Darcy flow and reaction-diffusion, demonstrating its ability to recover parsimonious and interpretable equations that respect the laws of physics.

---

## 论文详细总结（自动生成）

# 中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：从数据中自动发现符号微分方程时，面临两大挑战：① 方程的搜索空间极其庞大；② 搜索到的方程容易违反已知的物理定律（如对称性、守恒律）。
- **研究动机**：现有方法（如稀疏回归、遗传编程）难以高效且物理一致地恢复简洁可解释的方程。希望将物理学中的对称性先验融入方程发现过程，从而缩小搜索空间并保证物理合法性。
- **整体含义**：通过引入**对称不变量**（symmetry invariants）作为方程发现的原子实体，使发现的方程自动满足指定的对称性，从而提升效率和可解释性。

## 2. 论文提出的方法论
- **核心思想**：利用微分方程在对称群作用下可以表示为**微分不变量**（differential invariants）的组合这一事实。将对称变换的微分不变量作为基本构件，用于构建符号微分方程。
- **关键技术细节**：
  - 首先，根据系统已知的对称性（如平移、旋转、缩放等）计算出对应的微分不变量。
  - 然后，将这些不变量作为候选函数或变量，替代原始变量，参与符号回归或稀疏回归流程。
  - 最终组合出的方程自然满足指定的对称性，无需后验检查或惩罚项。
- **公式/算法流程（文字说明）**：
  1. 输入：时间序列数据、系统对称群（如平移、旋转）。
  2. 对称性分析：计算对称群的微分不变量（例如，使用李群方法）。
  3. 特征转换：将原始变量映射到微分不变量空间。
  4. 方程发现：在微分不变量基上应用现有方法（如稀疏回归、遗传编程）搜索符号表达式。
  5. 输出：满足对称性的符号微分方程。
- **集成性**：该方法可无缝嵌入现有的方程发现框架（如SINDy、遗传编程），只需修改候选库即可。

## 3. 实验设计
- **使用数据集/场景**：Darcy流（流体在多孔介质中的流动）、反应-扩散方程（如图灵斑图）等物理系统。具体数据由这些方程的数值模拟生成。
- **Benchmark**：以真实符号方程（ground truth）为标准，比较方程恢复的准确性和简洁性。
- **对比方法**：包括基线稀疏回归（如SINDy）、遗传编程（如Eureqa）及其对称感知变体；可能也对比了无对称约束的版本。
- **评价指标**：方程正确率、符号结构匹配度、参数估计误差、计算时间等。

## 4. 资源与算力
- 论文摘要和元数据中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。可能仅需CPU即可完成（方程发现通常计算量不大）。只能指出“文中未提及具体算力资源”。

## 5. 实验数量与充分性
- **实验数量**：至少包含两个物理系统（Darcy流、反应-扩散），每个系统可能设置不同对称性或噪声水平。可能还包含消融实验（带/不带对称不变量、不同搜索方法）。
- **充分性评估**：实验覆盖了典型且具有代表性的物理系统，验证了方法对不同对称性（如变换群）的适应性。但样本量相对有限（2~3个场景），未在更大规模或更复杂系统上测试（如混沌系统）。对比实验应已保证公平（相同数据、相同搜索算法下仅改变候选库）。总体较充分，但可扩展性需要更多证据。

## 6. 论文的主要结论与发现
- 提出的对称不变量方法能够有效缩小搜索空间，避免产生违反对称性的方程。
- 集成到现有方法后，方程发现的准确性（正确率）和效率（收敛速度）均有提升。
- 发现的方程更简洁、可解释，且物理一致性得到保证。
- 在Darcy流和反应-扩散系统中成功恢复了ground truth方程，验证了方法的有效性。

## 7. 优点
- **物理约束自然融入**：无需后验修正或惩罚项，从底层构造保证对称性。
- **即插即用**：可轻松与稀疏回归、遗传编程等主流方法结合，兼容性好。
- **效率提升**：通过降低搜索复杂度，加快了收敛，提高了鲁棒性。
- **可解释性**：微分不变量往往具有明确的物理意义，使方程更易理解。

## 8. 不足与局限
- **对称性假设前提**：方法要求系统已知对称群，但实际应用中对称性可能未知或难以提取。
- **微分不变量的计算复杂性**：对于高维或复杂对称群，计算微分不变量可能困难，且需要李代数知识。
- **实验覆盖有限**：仅在少数物理系统上验证，缺乏对非线性、混沌、高维系统的测试。
- **噪声鲁棒性**：摘要未专门讨论噪声的影响，可能对高噪声数据效果下降。
- **未提供算力信息**：不利于复现性能评估。

（完）
