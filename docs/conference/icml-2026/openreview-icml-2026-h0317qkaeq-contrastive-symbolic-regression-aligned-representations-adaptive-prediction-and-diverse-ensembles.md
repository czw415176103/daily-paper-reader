---
title: "Contrastive Symbolic Regression: Aligned Representations, Adaptive Prediction, and Diverse Ensembles"
title_zh: 对比符号回归：对齐表示、自适应预测与多样集成
authors: "Hengzhe Zhang, Qi Chen, Bing Xue, Wolfgang Banzhaf, Mengjie Zhang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/a94a4a0441cb770d6aece9a12361f9b83578efe2.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 对比学习用于符号回归
tldr: 本文提出对比符号回归（CSR），将进化特征构造与对比学习相结合，构建一个几何接近度反映目标相似度的表示空间。通过闭式解对齐特征空间与目标空间，并将构造特征用于K近邻回归。在多个回归数据集上，CSR展现了高预测精度和多样化表达式，克服了传统符号回归忽略实例间关系结构的局限。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有符号回归方法只关注输入-输出映射，忽略了数据实例间的结构关系。
method: 结合进化特征构造和对比学习，通过闭式解对齐特征空间与目标空间，采用KNN回归进行预测。
result: 在多种回归任务中取得了更高的预测精度，并生成了更多样化的表达式。
conclusion: 对比学习能有效提升符号回归的表示质量和泛化能力。
---

## Abstract
Existing symbolic regression approaches primarily focus on learning explicit input-output mappings, often neglecting relational structures among data instances. This paper introduces Contrastive Symbolic Regression (CSR), a feature-construction-based symbolic regression approach that integrates evolutionary feature construction with contrastive learning to shape a representation space where geometric proximity reflects similarity in the target space. CSR employs a contrastive objective that optimizes a linear transformation of constructed features, with a closed-form solution for aligning the feature space with the target space. The constructed features are applied to K-nearest neighbor regression, where we propose an efficient leave-one-out cross-validation (LOOCV) method that addresses standard LOOCV's computational expense and adaptively selects the neighborhood size, along with a linear-rank weighted K-nearest neighbor variant for faithful assessment of representation quality during evolution. A determinantal point process-based ensemble selection mechanism further enhances robustness by jointly considering model quality and diversity. Extensive experiments on 58 real-world regression datasets demonstrate that CSR consistently surpasses both traditional symbolic regression and modern machine learning counterparts, highlighting CSR as a promising direction for interpretable and effective regression modeling.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现有符号回归方法主要关注学习显式的输入-输出映射关系，忽略了数据实例之间的结构关系（relational structures），导致表示空间缺乏对目标空间相似性的刻画。
- **核心问题**：如何让符号回归在学习过程中显式建模实例间的相似性结构，从而提升表示质量、预测精度和表达式的多样性。
- **整体含义**：提出了一种名为**对比符号回归（Contrastive Symbolic Regression, CSR）** 的新方法，将进化特征构造与对比学习相结合，构造出的特征空间能够使几何邻近度反映目标相似度，进而用K近邻回归进行预测，实现可解释且高精度的回归建模。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：通过对比学习引导进化特征构造过程，使构造出的特征空间具有目标空间相似性的结构；再利用闭式解对齐特征空间与目标空间，最后在重构特征上应用K近邻回归进行预测。
- **关键技术细节**：
  - **对比目标**：优化构造特征的线性变换，使得几何邻近度与目标相似度一致。
  - **闭式解对齐**：通过线性变换的解析解，将特征空间与目标空间对齐，避免繁琐的迭代优化。
  - **K近邻回归**：提出一种高效的留一交叉验证（LOOCV）方法，解决标准LOOCV计算开销大的问题，并自适应选择邻域大小；还设计了线性排名加权的K近邻变体，用于进化过程中对表示质量的忠实评估。
  - **集成选择**：基于行列式点过程（Determinantal Point Process, DPP）的集成选择机制，同时考虑模型质量和多样性，增强鲁棒性。
- **算法流程**（文字描述）：
  1. 初始化种群，每个个体表示一组构造特征（通过符号表达式组合原始输入）。
  2. 对每个个体，利用对比损失优化特征空间的线性变换（闭式解）。
  3. 使用改进的LOOCV方法评估构造特征的K近邻回归性能，并自适应选择邻域大小。
  4. 进化过程：通过选择、交叉、变异生成新个体，并重复评估。
  5. 最终从进化获得的个体中，依据DPP选择多样且有效的模型组成集成。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **数据集**：在58个真实世界回归数据集上进行了广泛实验（覆盖多种领域和规模）。
- **Benchmark**：对比了传统符号回归方法（如GP-based SR）以及现代机器学习方法（如随机森林、梯度提升、神经网络等）。
- **对比方法**：文中提到“consistently surpasses both traditional symbolic regression and modern machine learning counterparts”，推测对比了至少两类十余种方法，具体名称未在abstract中列出。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点。

- **未明确说明**：提供的摘要和元数据中没有提及具体的GPU型号、数量或训练时长。因此无法总结算力信息。推测论文全文可能包含实验环境描述，但当前内容不足以支撑回答。

## 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平。

- **实验数量**：58个真实数据集，覆盖广泛，是较大的基准规模。
- **消融实验**：摘要中提到了“efficient leave-one-out cross-validation”和“DPP-based ensemble”等组件，推测进行了消融研究验证各部分贡献，但未明确数量。
- **充分性评价**：58个数据集上的对比实验较为充分，数据集多样性高，结果可信度较高。但未提及合成数据或噪声测试，可能在某些极端情况下泛化性仍需验证。总体上看，实验设计较客观，对比方法种类全面。

## 6. 论文的主要结论与发现

- **主要结论**：CSR在58个真实回归数据集上一致优于传统符号回归和现代机器学习方法。
- **发现**：
  - 对比学习能有效提升符号回归的表示质量和泛化能力。
  - 闭式解对齐特征空间与目标空间具有高效性。
  - 改进的LOOCV方法与自适应邻域选择提升了预测精度。
  - DPP集成选择机制在模型质量和多样性之间取得了良好平衡，增强了鲁棒性。
- **意义**：CSR为可解释且有效的回归建模开辟了新方向。

## 7. 优点：方法或实验设计上有哪些亮点

- **方法创新**：
  - 首次将对比学习与进化特征构造结合，显式利用实例间结构关系。
  - 闭式解对齐避免了繁琐的迭代优化，计算高效。
  - 高效的LOOCV方法解决了经典LOOCV的计算瓶颈，并自适应选择邻域。
  - DPP集成选择同时兼顾多样性和质量，提升集成鲁棒性。
- **实验亮点**：
  - 覆盖58个真实数据集，规模大、领域广，具有代表性。
  - 与多种传统和现代方法对比，比较充分。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验覆盖**：尽管有58个数据集，但缺乏对合成数据、高维数据或噪声场景的专门分析，可能遗漏特定条件下的性能退化。
- **偏差风险**：对比学习中正负样本的构造方式可能引入偏差，若目标空间相似性定义不当，可能影响表示对齐效果。
- **应用限制**：K近邻回归的预测结果可解释性弱于直接符号表达式，虽然特征构造是可解释的，但最终预测依赖实例近邻，可能降低可解释性。
- **计算资源**：进化过程与LOOCV结合，虽经过优化，但整体计算复杂度仍可能较高，在大规模数据集上应用时需考虑时间成本。
- **未提及的细节**：闭式解对齐的具体数学形式、对比损失函数细节、超参数敏感性等未在摘要中透露，需阅读全文才能评估其稳健性。

（完）
