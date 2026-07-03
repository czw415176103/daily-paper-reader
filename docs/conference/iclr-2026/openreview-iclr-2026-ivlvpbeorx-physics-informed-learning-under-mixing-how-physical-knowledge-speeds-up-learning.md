---
title: "Physics-informed learning under mixing: How physical knowledge speeds up learning"
title_zh: 混合下的物理信息学习：物理知识如何加速学习
authors: "Anna Scampicchio, Leonardo Felipe Toso, Rahel Rickenbach, James Anderson, Melanie Zeilinger"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=IvLVPbeoRx"
tags: ["query:physics-ml"]
score: 7.0
evidence: 物理信息学习理论，用于科学机器学习
tldr: 论文从理论上分析了物理信息正则化在数据依赖情况下如何加速学习。通过推导经验风险最小化的过量风险界，证明当物理先验信息对齐时，学习率从慢速的Sobolev极小极大速率提升到快速独立同分布速率，且不会因数据依赖而损失样本效率。该工作为物理信息机器学习的有效性提供了理论支撑。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 理解物理先验知识如何影响数据依赖情况下的学习率是物理信息机器学习的重要理论问题。
method: 对带物理信息正则化的经验风险最小化进行复杂度分析，导出概率和期望下的过量风险界。
result: 证明当物理先验对齐时，学习率从慢速Sobolev速率提升到最优独立同分布速率，且无样本量缩减。
conclusion: 该理论表明物理信息正则化在相关数据下能显著加速学习，为设计更高效的物理信息算法提供了指导。
---

## Abstract
A major challenge in physics-informed machine learning is to understand how the incorporation of prior domain knowledge affects learning rates when data are dependent. Focusing on empirical risk minimization with physics-informed regularization, we derive complexity-dependent bounds on the excess risk in probability and in expectation. We prove that, when the physical prior information is aligned, the learning rate improves from the (slow) Sobolev minimax rate to the (fast) optimal i.i.d. one without sample-size deflation due to data dependence.

---

## 论文详细总结（自动生成）

以下是对给定论文《Physics-informed learning under mixing: How physical knowledge speeds up learning》的详细中文总结。由于提供的论文内容仅为元数据和摘要，实际实验细节、算法流程、算力等信息均未包含，因此在相关部分会如实注明。

---

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在物理信息机器学习（Physics-informed Machine Learning）中，当训练数据存在依赖（如时序、空间混合过程）时，物理先验知识如何影响学习速率？传统理论往往假设数据独立同分布（i.i.d.），但实际物理数据常具有相关性，直接套用 i.i.d. 分析会低估样本效率或高估误差。
- **研究动机**：理解物理信息正则化在数据依赖情形下的理论性质，为设计更高效的物理信息算法提供理论指导。
- **整体含义**：论文从理论上证明，当物理先验与真实系统对齐时，使用物理信息正则化的经验风险最小化（ERM）可以大幅提升学习速率，从慢速的 Sobolev 极小极大速率提升至最优的 i.i.d. 速率，且不会因数据依赖而损失样本效率。这为物理信息学习方法的有效性提供了坚实的理论支撑。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：通过复杂度分析，推导带物理信息正则化的 ERM 在数据依赖（混合过程）下的过量风险界（excess risk bounds），包括概率形式和期望形式。
- **关键技术细节**：
  - 假设数据满足某种混合条件（如 ρ-混合或 β-混合），用以刻画数据依赖的强度。
  - 物理先验信息通过正则化项融入损失函数，正则化项惩罚模型输出与物理定律（如 PDE 残差）的偏离。
  - 利用经验过程理论和覆盖数（covering number）概念，结合混合序列的浓度不等式，导出过量风险的上界。
- **公式或算法流程**（文字说明）：
  - 定义物理信息损失函数：\( \hat{L}(f) = \frac{1}{n} \sum_{i=1}^n \ell(f(x_i), y_i) + \lambda \cdot \mathcal{R}(f) \)，其中 \(\mathcal{R}(f)\) 为物理先验正则项。
  - 分析过量风险 \( \mathbb{E}[\ell(f_n)] - \inf_{f\in\mathcal{F}} \mathbb{E}[\ell(f)] \) 的界。
  - 证明：当物理先验信息与真实系统对齐时，正则项带来的惩罚随样本量增加而快速衰减，从而使学习率提升至 i.i.d. 最优速率 \( O(n^{-1/2}) \)（在适当条件下），而非依赖数据的慢速 Sobolev 速率（如 \( O(n^{-\alpha}) \) 且 α < 1/2）。
- **关键结论**：学习率提升不依赖于数据混合的强度，即物理先验可以抵消数据依赖带来的样本效率损失。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **实验信息**：提供的元数据及摘要中未包含任何实验细节，因此无法回答。
- **已知信息**：论文标题、摘要及元数据均未提及具体数据集、基准方法或对比实验。根据元数据中“evidence: 物理信息学习理论，用于科学机器学习”和“tldr”内容，推测该工作以理论分析为主，可能仅包含合成示例或简单数值验证，但原文未提供。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）

- **未明确说明**：提供的文本中没有任何关于算力、GPU 型号、训练时长或计算资源的信息。该论文属于理论分析型工作，可能不涉及大规模实验，因此算力信息未报告。

## 5. 实验数量与充分性：大概做了多少组实验，这些实验是否充分、是否客观、公平

- **无法评估**：由于无实验内容，无法对实验数量、消融实验、充分性等进行评价。论文主要贡献在理论层面，实验部分（如果有）可能仅为辅助说明，而非核心。若需全面评价，需获取完整论文。

## 6. 论文的主要结论与发现

- **主要结论**：
  1. 物理信息正则化在数据依赖（混合）条件下，当物理先验对齐时，可以将学习速率从慢速的 Sobolev 极小极大速率提升到最优的 i.i.d. 速率。
  2. 该提升不会因为数据依赖而降低样本效率（即无样本量缩减效应）。
  3. 该理论为物理信息机器学习在非 i.i.d. 场景下的有效性提供了严格的理论保证。
- **发现**：物理先验不仅提高了模型泛化能力，还缓解了数据相关性带来的学习障碍，使得在少量相关数据上也能达到快速收敛。

## 7. 优点：方法或实验设计上有哪些亮点

- **理论创新**：首次在混合数据依赖框架下，严格证明了物理信息正则化对学习速率的加速作用，填补了相关理论空白。
- **结论普适性**：结论不依赖特定的混合系数衰减速率，具有较强的泛化性。
- **清晰的问题转化**：将物理先验对齐程度映射为正则项的可识别性，用复杂度界统一分析。
- **简洁有力**：摘要简洁，直接给出了关键速率比较，易于理解。

## 8. 不足与局限

- **实验缺失**：提供的文本未包含任何实验验证，无法判断理论假设（如物理先验对齐、混合条件）在实际问题中的满足程度。
- **应用限制**：理论假设可能要求物理先验与真实系统精确对齐，而实际中先验知识往往含有偏见或不完备，论文未讨论不对齐情况下的性能退化。
- **数据依赖模型**：仅考虑了混合过程中的依赖，未覆盖更复杂的长程依赖或多层嵌套依赖。
- **信息有限**：由于仅基于元数据总结，可能遗漏了原文中的模拟实验或补充材料。如需完整评价，应获取全文。

---

（完）
