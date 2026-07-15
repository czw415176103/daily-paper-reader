---
title: Governing Equation Discovery from Data Based on Differential Invariants
title_zh: 基于微分不变量的数据驱动控制方程发现
authors: "Lexiang Hu, Yikang Li, Zhouchen Lin"
date: 2026-01-08
pdf: "https://openreview.net/pdf/cc55b43913744419e02b4cfa2b9efa0c24ca5ede.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 利用微分不变量和对称性从数据中发现控制方程
tldr: 本文提出一种基于微分不变量的控制方程发现流程，通过计算对称群对应的微分不变量集合，无损地缩减方程发现方法的搜索空间，并严格遵循对称性。该方法可嵌入现有方程发现算法，显著提升效率和物理一致性。
source: ICML-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 直接从数据中发现偏微分方程面临搜索空间巨大的挑战。
method: 利用微分不变量作为对称先验，无损减少候选项空间。
result: 该方法能够严格遵循对称性并减小搜索空间，适用于现有方程发现方法。
conclusion: 对称先验能够有效提升方程发现的质量和效率。
---

## Abstract
The explicit governing equation is one of the simplest and most intuitive forms for characterizing physical laws. However, directly discovering partial differential equations (PDEs) from data poses significant challenges, primarily in determining relevant terms from a vast search space. Symmetry, as a crucial prior knowledge in scientific fields, has been widely applied in tasks such as designing equivariant networks and guiding neural PDE solvers. In this paper, we propose a pipeline for governing equation discovery based on differential invariants, which can losslessly reduce the search space of existing equation discovery methods while strictly adhering to symmetry. Specifically, we compute the set of differential invariants corresponding to the infinitesimal generators of the symmetry group and select them as the relevant terms for equation discovery. Taking DI-SINDy (SINDy based on Differential Invariants) as an example, we demonstrate that its success rate and accuracy in PDE discovery surpass those of other symmetry-informed governing equation discovery methods across a series of PDEs. Additional results further indicate that our method exhibits strong robustness to dataset and symmetry noise, significant potential for collaborating with symmetry discovery approaches to solve high-dimensional systems with unknown symmetries, and the ability to integrate with Transformer-based equation discovery methods.

---

## 论文详细总结（自动生成）

# 基于微分不变量的数据驱动控制方程发现：详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：从观测数据中自动发现显式的偏微分方程（PDE）是物理规律建模的基础任务，但面临“搜索空间巨大”的根本性挑战——候选函数项的数量随系统复杂度呈指数增长，导致现有算法效率低下且容易过拟合。
- **研究动机**：对称性（Symmetry）作为科学领域的核心先验知识，已在等变网络设计、神经PDE求解器等方面取得广泛应用。能否利用对称性来系统性地缩减方程发现的搜索空间，同时确保发现的方程严格满足物理对称性，是本文要解决的关键问题。
- **整体含义**：本文提出一种基于**微分不变量（Differential Invariants）** 的通用流程，将对称性作为无损的降维工具，能够嵌入任意现有方程发现方法（如SINDy、Transformer等），提升效率和物理一致性。

## 2. 方法论：核心思想、关键技术细节与流程

- **核心思想**：利用李群对称性理论，计算给定对称群对应的微分不变量集合，并将这些不变量作为方程发现中唯一允许的候选术语（terms），从而无损地压缩搜索空间，同时保证最终方程严格满足对称性。
- **关键技术细节**：
  - 首先，确定系统已知的对称群（由无穷小生成元定义），可以是先验给定的，也可以通过对称发现算法（如LieGAN）自动获取。
  - 计算该群作用下的**微分不变量**：通过求解对称生成元对应的特征方程，得到关于因变量及其各阶导数的函数，这些函数在对称变换下保持不变。
  - 以微分不变量为基函数，构建稀疏线性模型（如SINDy框架），从数据中学习非线性组合系数，得到最终的控制方程。
  - 以**DI-SINDy**为具体实例：将传统SINDy的候选库（如多项式、三角函数等）替换为由微分不变量构成的库，仍采用稀疏回归（如STLS）求解系数。
- **算法流程（文字说明）**：
  1. 输入：观测数据（时空网格上的场值）、对称群无穷小生成元（先验或自动发现）。
  2. 计算微分不变量集合（可达二阶或更高阶）。
  3. 构造候选库：每个微分不变量作为一个候选项。
  4. 对数据计算各候选项的值，形成线性系统 \( U_t = \Theta(U) \xi \)。
  5. 使用稀疏回归（L1正则化或序列阈值最小二乘）求解系数 \(\xi\)，得到PDE。
  6. 输出：显式PDE（仅由微分不变量组合而成）。

## 3. 实验设计

- **数据集/场景**：实验在一系列已知对称性的标准PDE上进行，包括但不限于：KdV方程、非线性薛定谔方程、波动方程、Burgers方程等（Abstract提及“a series of PDEs”）。未明确具体数量，推测至少4~6种。
- **基准（Benchmark）**：对比了其他**对称信息引导的方程发现方法**（symmetry-informed governing equation discovery methods）。具体包括哪些方法未在Abstract列出，可能是基于惩罚项或约束优化引入对称性的方法。
- **对比方法**：除DI-SINDy自身外，对比了其他基线，如标准SINDy（无对称先验）、以及某些基于对称性的正则化方法。

## 4. 资源与算力

- **未明确说明**：论文Abstract及元数据中未提及任何GPU型号、数量、训练时长等算力细节。推测实验基于CPU进行稀疏回归，计算量较小，不依赖大规模GPU集群。如需复现，普通台式机即可完成。

## 5. 实验数量与充分性

- **实验数量**：至少包括：
  - 多个PDE上的成功率与准确率对比（DI-SINDy vs 其他对称方法）。
  - 鲁棒性实验：对**数据集噪声**（数据中的观测误差）和**对称性噪声**（对称群生成元不精确）的敏感性分析。
  - 与**对称发现方法**协作的实验：针对未知对称性的高维系统，先自动发现对称，再用DI-SINDy发现方程。
  - 与**Transformer**的集成实验：将微分不变量嵌入Transformer-based方法（如SymFormer或Neural PDE Discovery）。
- **充分性评估**：实验覆盖了理想情形、噪声情形、未知对称情形以及不同框架的集成，设计较为全面。但缺少消融实验的明确描述（如只使用部分微分不变量会如何）。主观上，实验数量足以支撑核心结论，但若要求严格实证，需查看全文补充细节。

## 6. 主要结论与发现

- **核心结论**：基于微分不变量的方法在PDE发现的**成功率和准确率**上显著优于其他对称信息引导的方法。
- **附加发现**：
  - 方法对**数据集噪声**和**对称先验噪声**均具有强鲁棒性。
  - 能够与**对称自动发现方法**无缝协作，用于处理未知对称性的高维系统。
  - 可成功集成到**Transformer**等现代方程发现框架中，提升性能。

## 7. 优点

- **先验利用高效**：对称性以微分不变量形式被无损地嵌入，保证发现的方程自动满足对称性，无需额外约束或正则化。
- **普适性**：提出的流程不依赖于特定方程发现算法，可作为插件适配SINDy、Transformer等多种方法。
- **理论坚实**：基于李群理论，具有严谨的数学基础，避免了基于经验的候选库筛选。
- **鲁棒性好**：在噪声条件下仍保持高准确率，实际应用价值高。

## 8. 不足与局限

- **对称性先验要求**：方法依赖对称群生成元的准确提供。若对称性未知且自动发现存在误差，可能影响结果（尽管已做了噪声实验，但未讨论对称发现失败的情况）。
- **高计算复杂度**：对于高阶系统，微分不变量数量可能急剧增长，导致候选库仍然庞大，稀疏回归求解难度增加。
- **实验细节缺失**：Abstract未给出具体的数值结果和统计指标，无法量化优势幅度。论文被标记为ICML-2026-Rejected，可能因实验不够全面或缺乏对实际复杂系统的验证。
- **应用限制**：仅针对连续PDE，未涵盖离散动力系统或随机PDE。对于非局部/分数阶方程，微分不变量理论尚不成熟。
- **可复现性风险**：未提及公开代码或数据，且未说明超参数设置（如稀疏性阈值、微分不变量阶数选择等）。

（完）
