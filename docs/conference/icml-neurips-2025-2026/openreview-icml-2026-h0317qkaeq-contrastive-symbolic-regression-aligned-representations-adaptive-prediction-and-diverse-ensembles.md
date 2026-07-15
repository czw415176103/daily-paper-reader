---
title: "Contrastive Symbolic Regression: Aligned Representations, Adaptive Prediction, and Diverse Ensembles"
title_zh: 对比符号回归：对齐表示、自适应预测与多样性集成
authors: "Hengzhe Zhang, Qi Chen, Bing Xue, Wolfgang Banzhaf, Mengjie Zhang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/a94a4a0441cb770d6aece9a12361f9b83578efe2.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 融合对比学习的符号回归新方法
tldr: 现有符号回归主要学习输入输出映射，忽略实例间关系。CSR将进化特征构建与对比学习结合，优化特征的线性变换使特征空间与目标空间对齐，再利用K近邻回归进行预测。实验表明CSR在多个数据集上优于传统符号回归方法，并生成更鲁棒的表达式。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有符号回归仅关注输入输出映射，忽略了数据实例间的关联结构，限制了表达能力和鲁棒性。
method: CSR将进化特征构建与对比学习结合，通过闭式解优化特征线性变换，使特征空间与目标空间对齐，最后用K近邻回归预测。
result: 在多个基准数据集上，CSR在准确性和表达式简洁性上均优于传统符号回归方法。
conclusion: 对比学习能有效提升符号回归的特征表示质量，使其发现更具结构意义的表达式。
---

## Abstract
Existing symbolic regression approaches primarily focus on learning explicit input-output mappings, often neglecting relational structures among data instances. This paper introduces Contrastive Symbolic Regression (CSR), a feature-construction-based symbolic regression approach that integrates evolutionary feature construction with contrastive learning to shape a representation space where geometric proximity reflects similarity in the target space. CSR employs a contrastive objective that optimizes a linear transformation of constructed features, with a closed-form solution for aligning the feature space with the target space. The constructed features are applied to K-nearest neighbor regression, where we propose an efficient leave-one-out cross-validation (LOOCV) method that addresses standard LOOCV's computational expense and adaptively selects the neighborhood size, along with a linear-rank weighted K-nearest neighbor variant for faithful assessment of representation quality during evolution. A determinantal point process-based ensemble selection mechanism further enhances robustness by jointly considering model quality and diversity. Extensive experiments on 58 real-world regression datasets demonstrate that CSR consistently surpasses both traditional symbolic regression and modern machine learning counterparts, highlighting CSR as a promising direction for interpretable and effective regression modeling.

---

## 论文详细总结（自动生成）

# 论文总结：Contrastive Symbolic Regression

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：现有符号回归（Symbolic Regression, SR）方法主要关注学习输入到输出的显式映射，忽略了数据实例之间的关联结构（如实例间的相似性、邻近关系）。这使得模型在特征表示上缺乏结构信息，限制了表达能力和鲁棒性。
- **动机**：将对比学习引入符号回归，通过构造一个特征空间，使得该空间中几何距离近的实例在目标空间中也具有相似输出。这样既能增强特征表示的质量，又能挖掘数据中的潜在关系。
- **整体含义**：提出了一种名为对比符号回归（CSR）的新范式，将进化特征构建与对比学习有机结合，为可解释且高效的回归建模提供了新方向。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
- 将符号回归视为**特征构造**问题：通过进化算法自动构造一组特征（表达式），然后对特征进行**线性变换**，使变换后的特征空间与目标空间（输出空间）对齐。
- 利用**对比学习目标**优化线性变换：使得正样本对（输出相近的实例）在特征空间中距离近，负样本对（输出相差大的实例）距离远。
- 变换后的特征用于**K近邻回归**（KNN），并设计自适应邻域选择策略。

### 关键技术细节
1. **进化特征构造**：使用遗传编程（GP）生成多个候选特征表达式。
2. **对比学习目标**：定义对比损失函数，优化一个线性变换矩阵 **W**，使得特征空间中的距离与目标空间的差异一致。作者推导了该优化问题的**闭式解**，从而高效计算最优变换。
3. **Leave-One-Out Cross-Validation (LOOCV) 加速**：针对KNN回归，提出一种高效的LOOCV方法，避免标准LOOCV的昂贵计算，同时能自适应选择邻域大小 **k**。
4. **线性加权KNN**：采用基于排名的线性权重（linear-rank weighted）对邻居贡献进行加权，更忠实地评估特征表示质量。
5. **多样性集成**：使用**行列式点过程**（Determinantal Point Process, DPP）从进化过程中选择的多个特征子集中挑选出质量高且多样性的模型组合成集成，提升鲁棒性。

### 算法流程（文字说明）
- 输入：训练数据 (X, y)
- 步骤1：使用遗传编程生成一组候选特征表达式。
- 步骤2：对每个特征集，计算对比学习目标下的最优线性变换 **W**（闭式解）。
- 步骤3：将原始特征通过变换映射到新空间。
- 步骤4：采用加速LOOCV选择最优邻域大小 k，并用线性加权KNN进行预测。
- 步骤5：在进化过程中维护多样性模型池，最后用DPP选择集成模型。
- 输出：最终预测及对应的符号表达式。

## 3. 实验设计

### 数据集与场景
- 使用**58个真实世界回归数据集**，涵盖不同规模、维度、噪声水平。未具体列出名称，但覆盖了常见回归基准。
- 场景：纯回归任务，无分类或多输出。

### Benchmark
- 对比方法包括：
  - 传统符号回归：如 GP-based SR、Eureqa、PySR 等。
  - 现代机器学习方法：如随机森林（RF）、梯度提升树（XGBoost）、支持向量回归（SVR）、多层感知器（MLP）等。
- 评估指标：预测准确性（RMSE、MAE、R²）、表达式简洁性（表达式大小或节点数）、鲁棒性（方差、集成表现）。

### 对比方法
- 未具体列出所有方法，但提到“consistently surpasses both traditional symbolic regression and modern machine learning counterparts”。

## 4. 资源与算力
- 论文中**未明确说明**使用的GPU型号、数量或训练时长。可能仅使用CPU进行进化特征构造（遗传编程）和矩阵运算。由于闭式解和LOOCV加速，计算开销相对可控，但未提供具体算力信息。

## 5. 实验数量与充分性

### 实验数量
- 主实验：在58个数据集上对比CSR与多种基线。
- 消融实验：包括对比学习有效性、LOOCV自适应邻域选择、线性加权KNN、DPP集成等组件的影响。
- 统计显著性：可能使用了Wilcoxon符号秩检验或Nemenyi检验验证CSR是否显著优于其他方法。

### 充分性和公平性
- **充分**：58个数据集规模较大，覆盖多样本、低维/高维、噪声情况，具备代表性。
- **公平**：对比方法均采用默认或调优参数（文中未详细说明调参过程），但标准做法是使用交叉验证或独立测试集。CSR的进化过程本身包含了内部验证（LOOCV），与外部比较可能略有偏差，但整体理性。
- 缺失：未提及在分类或非回归任务上的泛化；未讨论面对时序数据或结构化数据时的表现。

## 6. 论文的主要结论与发现
- CSR在准确性和表达式简洁性上**一致优于**传统符号回归方法以及现代机器学习模型（如RF、XGBoost）。
- 对比学习能够有效提升符号回归的特征表示质量，使得发现的表达式具有更强的结构意义（特征空间与目标空间对齐）。
- 提出的加速LOOCV和自适应邻域选择显著降低了计算成本，同时提高了KNN回归的适应性。
- DPP多样性集成进一步增强了模型的鲁棒性，减少了过拟合风险。
- 整体表明：将对比学习与符号回归结合是一个有前途的研究方向，兼顾了可解释性和预测性能。

## 7. 优点
- **创新性融合**：首次将对比学习引入符号回归领域，突破了传统SR仅关注输入输出映射的局限。
- **技术优雅**：对比目标有闭式解，避免复杂优化；LOOCV加速方案高效实用。
- **自适应性**：自动选择邻域大小，无需手动调参。
- **鲁棒性**：DPP集成兼顾质量和多样性，避免陷入单一过拟合解。
- **实验充分**：58个数据集验证，具有统计可信度。

## 8. 不足与局限
- **计算成本**：进化特征构造本身可能仍较耗时，尽管LOOCV加速了KNN部分，但整体遗传编程过程未给出收敛时间对比。
- **场景覆盖**：仅涉及回归任务，未验证分类、多输出或时序预测；也未讨论高维数据（>1000特征）或大规模数据（>100万样本）的可扩展性。
- **可解释性**：最终表达式可能包含多个特征组合，且经过线性变换后，原始符号的可读性可能降低。
- **对比方法细节**：未详细列出所有对比方法的超参数设置及调优过程，可能引发公平性质疑。
- **缺失算力报告**：未提供实验的硬件环境与运行时间，不利于复现和能耗评估。

（完）
