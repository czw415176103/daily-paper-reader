---
title: "Symbolic Density Estimation: A Decompositional Approach"
title_zh: 符号密度估计：一种分解方法
authors: "Angelo Rajendram, Xieting Chu, Max Fieg, Aishik Ghosh, Vijay Ganesh"
date: 2026-01-21
pdf: "https://openreview.net/pdf/ae0761184cd12b307ed347b9b9d5edcdeee28bbb.pdf"
tags: ["query:sr"]
score: 10.0
evidence: 使用符号回归进行密度估计的新框架
tldr: 符号回归在密度估计中未充分探索，本文提出AI-Kolmogorov多阶段框架：先通过聚类和概率图模型分解问题，再进行非参数密度估计，最后对密度估计执行符号回归。在合成混合模型、多元正态和高压物理分布上验证，生成可解释的密度表达式。
source: ICML-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 符号回归在回归任务中成功，但在密度估计中应用有限。
method: 多阶段管道：问题分解、非参数密度估计、支持估计、符号回归。
result: 在多种分布上证明了框架的有效性和可解释性。
conclusion: 将符号回归扩展到密度估计，生成可解释的概率密度模型。
---

## Abstract
We introduce AI-Kolmogorov, a novel framework for Symbolic Density Estimation (SymDE). Symbolic regression (SR) has been effectively used to produce interpretable models in standard regression settings but its applicability to density estimation tasks has largely been unexplored. To address the SymDE task we introduce a multi-stage pipeline: (i) problem decomposition through clustering and/or probabilistic graphical model structure learning; (ii) nonparametric density estimation; (iii) support estimation; and finally (iv) SR on the density estimate. We demonstrate the efficacy of AI-Kolmogorov on synthetic mixture models, multivariate normal distributions, and three exotic distributions, two of which are motivated by applications in high-energy physics. We show that AI-Kolmogorov can discover underlying distributions or otherwise provide valuable insight into the mathematical expressions describing them.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文摘要和元数据生成的中文总结。

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究背景**：符号回归（Symbolic Regression, SR）在标准回归任务中已被证明能生成可解释的数学模型，但在**概率密度估计**（Density Estimation）任务中的应用却鲜有探索。
- **核心问题**：如何将符号回归扩展到密度估计领域，以生成**可解释的密度函数表达式**，从而替代传统黑盒密度估计方法？
- **整体含义**：提出新的框架 AI-Kolmogorov，用于符号密度估计（Symbolic Density Estimation, SymDE），旨在发现数据背后的真实分布，或提供关于分布数学表达式的有价值洞见。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：采用**多阶段分解式管道**，将复杂的密度估计问题逐步拆解，最后利用符号回归从非参数密度估计中回归出简洁的解析表达式。
- **关键技术细节与算法流程**：
  1. **问题分解**：通过**聚类**（Clustering）和/或**概率图模型结构学习**（Probabilistic Graphical Model Structure Learning）将原始数据分布分解为多个简单子成分。
  2. **非参数密度估计**：对每个子成分（或整体，经分解后）进行非参数密度估计（如KDE等）。
  3. **支持估计**：估计密度的支持域（Support），以限定符号回归的搜索空间。
  4. **符号回归**：对密度估计结果执行符号回归，找到最适合匹配该密度的数学表达式。
- **公式/算法**：未在摘要中提供具体公式，但流程清晰。

## 3. 实验设计：使用了哪些数据集 / 场景，benchmark 与对比方法
- **数据集/场景**：
  - 合成混合模型（Synthetic Mixture Models）
  - 多元正态分布（Multivariate Normal Distributions）
  - 三种“异质”分布（Exotic Distributions），其中两种来源于高能物理应用。
- **Benchmark**：未明确提及具体的基准方法，但隐式对比对象应为传统非参数密度估计（如KDE）和黑盒密度估计方法。
- **对比方法**：摘要未列出具体对比方法，但强调了本框架能够发现底层分布或提供见解。

## 4. 资源与算力
- **文中未明确说明**：摘要及元数据未提及使用的GPU型号、数量、训练时长或任何算力相关信息。需注意这一点。

## 5. 实验数量与充分性
- **实验数量**：至少覆盖了**四类**分布（合成混合、多元正态、三种异质分布中的两种来自高能物理）。由于是“演示性质”（demonstrate efficacy），可能每组分布有多个参数变体，但具体实验次数未给出。
- **充分性与客观性**：从摘要看，实验覆盖了简单到复杂（包括实际物理应用）的分布，具有一定的多样性。但因缺乏消融实验、与现有符号回归方法（如PySR）或密度估计基线（如KDE、高斯混合模型）的定量比较，**充分性需存疑**。论文被ICML-2026拒稿，可能表明评审认为实验不够全面。

## 6. 论文的主要结论与发现
- AI-Kolmogorov框架能够**成功发现**潜在分布或提供有价值数学表达式。
- 证明了符号回归可以扩展到密度估计任务，生成**可解释的概率密度模型**。
- 在合成和实际（高能物理）分布上展现了有效性。

## 7. 优点：方法或实验设计上的亮点
- **创新性**：首次系统提出符号回归用于密度估计，填补了该领域的空白。
- **可解释性**：最终输出简洁的解析表达式，优于黑盒模型。
- **模块化设计**：多阶段的分解-估计-回归管道灵活，可复用不同非参数估计和符号回归算法。
- **应用驱动**：包含高能物理中的异质分布，体现实际价值。

## 8. 不足与局限
- **实验仅基于少量分布**：未在真实高维、大规模数据集（如图像、文本）上测试。
- **缺乏定量比较**：未报告与基线方法的数值指标（如KL散度、对数似然、计算时间等）。
- **计算复杂度**：未讨论多阶段管道的效率问题，可能在高维数据中面临挑战。
- **鲁棒性未验证**：未进行消融实验或对聚类/图学习模块的敏感性分析。
- **被拒稿暗示**：可能实验覆盖不够、方法细节未充分论证，或与现有方法对比不足。

（完）
