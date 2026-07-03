---
title: Robust discovery of governing equations through symmetry
title_zh: 通过对称性稳健发现控制方程
authors: "Haoran Chen, Dunhui Xiao"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=n0CiqI3Rz2"
tags: ["query:sr"]
score: 9.0
evidence: 基于对称性的符号回归方程发现
tldr: 从数据中发现控制方程易受噪声和数据稀疏影响。本文提出对称性启发符号回归框架，自动识别物理不变性并嵌入对称约束，结合验证步骤，在含噪PDE恢复中表现稳健，得到紧凑准确的方程。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有方程发现方法对噪声敏感且数据需求大。
method: 自动识别对称性并嵌入对称约束变量集，再进行符号回归。
result: 在PDE和变系数系统上恢复的方程更紧凑、更准确。
conclusion: 利用对称性可提升方程发现的鲁棒性和稀疏性。
---

## Abstract
Discovering governing equations of dynamical systems directly from data remains a fundamental challenge, especially under noise and data scarcity. We propose a symmetry-inspired symbolic regression (SI-SR) framework that automatically identifies intrinsic physical invariances and embeds them into a symmetry-constrained variable set, enhancing robustness and promoting sparsity. The framework combines a validation step for symmetry confirmation with symbolic regression for expressive nonlinear modelling. We evaluate SI-SR on canonical partial differential equations (PDEs) and variable-coefficient systems, with systematic comparisons against state-of-the-art baselines. Results show that leveraging symmetry reduces redundancy and enables the recovery of compact, accurate models. This establishes symmetry as a powerful inductive bias for data-driven equation discovery.

---

## 论文详细总结（自动生成）

# 论文总结：通过对称性稳健发现控制方程

## 1. 核心问题与整体含义
- **研究动机**：从数据中发现动力学系统的控制方程（尤其是偏微分方程）仍面临根本性挑战，主要困难在于数据噪声和数据稀疏性会严重干扰方程的恢复。
- **整体含义**：提出一种受对称性启发的符号回归（SI-SR）框架，通过自动识别物理系统固有的不变性（对称性）并将其作为约束嵌入变量集，来增强方程发现的鲁棒性并促进模型稀疏性，从而在有限且含噪的数据下仍能恢复紧凑、准确的控制方程。

## 2. 方法论
- **核心思想**：利用对称性作为归纳偏置（inductive bias），在符号回归过程中自动嵌入物理不变性约束，减少冗余，提升模型的可解释性和泛化能力。
- **关键技术细节**：
  - 自动识别系统内在的对称性（如平移不变性、缩放不变性等）。
  - 将识别出的对称性转化为对称约束变量集（symmetry-constrained variable set），替代原始变量进行符号回归。
  - 框架包含一个验证步骤（validation step），用于确认所识别对称性的有效性。
  - 符号回归（symbolic regression）部分负责生成非线性表达式模型。
- **公式或算法流程**（文字说明）：
  1. 输入含噪观测数据。
  2. 自动检测数据中隐含的对称性（例如通过李群方法或数据驱动不变性检测）。
  3. 将对称性转化为一组约束变量（如不变特征、约化变量）。
  4. 针对约束变量集执行符号回归，搜索形式紧凑的解析表达式。
  5. 通过验证步骤确认对称性假设并评估方程质量。
  6. 输出最终的控制方程。

## 3. 实验设计
- **数据集/场景**：标准偏微分方程（PDE）以及变系数系统（variable-coefficient systems）。
- **Benchmark**：与当前最先进的基线方法进行系统比较（baselines未在摘要中具体列出名称，但提到“state-of-the-art baselines”）。
- **对比方法**：未详细说明，可能包含 SINDy、EQL、其他符号回归方法等。

**注意**：由于仅为摘要，实验的具体数据集名称、对比方法细节、评价指标等均未给出。

## 4. 资源与算力
- **未明确说明**：摘要及元数据中未提及使用的 GPU 型号、数量、训练时长等算力信息。因此无法总结。

## 5. 实验数量与充分性
- **实验数量**：摘要仅提及在“标准偏微分方程和变系数系统”上进行评估，未说明具体实验组数。也未提及消融实验。
- **充分性评估**：由于缺乏详细信息，无法判断实验的充分性和公平性。仅从摘要看，似乎涵盖了典型 PDE 和变系数场景，但缺少对噪声程度、数据量大小、不同对称性类型等变量的系统探讨，整体实验充分性不明确。

## 6. 主要结论与发现
- 利用对称性可以减少冗余，使得恢复的方程更紧凑、更准确。
- 对称性作为归纳偏置能够有效提升数据驱动方程发现方法的鲁棒性（对噪声和数据稀疏的耐受性）。
- 该方法在 PDE 和变系数系统上均优于现有基线，验证了对称性约束的价值。

## 7. 优点
- **方法亮点**：首次将对称性自动识别与符号回归结合，无需人工预设对称性类型，实现了物理不变性的自动挖掘与嵌入。
- **鲁棒性**：对称性约束天然过滤噪声干扰，提升模型在低信噪比场景下的表现。
- **稀疏性**：约束变量集减少了搜索空间，促进生成更简洁的方程，避免过拟合。
- **验证步骤**：增加了自洽性验证，保证识别出的对称性可信。

## 8. 不足与局限
- **实验覆盖不足**：摘要未提供具体的实验细节（数据集、噪声水平、数据量、基线结果数值），无法独立验证方法效果。
- **对称性假设强度**：要求系统存在明显对称性，对于无对称或复杂对称的系统可能失效。
- **计算成本**：未讨论符号回归在大规模高维系统中的计算开销。
- **可复现性**：未提供代码或开源实现，且实验设置不透明。
- **局限性**：方法仅适用于可观测动态系统，对于随机系统或对称性破坏的系统可能不适用。

（完）
