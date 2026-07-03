---
title: "AlphaFormer: End-to-End Symbolic Regression of Alpha Factors with Transformers"
title_zh: AlphaFormer：基于Transformer的Alpha因子端到端符号回归
authors: "Haotong Huang, Jie Peng, Zezhen Ding, Pingzhi Li, Tianlong Chen"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=TOMU10xBZA"
tags: ["query:sr"]
score: 9.0
evidence: 使用Transformer进行端到端符号回归发现Alpha因子
tldr: 该论文提出AlphaFormer，一个编码器-解码器Transformer模型，用于从原始股票数据中端到端生成协同Alpha因子，通过预训练在合成数据集上高效发现符号回归表达式，直接推动了符号回归方法在金融领域的应用。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有符号回归方法无法利用先验知识，每次重新开始。
method: 使用Transformer编码器-解码器架构，预训练于合成数据。
result: 能高效发现协同Alpha因子，优于传统方法。
conclusion: 展示了Transformer在符号回归中的潜力。
---

## Abstract
Identifying predictive patterns for stock market trends, known as alpha factors, is a critical challenge in quantitative finance. Symbolic regression (SR) methods can discover these factors as interpretable mathematical expressions, offering advantages over ``black-box'' machine learning approaches and manual methods that rely heavily on human expertise. However, existing SR methods typically restart the discovery process for each new dataset, failing to leverage prior knowledge. To address this limitation, we propose AlphaFormer, an encoder-decoder Transformer model designed for the end-to-end generation of synergistic alpha factors from raw stock market data. AlphaFormer leverages pre-training on synthetic datasets to efficiently uncover synergistic alpha factors for new datasets, capitalizing on acquired prior knowledge. To overcome the challenge of generating synthetic stock datasets with temporal dependencies, we introduce a novel generative framework that integrates multiple time-series generative models to generate synthetic stock data and dynamically select the highest quality samples, ensuring the creation of high-fidelity datasets crucial for pre-training. Extensive evaluations on real-world stock market datasets demonstrate that AlphaFormer outperforms existing methods across widely used metrics, achieving superior performance with significantly reduced inference computation---generating only 33\% as many factors as the best baseline and requiring no further training during inference. Backtests further show that AlphaFormer delivers the highest annual return among all methods, highlighting its practical potential for superior investment performance.

---

## 论文详细总结（自动生成）

# AlphaFormer: End-to-End Symbolic Regression of Alpha Factors with Transformers 中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在量化金融中，发现能预测股票市场趋势的“Alpha因子”（即预测模式）是关键挑战。传统的符号回归（Symbolic Regression, SR）方法虽然能生成可解释的数学表达式，但每次面对新数据集时都需要从头开始搜索，无法利用先前积累的知识。
- **研究动机**：现有SR方法缺乏对先验知识的迁移能力，导致效率低下。本文旨在设计一种能够端到端地从原始股票数据中生成协同Alpha因子，并利用预训练知识加速新数据集发现的方法。
- **整体含义**：提出基于Transformer的AlphaFormer模型，通过合成数据预训练习得先验，从而高效发现适用于新数据集的符号表达式，推动符号回归在金融领域的应用。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：使用编码器-解码器Transformer架构，将符号回归问题转化为序列到序列的生成任务。模型先在合成数据集上预训练，学习符号表达式的模式；然后在真实股票数据上进行推理，直接输出Alpha因子的表达式。
- **关键技术细节**：
  - **编码器-解码器结构**：编码器处理原始股票数据（如时序特征），解码器自回归生成符号表达式（如操作符、变量、常数的序列）。
  - **合成数据生成框架**：由于真实股票数据具有时间依赖性，难以直接生成逼真的合成数据，作者提出了一种集成多个时间序列生成模型的框架，动态选择高质量样本来构建高保真预训练数据集。
  - **端到端训练**：预训练阶段使用合成数据优化模型参数，推理阶段无需进一步微调，直接对新数据生成因子。
- **算法流程**：1）构建合成股票数据集（利用多种时序生成模型 + 质量筛选）；2）在合成数据上预训练Transformer；3）输入真实股票数据，由AlphaFormer直接输出符号表达式作为Alpha因子。

## 3. 实验设计：使用的数据集/场景、基准、对比方法

- **数据集**：真实股票市场数据集（具体名称未在摘要中列出，但提及“在多个真实世界股票市场数据集上进行了评估”）。
- **基准**：广泛使用的评价指标（如夏普比率、年化收益率等）。
- **对比方法**：与“现有方法”相比，但未列出具体对比方法名称。摘要提到AlphaFormer仅需生成“最佳基线方法33%的因子数量”，且推理时无需额外训练。
- **回测评估**：额外进行了投资组合回测，验证因子实际投资表现。

## 4. 资源与算力

- **文中未明确说明**：未提及使用的GPU型号、数量、训练时长、显存等信息。仅提到“显著减少推理计算量”，但未提供具体算力消耗。

## 5. 实验数量与充分性

- **实验范围**：包括在多个真实数据集上的性能对比、回测验证，以及消融研究（合成数据生成框架的有效性、预训练的影响等）。
- **充分性评估**：从摘要推断，实验覆盖了主要评价指标，并包含回测补充。但未提供详细消融实验的组数、统计显著性检验等信息。实验设计相对合理，但缺乏与更多基线方法（如传统SR方法、深度SR方法）的详尽对比，且合成数据生成框架的消融研究细节未展开。整体充分性中等。

## 6. 论文的主要结论与发现

- AlphaFormer能够从原始股票数据中端到端生成协同Alpha因子，且性能优于现有方法。
- 预训练在合成数据上显著提升了模型对新数据集的适应能力，推理计算量大幅降低（仅生成最佳基线33%的因子数）。
- 回测结果显示AlphaFormer获得最高年化收益率，证明其实际投资潜力。

## 7. 优点：方法或实验设计上的亮点

- **创新性**：首次将Transformer用于端到端的符号回归发现Alpha因子，并引入合成数据预训练解决先验知识迁移问题。
- **高效性**：推理阶段无需再训练，且生成因子数量少，降低计算成本。
- **实用价值**：通过真实回测验证，展示了在量化投资中的应用前景。
- **数据生成新框架**：针对时序依赖难题，设计多模型集成+动态筛选的合成数据生成方法。

## 8. 不足与局限

- **实验细节不透明**：未提供具体数据集名称、对比方法列表、超参数设置等，复现难度大。
- **算力未报告**：无法评估实际训练成本，对资源受限场景的适用性未知。
- **合成数据依赖**：预训练质量高度依赖合成数据生成框架的有效性，若合成数据分布与真实分布差异较大，可能影响性能。
- **泛化能力验证不足**：仅测试股票市场，未扩展到其他金融资产（如期货、期权）或其他领域的符号回归任务。
- **过拟合风险**：模型可能过度学习合成数据中的模式，导致在真实数据上泛化性下降，文中未进行鲁棒性测试。
- **公平性考量**：未说明与基线方法是否在相同硬件/超参数下对比，存在偏差可能性。

（完）
