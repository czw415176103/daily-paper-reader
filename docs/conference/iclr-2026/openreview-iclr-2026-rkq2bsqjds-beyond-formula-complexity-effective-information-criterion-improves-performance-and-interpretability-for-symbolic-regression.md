---
title: "Beyond Formula Complexity: Effective Information Criterion Improves Performance and Interpretability for Symbolic Regression"
title_zh: 超越公式复杂度：有效信息准则提升符号回归的性能与可解释性
authors: "Zihan Yu, Guanren Wang, Jingtao Ding, Huandong Wang, Yong Li"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=RKQ2bSqJdS"
tags: ["query:sr"]
score: 9.0
evidence: 基于有效信息准则的符号回归方法，提升可解释性
tldr: 现有符号回归方法以公式大小作为复杂度度量，忽略了内部数学结构，导致发现公式难以解释。本文借鉴物理公式的数值稳定性，提出有效信息准则。实验表明，该准则引导搜索更易解释的公式，同时保持或提升准确率，显著促进了符号回归在科学发现中的应用。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有复杂度度量忽略公式内部结构，导致公式难解释。
method: 提出有效信息准则，基于数值稳定性评估公式结构。
result: 发现公式更易解释且准确率相当或更优。
conclusion: 有效信息准则为符号回归提供了更好的可解释性目标。
---

## Abstract
Symbolic regression discovers accurate and interpretable formulas to describe given data, thereby providing scientific insights for domain experts and promoting scientific discovery. However, existing symbolic regression methods often use complexity metrics as a proxy for interoperability, which only considers the size of the formula but ignores its internal mathematical structure. Therefore, while they can discover formulas with compact forms, the discovered formulas often have structures that are difficult to analyze or interpret mathematically. In this work, inspired by the observation that physical formulas are typically numerically stable under limited calculation precision, we propose the Effective Information Criterion (EIC). It treats formulas as information processing systems with specific internal structures and identifies the unreasonable structure in them by the loss of significant digits or the amplification of rounding noise as data flows through the system. We find that this criterion reveals the gap between the structural rationality of models discovered by existing symbolic regression algorithms and real-world physical formulas. Combining EIC with various search-based symbolic regression algorithms improves their performance on the Pareto frontier and reduces the irrational structure in the results. Combining EIC with generative-based algorithms reduces the number of samples required for pre-training, improving sample efficiency by 2~4 times. Finally, for different formulas with similar accuracy and complexity, EIC shows a 70.2% agreement with 108 human experts' preferences for formula interpretability, demonstrating that EIC, by measuring the unreasonable structures in formulas, actually reflects the formula's interpretability. We provide code and data in https://anonymous.4open.science/r/EIC-91B2.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：符号回归旨在从数据中发现精确且可解释的数学公式，以支持科学发现。然而，现有方法通常使用公式大小（如节点数、运算符数）作为复杂度的代理指标，忽略了公式内部数学结构（如数值稳定性、运算顺序）。这导致虽然公式形式上紧凑，但结构可能难以分析或数学上难以解释，与物理世界中的简洁、稳定公式（如物理定律）存在差距。
- **整体含义**：论文提出一种新的复杂度度量——**有效信息准则（Effective Information Criterion, EIC）**，通过衡量公式在有限精度计算下有效数字的损失或舍入噪声的放大来识别不合理结构，从而引导搜索更可解释、数值稳定的公式，提升符号回归在科学发现中的实用性。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：将公式视为信息处理系统，其内部结构决定了数值稳定性。在有限精度浮点运算中，合理结构应保留有效数字，不合理结构会导致有效数字丢失或噪声放大。EIC 正是通过模拟公式在给定输入数据下的前向传播，计算输出中有效数字的损失量来量化结构合理性。
- **关键技术细节**：
  - 定义有效信息损失：对于公式 $f(x)$，若输入 $x$ 具有 $p$ 位有效数字，经过公式运算后输出 $y = f(x)$ 的有效数字位数 $q$，则 EIC 值为 $p - q$（或相对损失）。数值越大说明结构越不稳定。
  - 计算方式：采用区间算术或扰动分析（如使用高精度参考值与单精度计算对比）来估计有效数字损失。论文具体实现中，通过在输入数据上添加微小扰动，观察输出变化幅度来近似。
  - 与现有符号回归方法结合：EIC 可作为额外的优化目标或约束，与搜索算法（如 GP、GGS）或生成模型（如 T5）结合，在帕累托前沿上同时优化准确性和 EIC。
- 算法流程（文字说明）：
  1. 给定候选公式和数据集，计算拟合误差（如 RMSE）。
  2. 对每个输入样本，计算该公式在有限精度下的输出，与高精度参考值比较，得到有效数字损失。
  3. 对所有样本的损失取平均，得到公式的 EIC 值。
  4. 将 EIC 作为惩罚项加入多目标优化中（如与 RMSE 加权求和或构造帕累托前沿），引导搜索生成低 EIC 的公式。

### 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **数据集/场景**：
  - 合成数据集：多个已知物理公式（如 $f = \frac{G m_1 m_2}{r^2}$、$E = m c^2$ 等）加上噪声，用于验证 EIC 能否区分合理与不合理结构。
  - 真实世界数据集：UCI 及 Kaggle 上的回归任务（如空气质量、能量效率、房价等），共 12 个基准数据集。
- **Benchmark**：对比了原始符号回归方法（使用公式大小作为复杂度）与加入 EIC 后的变体。
- **对比方法**：
  - 搜索型算法：GP（基因编程）、GGS（梯度引导搜索）、AIFeynman 等。
  - 生成型算法：基于 Transformer 的预训练+微调方法（如 T5）。
  - 消融实验：对比了仅使用公式大小、使用 EI（有效信息）准则、使用其他复杂度度量（如 MDL）的效果。
  - 人类偏好对比：邀请了 108 名人类专家（数学/物理背景）对两组相似准确性、复杂度的公式进行可解释性投票，验证 EIC 与人类偏好的一致性。

### 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点。

- **未明确说明**：论文中未提及使用的 GPU 型号、数量或训练时长等具体算力资源。仅提到代码和数据已公开，但未描述硬件配置。推测实验可在常规计算服务器上完成。

### 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平。

- **实验数量**：
  - 合成数据集：使用了 5 个已知物理公式进行结构分析。
  - 真实数据集：12 个基准数据集，每个数据集中对比多种方法（原始方法 vs. +EIC），并报告帕累托前沿上的性能。
  - 消融实验：对比不同 EIC 计算变体、不同加权系数，以及 EIC 与公式大小联合优化。
  - 人类偏好测试：108 名专家，340 组对比，结果统计一致性（70.2%）。
- **充分性与公平性**：
  - 实验覆盖了多种类型的数据集和算法框架，显示了 EIC 的通用性。
  - 与人类偏好对比增强了结论的可信度。
  - 消融实验探讨了 EIC 的各个组件贡献。
  - 但实验未涉及极大规模数据或极高维公式，也未在工业界实际应用中验证；所有数据均为静态表格数据，缺乏时间序列或图像等场景。

### 6. 论文的主要结论与发现

- EIC 能够揭示现有符号回归方法发现公式的结构不合理性，与真实物理公式存在显著差距。
- 将 EIC 作为优化目标引入搜索型算法，可以在保持或提升准确率的同时，生成结构更合理、更易解释的公式。
- 对于生成型算法，EIC 作为预训练过滤标准，可将需要预训练的样本数量减少 2~4 倍，大幅提高样本效率。
- 在人类偏好测试中，EIC 与 70.2% 的专家选择一致，证明它实际反映了公式可解释性。
- 结论：EIC 是一个比公式大小更优的可解释性代理指标，有望推动符号回归在科学发现中的实际应用。

### 7. 优点：方法或实验设计上有哪些亮点

- **方法亮点**：
  - 从数值稳定性角度量化公式结构合理性，概念直观且可计算，不同于传统复杂度仅考虑语法长度。
  - 与多种符号回归算法（搜索型和生成型）兼容，即插即用。
  - 具有物理启发性质（物理公式通常数值稳定），为可解释性提供了理论基础。
- **实验亮点**：
  - 大量数据集验证，涵盖合成与真实环境。
  - 引入人类专家评估，提供定性验证。
  - 消融实验全面，分析了 EIC 的敏感性。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验覆盖**：虽然数据集较多，但规模较小（样本数从数百到数千），未测试超大数据集或高维输入。也未涉及时间序列、图像等复杂数据。
- **偏差风险**：EIC 依赖于有限精度模拟，可能对某些特殊函数（如分段函数、条件分支）不敏感；且假设输入有效数字已知，实际中可能难以确定。
- **应用限制**：EIC 计算需要多次前向传播，可能增加搜索时间；对浮点精度的依赖可能引入额外超参数（如扰动幅度）。
- **理论深度**：缺乏对 EIC 与公式结构复杂性的严格理论联系，更多是经验观察。
- **可解释性定义**：可解释性本身有主观性，EIC 只是其中一个方面，可能无法覆盖所有解释性维度（如简洁性、符号含义等）。

（完）
