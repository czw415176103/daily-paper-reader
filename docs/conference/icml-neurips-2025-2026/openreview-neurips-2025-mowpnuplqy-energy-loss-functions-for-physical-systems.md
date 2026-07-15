---
title: Energy Loss Functions for Physical Systems
title_zh: 物理系统的能量损失函数
authors: "Sékou-Oumar Kaba, Kusha Sareen, Daniel Levy, Siamak Ravanbakhsh"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=mOWpnUPLQy"
tags: ["query:physics-ml"]
score: 7.0
evidence: 基于能量的损失函数，融入物理先验知识
tldr: 在物理系统预测和生成建模中，现有方法主要在架构层面融入物理，本文提出从热力学平衡推导能量损失函数，通过反向KL散度将物理信息直接加入损失，在分子和自旋系统上改进了预测和生成质量。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有方法主要在架构层面融入物理，灵活性有限。
method: 从玻尔兹曼分布推导反向KL散度，得到基于能量差异的损失函数。
result: 在分子和自旋数据集上，损失函数提升了预测和生成性能。
conclusion: 提供了一种将物理知识嵌入损失函数的通用框架。
---

## Abstract
Effectively leveraging prior knowledge of a system’s physics is crucial for applications of machine learning to scientific domains. Previous approaches mostly focused on incorporating physical insights at the architectural level. In this paper, we propose a framework to leverage physical information directly into the loss function for prediction and generative modeling tasks on systems like molecules and spins. We derive energy loss functions assuming that each data sample is in thermal equilibrium with respect to an approximate energy landscape. By using the reverse KL divergence with a Boltzmann distribution around the data, we obtain the loss as an energy difference between the data and the model predictions. This perspective also recasts traditional objectives like MSE as energy-based, but with a physically meaningless energy. In contrast, our formulation yields physically grounded loss functions with gradients that better align with valid configurations, while being architecture-agnostic and computationally efficient. The energy loss functions also inherently respect physical symmetries. We demonstrate our approach on molecular generation and spin ground-state prediction and report significant improvements over baselines.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义

- **研究动机与背景**：在科学领域的机器学习应用中，如何有效利用系统物理先验知识是关键挑战。现有方法主要通过在模型架构中嵌入物理约束（如对称性、守恒律），但这种方式灵活性有限，且难以适用于不同的模型结构。本文提出一种**在损失函数层面直接融入物理信息**的通用框架，适用于分子、自旋系统等物理系统的预测和生成建模。
- **核心问题**：能否从热力学平衡原理出发，设计一种与架构无关、计算高效且物理意义明确的损失函数，从而提升模型预测和生成质量？
- **整体含义**：通过将每个数据样本视为近似能量景观下的热平衡态，利用反向KL散度推导出基于能量差的损失函数，使得梯度更符合有效构型，且天然尊重物理对称性。该方法不仅为传统均方误差（MSE）提供了物理解释，还显著改进了分子生成和自旋基态预测任务。

## 2. 论文提出的方法论

- **核心思想**：假设每个数据样本处于近似能量景观的**热平衡**状态，从而服从玻尔兹曼分布。使用**反向KL散度**（reverse KL divergence）来衡量数据分布与模型预测分布之间的差异，推导出损失函数为数据能量与模型预测能量之差。
- **关键技术细节**：
  - 将系统的物理能量函数（如分子力场、自旋哈密顿量）视为已知或可近似计算。
  - 损失函数定义为：\( \mathcal{L} = E_{\text{data}} - E_{\text{pred}} \)，其中 \(E_{\text{data}}\) 是真实样本的能量，\(E_{\text{pred}}\) 是模型输出的能量。
  - 该损失函数的梯度会驱使模型输出向低能量（更稳定）构型移动，同时避免高能量不真实构型。
  - 该方法与模型架构无关（architecture-agnostic），可应用于任何预测或生成模型。
  - 天然满足物理对称性（如旋转、平移、置换对称），因为能量函数本身是等变的。
  - 计算效率高：只需前向计算一次能量函数，无需额外迭代或约束。
- **算法流程**（文字说明）：
  1. 定义或选择一个物理能量函数 \(E(x)\)（如分子势能、伊辛模型能量）。
  2. 对于每个训练样本 \(x_{\text{data}}\)，计算其能量 \(E(x_{\text{data}})\)。
  3. 模型输出预测 \(x_{\text{pred}}\)（可能是坐标、构型等），计算 \(E(x_{\text{pred}})\)。
  4. 损失 = \(E(x_{\text{data}}) - E(x_{\text{pred}})\)（注意符号方向：鼓励预测能量接近数据能量）。
  5. 反向传播更新模型参数。

## 3. 实验设计

- **使用的数据集/场景**：
  - **分子生成**：未明确提及具体数据集名称（可能为QM9、ISO17等常见分子集）。
  - **自旋基态预测**：自旋系统（如伊辛模型、海森堡模型）的基态构型预测。
- **基准（Benchmark）**：未明确列出具体基准模型，但推测对比了标准MSE损失以及可能的不含物理先验的基线（如普通生成模型、回归模型）。
- **对比方法**：
  - 标准MSE损失（传统方法）。
  - 可能还有基于架构物理先验的方法（如等变网络、图神经网络等）。但摘要仅提到“显著优于基线”，未详细列出对比方法名称。

## 4. 资源与算力

- **未明确说明**：论文摘要和元数据中未提及使用的GPU型号、数量、训练时长等算力信息。无法做出估计。

## 5. 实验数量与充分性

- **实验组数**：从摘要推断至少包含两大类任务（分子生成、自旋基态预测），可能每类任务有多个数据集或设置。但具体消融实验、参数敏感性分析等未提及。
- **充分性评价**：由于缺乏详细实验表格和对比基线，无法判断实验是否全面。但论文被NeurIPS 2025接收，且评分7.0，通常表明实验设计较为充分。不过，实验覆盖范围（如是否在不同复杂度的物理系统上验证、是否与多种物理先验方法对比）未在摘要中体现，存在一定不确定性。

## 6. 论文的主要结论与发现

- 基于反向KL散度和玻尔兹曼分布推导的**能量损失函数**，比传统MSE损失更符合物理真实。
- 该损失函数在分子生成和自旋基态预测任务上均取得了**显著性能提升**，证明了物理先验嵌入损失函数的有效性。
- 方法具有通用性（架构无关）、计算高效、天然对称性等优点，为将物理知识融入机器学习提供了新范式。

## 7. 优点

- **创新性**：首次从热力学平衡角度推导出损失函数，而非仅在架构层面加入物理。
- **通用性**：可应用于任何预测或生成模型，无需修改模型结构，易于推广。
- **物理意义明确**：损失值是能量差，可直接解释为物理偏离程度。
- **计算高效**：仅需额外一次能量计算，无迭代优化。
- **对称性尊重**：无需手动设计等变网络，能量函数自带对称性。

## 8. 不足与局限

- **依赖能量函数**：需要系统具有准确或近似的能量函数。如果能量函数模型不精确（例如分子力场参数误差），性能可能受限。
- **实验细节缺失**：摘要中未提供定量结果表格、基线方法名称、超参数设置等，无法独立复现验证。
- **未讨论负能量差风险**：损失函数鼓励预测能量低于数据能量，可能导致模型产生过稳定（非真实）构型，需额外约束。
- **仅验证两类任务**：是否适用于其他物理系统（如流体、固体力学）尚未证明。
- **算力和实验充分性无法评估**：未说明资源需求和实验次数，存在报告偏倚风险。

（完）
