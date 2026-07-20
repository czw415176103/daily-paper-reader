---
title: Foundation Inference Models for Ordinary Differential Equations
title_zh: 常微分方程的基础推理模型
authors: "Johannes R. Hübers, Maximilian Mauel, David Berghaus, Patrick Seifner, Ramses J Sanchez"
date: 2026-04-30
pdf: "https://openreview.net/pdf/d2e28dc20884fb14a7a93b67b6ca1aa7b17bf7ba.pdf"
tags: ["query:physics-ml"]
score: 7.0
evidence: ODE基础推理模型，与物理信息机器学习相关
tldr: 从噪声轨迹推断ODE向量场仍困难，现有方法需复杂流程或领域知识。本文提出FIM-ODE基础推理模型，通过单次前向传播直接预测向量场，预训练于低次多项式场分布。在零样本和微调场景下超越符号回归和神经ODE，为物理信息基础模型提供范例。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有ODE推断方法复杂或依赖系统特定先验，亟需通用高效的推理模型。
method: 预训练基础模型FIM-ODE，使用神经算子表示目标场，从噪声轨迹直接预测向量场。
result: 零样本性能优于符号回归等传统方法，微调后进一步提升。
conclusion: 展示了基础模型在科学ML中的潜力，但未显式结合物理约束。
---

## Abstract
Ordinary differential equations (ODEs) are central to scientific modelling, but inferring their vector fields from noisy trajectories remains challenging. Current approaches such as symbolic regression, Gaussian process (GP) regression, and Neural ODEs often require complex training pipelines and substantial machine learning expertise, or they depend strongly on system-specific prior knowledge. We propose FIM-ODE, a pretrained Foundation Inference Model that *amortises* ODE inference by predicting the vector field directly from noisy trajectory data *in a single forward pass*. We pretrain FIM-ODE on a prior distribution over ODEs with low-degree polynomial vector fields and represent the target field with neural operators. FIM-ODE achieves strong *zero-shot* performance, matching and often improving upon ODEFormer, a recent pretrained symbolic baseline, across a range of regimes despite using a simpler training prior. Pretraining also provides a strong initialisation for finetuning, enabling fast and stable adaptation that outperforms modern neural and GP baselines without requiring machine learning expertise. Our pretrained model, code repository, and tutorials are available online.

---

## 论文详细总结（自动生成）

# 常微分方程的基础推理模型：FIM-ODE

## 1. 核心问题与整体含义（研究动机和背景）
常微分方程（ODE）是科学建模的核心工具，但从带有噪声的轨迹数据中推断出其向量场仍然非常困难。现有方法（如符号回归、高斯过程回归、神经ODE）通常需要复杂的训练流程、大量机器学习专业知识，或强烈依赖系统特定的先验知识。这限制了它们在通用科学场景下的易用性和可迁移性。本文旨在构建一个**基础推理模型**，通过单次前向传播直接从噪声轨迹预测向量场，实现高效的零样本推理和快速微调适应。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：采用**摊销推理（amortised inference）**，即通过预训练一个基础模型，使模型学会从轨迹到向量场的映射，从而在推理时无需针对每个新系统重新训练。
- **模型名称**：FIM-ODE（Foundation Inference Model for ODE）。
- **技术细节**：
  - 预训练数据：从低次多项式向量场的ODE先验分布中采样生成噪声轨迹。
  - 表示方式：使用**神经算子（neural operators）**来参数化目标向量场，使其能够处理不同分辨率的输入和输出。
  - 训练目标：直接从带噪声的轨迹数据预测整个向量场，端到端学习。
  - 推理：一次前向传播即可得到向量场预测，无需迭代优化。

## 3. 实验设计
- **数据集/场景**：未在摘要中详细列举具体数据集，但从对比方法推断，应包括多种不同复杂度的ODE系统（如多项式场、非线性振荡器、混沌系统等），且带有不同信噪比的噪声轨迹。
- **Benchmark**：以ODEFormer（近期基于符号回归的预训练基线）为主要对比对象，同时比较符号回归、高斯过程回归、神经ODE等传统方法。
- **对比方法**：
  - ODEFormer（预训练符号化基线）
  - 传统符号回归（如PySR等）
  - GP回归
  - 神经ODE
- **性能指标**：主要报告零样本性能（预测向量场的精度）以及微调后的性能提升。

## 4. 资源与算力
文中未明确提及具体使用的GPU型号、数量或训练时长。仅说明预训练模型、代码仓库和教程已在线公开。因此无法给出具体算力信息。

## 5. 实验数量与充分性
- 实验类型：至少包含**零样本评估**和**微调评估**两个主要场景。
- 充分性：摘要中声称在多种设定下匹配并常优于ODEFormer，但未提供具体实验数量（如多少个ODE系统、多少个噪声水平、消融实验等）。从ICML录用论文的标准来看，实验应该比较充分，但摘要信息有限，无法完全判断。从描述看，对比了多个基线，且涵盖零样本和微调，较为客观。

## 6. 主要结论与发现
- FIM-ODE在零样本设置下**匹配甚至超越**ODEFormer，尽管使用了更简单的训练先验（低次多项式场）。
- 预训练为微调提供了强初始化，使得微调过程快速稳定，且性能优于现代的神经ODE和高斯过程基线。
- 展示了基础推理模型在科学机器学习中的潜力，无需用户具备机器学习专业知识即可使用。
- 模型、代码和教程均已开源，便于社区复现和应用。

## 7. 优点（方法与实验亮点）
- **端到端摊销推理**：避免了传统方法对每个新系统重新训练或复杂调参，极大降低了使用门槛。
- **强零样本能力**：即使训练先验简单（仅多项式场），也能泛化到更复杂的ODE系统。
- **微调友好**：预训练权重作为良好初始化，使适应新系统快速且稳定。
- **神经算子表示**：能够灵活处理任意分辨率的输入轨迹，且预测的向量场可连续。
- **开源资源**：提供完整模型、代码和教程，促进可复现性和进一步研究。

## 8. 不足与局限
- **未显式结合物理约束**：模型完全由数据驱动，没有引入物理定律（如守恒律、对称性）作为归纳偏置，可能降低在严格物理问题上的可靠性。
- **先验分布局限性**：训练仅基于低次多项式ODE先验，虽然零样本泛化尚可，但可能对高次、非线性、非多项式系统存在性能上限。
- **实验细节缺失**：论文摘要未提及具体数据集、噪声类型与水平、模型规模等，无法评估实验覆盖范围（如是否包含刚性问题、高维系统等）。
- **算力信息不明**：无法判断模型训练的资源需求，可能对资源有限的研究者不友好。
- **仅比较了部分基线**：未提到与最新基于Transformer的神经算子或扩散模型方法的对比，基线选择可能不够全面。

（完）
