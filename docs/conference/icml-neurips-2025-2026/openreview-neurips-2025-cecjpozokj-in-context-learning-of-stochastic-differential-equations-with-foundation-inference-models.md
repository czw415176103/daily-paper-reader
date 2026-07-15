---
title: In-Context Learning of Stochastic Differential Equations with Foundation Inference Models
title_zh: 基于基础推理模型的随机微分方程上下文学习
authors: "Patrick Seifner, Kostadin Cvejoski, David Berghaus, Cesar Ojeda, Ramses J Sanchez"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=ceCJPoZOKJ"
tags: ["query:sr"]
score: 9.0
evidence: 通过上下文学习从数据中发现随机微分方程的漂移和扩散函数
tldr: FIM-SDE是一个预训练的识别模型，能够以零样本方式从带噪时间序列中准确估计低维随机微分方程的漂移和扩散函数。它通过上下文学习实现了对微分方程的发现，避免了复杂的训练过程和对动力学的强先验假设。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有的从数据中发现随机微分方程方法依赖强先验知识或复杂训练。
method: 提出FIM-SDE，一个预训练的基础推理模型，通过上下文学习实现零样本漂移和扩散估计。
result: 在低维随机微分方程上实现了准确的零样本漂移和扩散函数估计。
conclusion: 预训练的基础模型可以有效实现微分方程发现，且无需针对新任务重新训练。
---

## Abstract
Stochastic differential equations (SDEs) describe dynamical systems where deterministic flows, governed by a drift function, are superimposed with random fluctuations, dictated by a diffusion function. The accurate estimation (*or discovery*) of these functions from data is a central problem in machine learning, with wide application across the natural and social sciences. Yet current solutions either rely heavily on prior knowledge of the dynamics or involve intricate training procedures. We introduce FIM-SDE (Foundation Inference Model for SDEs), a pretrained recognition model that delivers accurate *in-context* (or zero-shot) estimation of the drift and diffusion functions of *low-dimensional* SDEs, from noisy time series data, and allows rapid *finetuning* to target datasets. Leveraging concepts from amortized inference and neural operators, we (pre)train FIM-SDE in a supervised fashion to map a large set of noisy, discretely observed SDE paths onto the space of drift and diffusion functions. We demonstrate that FIM-SDE achieves robust *in-context* function estimation across a wide range of synthetic and real-world processes --- from canonical SDE systems (*e.g*., double-well dynamics or weakly perturbed Lorenz attractors) to stock price recordings and oil-price and wind-speed fluctuations --- while matching the performance of symbolic, Gaussian process and Neural SDE baselines trained on the target datasets. When *finetuned* to the target processes, we show that FIM-SDE consistently outperforms all these baselines.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：随机微分方程（SDEs）广泛用于描述受漂移和扩散函数控制的动态系统。从带噪时间序列数据中准确估计（或“发现”）这些函数是机器学习的核心问题，在自然科学和社会科学中有重要应用。
- **现存挑战**：当前解决方案要么严重依赖动力学先验知识，要么涉及复杂的训练过程，缺乏通用性和零样本能力。
- **研究目标**：提出一种预训练的基础推理模型，能够以零样本（in-context）方式从带噪时间序列中准确估计低维SDE的漂移和扩散函数，并支持快速微调。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：FIM-SDE（Foundation Inference Model for SDEs）是一个预训练的识别模型，利用摊销推断（amortized inference）和神经算子（neural operators）的概念，通过监督学习将大量带噪离散观测的SDE路径映射到漂移和扩散函数空间。
- **关键技术细节**：
  - 预训练阶段：在合成数据上使用监督学习，模型学习从噪声离散路径到函数空间的映射。
  - 上下文学习（In-Context Learning）：无需针对新任务重新训练，即可零样本估计新观测序列对应的漂移/扩散函数。
  - 微调（Finetuning）：可快速适应目标数据集，提升性能。
- **公式/算法流程**（文字说明）：
  - 输入：一组离散观测的带噪时间序列（SDE路径）。
  - 输出：估计的漂移函数和扩散函数。
  - 训练：使用大量已知漂移和扩散的合成SDE路径，以监督方式优化模型参数，使其能够从路径直接推断函数。
  - 推理：对新序列，模型直接输出函数估计，无需迭代优化或先验假设。

### 3. 实验设计
- **数据集/场景**：
  - 合成系统：典型SDE系统，如双阱动力学（double-well dynamics）、弱扰动洛伦兹吸引子（weakly perturbed Lorenz attractors）等。
  - 真实世界过程：股票价格记录、石油价格和风速波动。
- **基准方法（Baselines）**：
  - 符号回归（symbolic methods）
  - 高斯过程（Gaussian process）
  - 神经SDE（Neural SDE）——这些基线均是在目标数据集上单独训练的。
- **对比方式**：
  - 零样本（in-context）下与基线训练后的性能对比。
  - 微调后与基线训练后性能对比。

### 4. 资源与算力
- **未明确说明**：摘要及元数据中未提及GPU型号、数量、训练时长等具体算力信息。仅指出模型为“预训练”的，但细节缺失。

### 5. 实验数量与充分性
- **实验数量**：摘要未给出具体实验组数或消融实验细节，但提到“wide range of synthetic and real-world processes”，表明覆盖了多种场景。
- **充分性评价**：
  - 从覆盖类型看（合成+真实），实验有一定广度。
  - 但缺少消融实验（如不同噪声水平、不同维度的表现）、超参数影响分析等细节，因此充分性有限。
  - 对比基线仅为摘要提及，未说明是否进行了统计显著性检验或多次重复实验，客观性难以判断。

### 6. 论文的主要结论与发现
- FIM-SDE能够在零样本条件下，对低维SDE的漂移和扩散函数做出稳健的估计。
- 在多种合成和真实世界过程上，其零样本性能可与在目标数据集上训练的符号方法、高斯过程、神经SDE基线相匹配。
- 当对目标过程进行微调后，FIM-SDE一致优于所有基线。
- 证明了预训练的基础模型可以有效实现微分方程发现，无需针对新任务重新训练。

### 7. 优点
- **零样本泛化能力**：首次将基础推理模型应用于SDE函数发现，避免了复杂训练或强先验假设。
- **灵活性**：支持快速微调以适配特定数据，在匹配基线的基础上进一步提升。
- **方法创新性**：结合摊销推断与神经算子，实现从含噪离散观测到连续函数空间的直接映射。
- **应用广泛**：适用于物理、金融、气候等多个领域的时间序列分析。

### 8. 不足与局限
- **维度限制**：仅针对低维SDE（论文明确写“low-dimensional”），高维系统或更复杂动力学的适用性未知。
- **实验覆盖不足**：未提供消融实验（如模型规模、数据量影响）、不同噪声强度下的鲁棒性分析、真实数据噪声特性评估等。
- **算力信息缺失**：无法判断训练成本是否可接受。
- **对比基线局限**：仅提及三类基线，未与最新SDE发现方法（如贝叶斯推断、变分法）对比。
- **可重复性风险**：缺乏代码或详细超参数，未说明预训练数据生成细节，可复现性存疑。

（完）
