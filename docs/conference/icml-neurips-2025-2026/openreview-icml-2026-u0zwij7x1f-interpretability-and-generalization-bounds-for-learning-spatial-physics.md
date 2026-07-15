---
title: Interpretability and Generalization Bounds for Learning Spatial Physics
title_zh: 学习空间物理的可解释性与泛化界
authors: "Alejandro Francisco Queiruga, Theo Gutman-Solo, Shuai Jiang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/06aa48cd605618db991967bbb40e8fa7b26ad129.pdf"
tags: ["query:physics-ml"]
score: 8.0
evidence: 对线性微分方程上机器学习的泛化界进行了严格量化
tldr: 本文利用数值分析技术，严格量化了应用于线性微分方程的机器学习模型的精度、收敛速度和泛化界。发现数据的函数空间对泛化至关重要，不同模型类别可能表现出相反的泛化行为，为物理信息学习提供了理论指导。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 机器学习在科学问题中表现看似良好，但缺乏严格的量化分析，易产生误导。
method: 使用数值分析技术推导线性微分方程参数发现和求解的精度、收敛速率和泛化界。
result: 识别出数据函数空间对泛化的关键作用，并实证展示了不同模型的相反泛化趋势。
conclusion: 该工作为物理信息机器学习提供了理论基础，揭示了泛化陷阱，指导模型选择。
---

## Abstract
While there are many applications of machine learning (ML) to scientific problems that \emph{look} promising, the eye test can be misleading compared to the quantitative values.
Using numerical analysis techniques, we rigorously quantify the accuracy, convergence rates, and generalization bounds of certain ML models applied to linear differential equations (DEs) for parameter discovery or solution finding. 
Beyond the quantity and discretization of data, we identify that the {function space} of the data is critical to the generalization of the model which can lead to divergence.
Similar lack of generalization is empirically demonstrated for commonly used models.
Surprisingly, we find that different classes of models can exhibit opposing generalization behaviors.
Based on our theoretical analysis, we also introduce a new mechanistic interpretability lens on scientific models whereby Green's function representations can be extracted from the weights of black-box models.
Our results inform a new cross-validation technique for measuring generalization in physical systems, and can be useful as a benchmark of future methods.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **背景**：机器学习在科学问题（如物理建模）中有大量应用，且表面上看结果“有希望”，但仅凭肉眼观察缺乏严格的定量分析，容易产生误导性结论。
- **核心问题**：针对线性微分方程（DEs）的两类常见机器学习任务——参数发现（parameter discovery）与解求解（solution finding），缺乏对模型精度、收敛速度及泛化能力的严格理论量化。数据量、数据离散化方式之外，数据的**函数空间**（function space）对模型泛化至关重要，甚至可能导致发散。
- **整体含义**：旨在为“物理信息机器学习”（physics-informed ML）提供严格的理论基础，揭示潜在的泛化陷阱，并指导模型选择与评估。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：采用**数值分析技术**（numerical analysis techniques）对应用于线性微分方程的机器学习模型进行精确的误差分析，量化其精度、收敛速率和泛化界。
- **关键技术细节**：
  - 突破传统仅关注数据量和离散化的视角，首次将**数据的函数空间**纳入泛化分析，证明其是决定模型泛化能力的关键因素，忽视该因素会导致模型发散。
  - 引入一种新的**机械可解释性视角**（mechanistic interpretability）：通过从黑箱模型的权重中提取**格林函数（Green's function）表示**，为科学模型提供可解释性。
  - 基于理论分析，提出一种适用于物理系统的新型**交叉验证技术**，用于测量泛化能力。
- **公式与算法流程**：摘要中未给出具体公式，但描述了通过数值分析推导线性DEs的精度、收敛速率和泛化界。文字流程可概括为：
  1. 将ML模型（如神经网络、稀疏回归等）应用于线性DEs的参数发现或求解；
  2. 利用数值分析中的误差传播理论，结合数据采样的函数空间结构，推导模型输出的误差上界和收敛阶；
  3. 将模型权重与Green's函数关联，实现对内部表示的理解；
  4. 设计基于函数空间度量的交叉验证策略，替代常规的数据随机分割。

## 3. 实验设计：数据集/场景、基准、对比方法
- **数据集/场景**：论文未在摘要中明确列出具体数据集名称，但涉及**线性微分方程**的参数发现和解求解场景，可能使用标准物理方程（如泊松方程、热方程等）的合成数据。
- **基准**：未明确说明基准方法，但理论分析本身提供了**严格误差界**作为比较基准。
- **对比方法**：文中提到“不同类别的模型可以表现出相反的泛化行为”，并实验证明了常用模型（commonly used models）存在的泛化缺乏问题。对比可能涉及：
  - 物理信息神经网络（PINNs）
  - 稀疏回归方法（如SINDy）
  - 基于核的方法等。但具体对比列表未在摘要中提供。

## 4. 资源与算力
- **说明**：论文摘要及元数据中**未提及**使用的GPU型号、数量、训练时长等算力信息。因此无法总结算力资源。

## 5. 实验数量与充分性
- **实验数量**：摘要仅指出“empirically demonstrated for commonly used models”，未提供具体实验组数（如不同方程、不同模型、消融实验等）。信息有限。
- **充分性判断**：从理论推导和实验验证相结合的角度看，论文通过严格的数学证明支撑主要发现，实验作为补充。但由于缺少实验细节，难以全面评估其充分性。理论部分可能较为充分，但实验覆盖广度（如不同非线性、高维问题）未知；对比的公平性也未明确。

## 6. 论文的主要结论与发现
- **核心发现**：数据的**函数空间**是影响模型泛化的关键因素，忽视它会导致发散。
- **反直觉行为**：不同类别的模型（如某些神经网络与稀疏模型）在面对相同函数空间数据时，可能表现出**相反的泛化趋势**。
- **理论贡献**：严格量化了线性DEs上ML模型的精度、收敛速度和泛化界。
- **可解释性贡献**：提出从黑箱模型权重中提取Green's函数表示的新方法。
- **实用贡献**：提出专门针对物理系统泛化测度的新交叉验证技术，可作为未来方法的基准。

## 7. 优点：方法或实验设计上的亮点
- **理论严谨性**：利用成熟的数值分析工具，首次对线性DEs上的ML模型泛化进行了严格的理论量化，超越了仅凭视觉或经验评估。
- **新颖视角**：识别出“函数空间”这一被以往工作忽略的关键因素，解释了为什么某些看似合理的模型会在实际中失败。
- **可解释性融合**：将Green's函数（数学物理中经典工具）与黑箱模型权重联系起来，赋予了物理可解释性，有助于理解模型内部计算。
- **实用方法**：提出的交叉验证技术直接服务于物理系统，相比传统随机交叉验证更合理。
- **反直觉发现**：揭示了不同模型类别的相反泛化行为，对模型选择具有直接指导价值。

## 8. 不足与局限：实验覆盖、偏差风险、应用限制
- **实验覆盖不足**：摘要未提供实验细节，难以判断是否在多种线性DEs（如变系数、高维、复杂边界条件）上进行了充分验证，也未明确列出对比方法和消融实验。
- **偏差风险**：理论分析可能依赖于特定的数值离散假设（如网格规则性），实际应用中非理想条件可能导致偏差。
- **应用限制**：
  - 仅针对**线性微分方程**，对非线性、耦合方程系统的适用性未讨论。
  - 数据函数空间假设要求已知或可估计，实际中可能难以获得。
  - 提取Green's函数的方法可能仅适用于特定模型架构（如全连接网络或特定核方法），通用性存疑。
- **算力资源未披露**：影响可复现性评估。
- **benchmark缺失**：未明确与其他最新物理信息学习方法进行系统比较。

（完）
