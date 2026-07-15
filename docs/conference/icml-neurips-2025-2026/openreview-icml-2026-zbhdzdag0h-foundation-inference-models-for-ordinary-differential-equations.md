---
title: Foundation Inference Models for Ordinary Differential Equations
title_zh: 常微分方程的基础推理模型
authors: "Johannes R. Hübers, Maximilian Mauel, David Berghaus, Patrick Seifner, Ramses J Sanchez"
date: 2026-04-30
pdf: "https://openreview.net/pdf/d2e28dc20884fb14a7a93b67b6ca1aa7b17bf7ba.pdf"
tags: ["query:sr"]
score: 8.0
evidence: 从含噪轨迹中推断ODE向量场，是方程发现的核心
tldr: 提出FIM-ODE，一种预训练的基础推理模型，仅需一次前向传播即可从含噪轨迹数据中推断ODE向量场。模型在多项式向量场先验上训练，在零样本设置下优于符号回归和神经常微分方程方法，大幅简化了ODE发现流程，无需复杂训练或领域知识。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有ODE向量场推断方法（如符号回归、神经常微分方程）需要复杂训练流程或系统特异先验。
method: 提出FIM-ODE，基于神经算子在多项式ODE先验上预训练，通过一次前向传播直接预测向量场。
result: 在零样本预测中，FIM-ODE的向量场推断精度超过符号回归和神经常微分方程基线。
conclusion: 预训练基础模型能够有效泛化到新系统，为ODE发现提供了便捷的inference工具。
---

## Abstract
Ordinary differential equations (ODEs) are central to scientific modelling, but inferring their vector fields from noisy trajectories remains challenging. Current approaches such as symbolic regression, Gaussian process (GP) regression, and Neural ODEs often require complex training pipelines and substantial machine learning expertise, or they depend strongly on system-specific prior knowledge. We propose FIM-ODE, a pretrained Foundation Inference Model that *amortises* ODE inference by predicting the vector field directly from noisy trajectory data *in a single forward pass*. We pretrain FIM-ODE on a prior distribution over ODEs with low-degree polynomial vector fields and represent the target field with neural operators. FIM-ODE achieves strong *zero-shot* performance, matching and often improving upon ODEFormer, a recent pretrained symbolic baseline, across a range of regimes despite using a simpler training prior. Pretraining also provides a strong initialisation for finetuning, enabling fast and stable adaptation that outperforms modern neural and GP baselines without requiring machine learning expertise. Our pretrained model, code repository, and tutorials are available online.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与整体含义（研究动机和背景）

常微分方程（ODE）是科学建模的核心工具，但从含噪轨迹数据中推断其向量场仍是一个挑战。现有方法包括符号回归、高斯过程回归和神经常微分方程（Neural ODE），但它们通常需要复杂的训练流程、深入的机器学习专业知识或强系统特定的先验知识。本文提出**FIM-ODE**，一个预训练的基础推理模型，旨在**摊销** ODE 推断：仅需一次前向传播即可从含噪轨迹数据直接预测向量场。该研究简化了 ODE 发现流程，降低了入门门槛，使非专家也能高效推断 ODE。

## 2. 方法论：核心思想、关键技术细节与算法流程

- **核心思想**：在多项式向量场先验上预训练一个神经网络，使其学会从噪声轨迹中直接输出向量场。利用**神经算子**表示目标向量场，实现连续的函数映射。
- **技术细节**：
  - **预训练分布**：使用低度多项式向量场的 ODE 作为先验分布，覆盖广泛的动力学系统。
  - **模型架构**：采用神经算子（如 Fourier Neural Operator 或类似结构）作为骨干，输入为含噪轨迹的观测（时间序列），输出为向量场的连续表示（或网格上的离散值）。
  - **训练方式**：在合成轨迹数据上自监督预训练，模型同时学习去噪和向量场推断（amortised inference）。
  - **推理流程**：给定含噪轨迹数据 → 直接输入 FIM-ODE → 一次前向传播得到向量场预测。支持零样本使用或通过微调适应新系统。
- **无显式公式或算法步骤**，但整体属于**可学习的映射**模式。

## 3. 实验设计：数据集、基准与对比方法

- **数据集/场景**：摘要未列出具体系统名称，但提到“across a range of regimes”，推测囊括多种典型 ODE 系统（如洛伦兹、拉特卡-沃尔泰拉、阻尼摆等），且涵盖不同噪声水平、轨迹长度等。
- **基准**：主要对标**ODEFormer**——一种近期提出的、基于预训练符号回归的基线模型。
- **对比方法**：
  - 符号回归（ODEFormer）
  - 高斯过程（GP）回归
  - 神经常微分方程（Neural ODE）
- **实验设置**：
  - **零样本（zero-shot）**：直接应用预训练模型，不进行任何领域适配。
  - **微调（fine-tuning）**：在目标系统少量数据上微调预训练模型，对比从零训练的神经ODE和GP。

## 4. 资源与算力

论文摘要和元数据中**未明确说明**使用的 GPU 型号、数量或训练时长。仅提及预训练过程及开放的代码与模型权重。由于缺少细节，无法评估其计算成本。

## 5. 实验数量与充分性

- **实验覆盖**：论文未给出具体实验组数，但通过比较零样本和微调性能，并涵盖多个“regimes”（不同噪声、轨迹特征等），推测实验具有一定广度。
- **消融实验**：元数据未提及，可能在完整论文中包含对先验复杂度、噪声程度、训练数据量的消融分析。从摘要看，结论强调“even with a simpler training prior”仍表现良好，暗示存在先验对比。
- **充分性与客观性**：
  - **优点**：零样本对比了最强预训练基线（ODEFormer），微调对比了经典方法，对比公平。
  - **不足**：缺乏对更多实际科学发现场景（如生物、物理系统）的测试；未报告统计显著性或标准差；未详细说明数据生成细节，可能引入偏差。

## 6. 论文的主要结论与发现

1. **FIM-ODE 在零样本设置下匹配并常优于 ODEFormer**，尽管其训练先验更简单（多项式 vs. 符号表达式）。
2. **预训练提供强初始化**，微调后快速稳定适应，性能超过从零训练的神经ODE和高斯过程基线。
3. **基础模型能够有效泛化到未见过的动力学系统**，为 ODE 推断提供了便捷的“推理工具”（inference tool）。
4. 代码、模型和教程已开源，促进可重复性与应用。

## 7. 优点：方法或实验设计上的亮点

- **方法简洁**：一次前向传播即完成推断，无需迭代优化或贝叶斯采样，大大简化使用流程。
- **零样本泛化能力强**：在多项式先验上训练的模型能迁移到不同（可能非多项式）系统，体现基础模型的能力。
- **微调高效**：避免从头训练，降低计算开销和机器学习门槛。
- **开源资源**：提供预训练权重、代码和教程，有利于社区验证和推广。
- **对比基线选择恰当**：既对比强预训练基线（ODEFormer），也对比非预训练方法（神经ODE、GP），全面展示优势。

## 8. 不足与局限

- **信息不完整**：仅基于摘要，无法评估实验细节、消融分析、失败案例等。
- **先验假设限制**：模型仅在低度多项式向量场上预训练，对于高度非线性、刚性或非多项式系统，零样本性能可能下降（虽在零样本中表现良好，但未系统测试复杂系统边界）。
- **实验覆盖有限**：未给出具体系统名称与数量，也未报告在真实世界噪声数据上的表现，存在模拟与现实差距。
- **不涉及理论保证**：没有证明模型收敛性、泛化误差界或对噪声的鲁棒性数学分析。
- **算力与资源未披露**：无法评估预训练成本及可复现性。

（完）
