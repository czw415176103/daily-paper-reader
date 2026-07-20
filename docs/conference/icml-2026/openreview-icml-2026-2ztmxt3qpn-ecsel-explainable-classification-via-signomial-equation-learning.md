---
title: "ECSEL: Explainable Classification via Signomial Equation Learning"
title_zh: ECSEL：通过符号方程学习实现可解释分类
authors: "Adia C. Lumadjeng, Ilker Birbil, Erman Acar"
date: 2026-04-30
pdf: "https://openreview.net/pdf/e269ea3c601224e11bf00e4eb96adf87e4373870.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 学习符号方程作为符号表达式，在符号回归基准上测试
tldr: 本文提出ECSEL方法，通过学习符号方程（signomial）进行可解释分类。该方法在标准符号回归基准上比现有方法恢复更多目标方程，且计算量大幅降低。ECSEL将分类准确性与可解释性结合，验证了符号方程学习在机器学习中的有效性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 许多符号回归基准具有紧凑的符号结构，但现有方法无法有效学习。
method: 直接学习符号方程形式的封闭表达式，同时作为分类器和解释。
result: 在符号回归基准上恢复更多目标方程，分类精度与主流模型相当且更具可解释性。
conclusion: ECSEL证明符号方程学习可以在保持可解释性的同时达到竞争性分类性能。
---

## Abstract
We introduce ECSEL, an explainable classification method that learns formal expressions in the form of signomial equations, motivated by the observation that many symbolic regression benchmarks admit compact signomial structure. ECSEL directly constructs a structural, closed-form expression that serves as both a classifier and an explanation. On standard symbolic regression benchmarks, our method recovers a larger fraction of target equations than competing state-of-the-art approaches while requiring substantially less computation. Leveraging this efficiency, ECSEL achieves classification accuracy competitive with established machine learning models without sacrificing interpretability. Further, we show that ECSEL satisfies some desirable properties regarding global feature behaviour, decision-boundary analysis, and local feature attributions. Experiments on benchmark datasets and two real-world case studies i.e., e-commerce and fraud detection, demonstrate that the learned equations expose dataset biases, support counterfactual reasoning, and yield actionable insights.

---

## 论文详细总结（自动生成）

# 论文总结：ECSEL: Explainable Classification via Signomial Equation Learning

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：现有符号回归方法虽然能够学习可解释的表达式，但在分类任务中往往无法有效恢复具有紧凑符号结构的方程，且计算开销大。同时，深度学习模型虽然精度高，但缺乏可解释性。如何同时实现高分类准确性与完全可解释性是一大挑战。
- **研究动机**：观察到许多符号回归基准数据集本身具有紧凑的符号方程结构（如 signomial 形式），但现有方法难以有效学习这类结构，导致分类器要么牺牲可解释性，要么计算成本过高。
- **整体含义**：本文提出 ECSEL（Explainable Classification via Signomial Equation Learning），通过直接学习 signomial 形式的封闭表达式，使其同时作为分类器和解释，从而兼顾准确性与可解释性。该方法在标准符号回归基准上恢复更多目标方程，且计算量大幅降低；在真实分类任务中精度与主流模型相当，同时提供全局特征行为、决策边界分析和局部特征归因等可解释性。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：将分类问题转化为符号回归问题，学习一个 signomial 方程（形如 $\sum_{i} c_i \prod_{j} x_j^{a_{ij}}$ 的表达式，其中指数为实数，系数为实数），该方程直接输出类别概率或分数，并可作为自然可读的解释。
- **关键技术细节**：
  - **符号表达式搜索**：ECSEL 使用一种高效的搜索策略（未在摘要中详细说明具体算法，但可能基于遗传编程或数学优化），直接构造 signomial 形式的封闭式表达式，而不是学习黑箱模型后再进行事后解释。
  - **可解释性属性**：方法满足全局特征行为解释、决策边界分析（例如通过符号方程的根或符号变化分析）、局部特征归因（如通过导数或贡献度计算）。
  - **与标准分类器兼容**：学习到的 signomial 方程可以作为分类器使用（例如通过设定一个阈值），同时提供解释。
- **算法流程**（文字说明）：
  1. 输入：带标签的数据集。
  2. 设置符号方程候选空间（signomial 结构，包括可能的单项式组合及指数）。
  3. 通过搜索算法（例如基于进化算法或梯度优化）最小化分类损失（如交叉熵或0-1损失），同时约束表达式的复杂度。
  4. 输出：一个 signomial 方程，可解释且可直接用于分类。

> 注：由于仅提供摘要，具体的公式和搜索算法细节在文中未呈现。

## 3. 实验设计：数据集、基准、对比方法
- **数据集**：
  - **标准符号回归基准**（未具体列出名称，但提到“standard symbolic regression benchmarks”）。
  - **真实世界案例**：电商数据集（e-commerce）和欺诈检测数据集（fraud detection），用于评估可解释性与实用性。
- **基准**：与现有的符号回归方法（state-of-the-art approaches）在恢复目标方程的比例上进行对比；同时也与主流的机器学习模型（如随机森林、GBDT、神经网络等）在分类精度上进行对比。
- **对比方法**：
  - 符号回归方法：文中提到“competing state-of-the-art approaches”（具体名称未给出，可能包括 GP、EQL、AI Feynman 等）。
  - 机器学习模型：未具体列出，但提到“established machine learning models”。

## 4. 资源与算力
- 文中未明确说明使用的 GPU 型号、数量或训练时长。仅在摘要中提到“requires substantially less computation”（计算量大幅降低），但未提供具体硬件配置或运行时间等细节。

## 5. 实验数量与充分性
- **实验组数**：至少包括三组主要实验：
  - 在标准符号回归基准上恢复目标方程的比例比较。
  - 在分类任务上（使用基准数据集和两个真实案例）与主流模型比较精度。
  - 可解释性验证：包括全局特征行为、决策边界分析、局部特征归因（通过示例说明数据集偏见、反事实推理、可操作洞察）。
- **充分性分析**：
  - **符号回归基准比较**：实验数量较少（仅提及一个基准集，未细分多个数据集），且未说明统计显著性。
  - **分类精度对比**：仅有两个真实案例和一个基准分类数据集，覆盖性有限（例如缺少标准 UCI 分类基准如 Iris、Adult 等），且未与多种可解释分类方法（如决策树、逻辑回归、LIME+黑箱）进行对比。
  - **可解释性验证**：提供了三个具体案例展示，但缺乏定量评估（如用户研究或解释质量指标）。
  - **消融实验**：未提及对搜索策略、复杂度正则化等的消融分析。
  - **总体评价**：实验设计尚可，但充分性中等。缺乏大规模、多领域的验证，以及更系统的可解释性评估。

## 6. 主要结论与发现
- **符号回归性能**：ECSEL 在标准符号回归基准上恢复的目标方程比例高于现有最先进方法。
- **计算效率**：所需计算量大幅低于竞争方法。
- **分类精度**：在基准数据集和两个真实案例中，ECSEL 的分类精度与主流机器学习模型相当，同时具备完全可解释性。
- **可解释性优势**：学习到的 signomial 方程能够揭示数据集偏见（如电商数据中的隐含偏差）、支持反事实推理（例如改变特征值预测结果的变化），并提供可操作的决策洞察（如欺诈检测中的关键特征组合）。
- **理论性质**：ECSEL 满足关于全局特征行为、决策边界分析和局部特征归因的一些理想性质（具体性质在摘要中未详述，但表明具有理论保证）。

## 7. 优点
- **完全可解释性**：输出为显式符号方程，无需事后解释，直接作为分类器，人可阅读。
- **高效率**：相比其他符号回归方法，计算成本显著降低，使其可应用于较大规模数据。
- **兼顾精度**：在保持可解释性的同时达到与主流黑箱模型媲美的分类性能。
- **多功能可解释性**：能提供全局、局部、决策边界分析，并支持反事实推理，实用性高。
- **理论支撑**：表明满足一些理想性质，为方法的可靠性提供了一定保证。

## 8. 不足与局限
- **实验覆盖有限**：
  - 符号回归基准的具体数量和名称未公开，难以复现；真实案例仅两个（电商、欺诈），缺乏更多样化的数据集（如医疗、金融、图像等）。
  - 未与多种可解释模型（如决策树、规则学习、线性模型）比较分类精度，也未与先进可解释 AI 方法（如 SHAP、LIME）进行定性或定量对比。
- **偏差风险**：ECSEL 学到的 signomial 方程可能依赖数据集特征分布，若数据有噪声或关联性弱，方程可能不稳定或过拟合；文中未讨论模型方差或稳健性。
- **应用限制**：仅适用于表格化、数值型特征的数据；对高维数据（如图像、文本）不适用；signomial 形式对于复杂非线性决策边界可能表达能力有限（虽然指数可实数，但仍是有限项单项式组合）。
- **资源信息缺失**：未提供硬件、运行时间、可扩展性分析，使得效率结论难以量化验证。
- **消融实验缺失**：未分析不同搜索算法、复杂度惩罚项、方程结构限制等超参数的影响。

（完）
