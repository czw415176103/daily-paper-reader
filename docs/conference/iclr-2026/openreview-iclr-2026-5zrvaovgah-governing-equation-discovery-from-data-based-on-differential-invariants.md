---
title: Governing Equation Discovery from Data Based on Differential Invariants
title_zh: 基于微分不变量的数据驱动控制方程发现
authors: "Lexiang Hu, Yikang Li, Zhouchen Lin"
date: 2025-09-02
pdf: "https://openreview.net/pdf?id=5zrvAOvgAH"
tags: ["query:sr"]
score: 9.0
evidence: 基于微分不变量从数据中发现偏微分方程
tldr: 从数据中发现显式控制方程是物理定律建模的关键，但搜索空间巨大且易违反物理规律。本文提出基于微分不变量的方程发现流程，利用对称性先验无损缩减搜索空间，确保发现方程满足指定对称性，显著提升方程发现效率。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 直接发现偏微分方程面临搜索空间庞大和物理规律违反的问题。
method: 计算对称变换的微分不变量集，以此作为原子项进行方程发现，严格保证对称性。
result: 该方法有效缩减搜索空间，与现有方程发现方法无缝集成。
conclusion: 微分不变量为物理约束下的方程发现提供了可靠框架。
---

## Abstract
The explicit governing equation is one of the simplest and most intuitive forms for characterizing physical laws. However, directly discovering partial differential equations (PDEs) from data poses significant challenges, primarily in determining relevant terms from a vast search space. Symmetry, as a crucial prior knowledge in scientific fields, has been widely applied in tasks such as designing equivariant networks and guiding neural PDE solvers. In this paper, we propose a pipeline for governing equation discovery based on differential invariants, which can losslessly reduce the search space of existing equation discovery methods while strictly adhering to symmetry. Specifically, we compute the set of differential invariants corresponding to the infinitesimal generators of the symmetry group and select them as the relevant terms for equation discovery. Taking DI-SINDy (SINDy based on Differential Invariants) as an example, we demonstrate that its success rate and accuracy in PDE discovery surpass those of other symmetry-informed governing equation discovery methods across a series of PDEs. Additional results further indicate that our method exhibits strong robustness to data and symmetry noise, as well as significant potential for solving high-dimensional dynamic systems.

---

## 论文详细总结（自动生成）

# 论文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：从观测数据中自动发现显式控制方程（尤其是偏微分方程）是物理定律建模的关键，但直接搜索面临两个主要挑战：搜索空间巨大（需要从大量候选项中筛选），且易违反物理规律。
- **研究动机**：对称性作为物理领域的重要先验知识，已在等变网络设计与神经PDE求解器中被广泛使用，但尚未被充分用于方程发现过程中来缩小搜索空间并保证物理一致性。
- **整体含义**：本文提出一种基于微分不变量的方程发现流程，利用对称性先验无损缩减搜索空间，同时严格保证所发现的方程满足指定对称性，从而提升效率与准确性。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程（用文字说明即可）

- **核心思想**：将对称性作为先验嵌入方程发现过程。通过计算对称变换的微分不变量（Differential Invariants）集合，并以此作为候选项进行稀疏回归，从而无损地缩减搜索空间并强制方程满足给定的对称性。
- **关键技术细节**：
    1. 给定对称群及其无穷小生成元，计算对应的微分不变量集（即对原坐标系下各阶导数在群变换下保持不变的函数组合）。
    2. 将微分不变量集作为方程发现的原子项（而非原始项），然后在低维特征空间中使用稀疏回归（如SINDy）搜索方程。
    3. 这样得到的方程自动满足指定对称性，且项数大幅减少。
- **算法流程**（文字说明）：
    1. 输入：观测数据（U, x, t）以及期望的对称群描述（如平移、旋转、缩放等）。
    2. 计算对称群的无穷小生成元，进而求出微分不变量基。
    3. 利用微分不变量基构造候选库（如多项式组合）。
    4. 采用稀疏回归方法（如SINDy）从候选库中筛选出最稀疏且拟合误差最小的方程。
    5. 输出：满足对称性的控制方程。
- **示例方法**：DI-SINDy（基于微分不变量的SINDy）作为具体实现。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **数据集/场景**：一系列偏微分方程（PDE）作为测试场景，具体包括哪些方程未在摘要中列举，但提到高维动态系统。
- **Benchmark**：以其他基于对称性先验的方程发现方法作为对比基准。
- **对比方法**：其他“symmetry-informed governing equation discovery methods”，具体名称未列出，但强调DI-SINDy在成功率和准确率上均优于它们。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点。

- **未明确说明**：论文摘要与元数据中未提及使用的GPU型号、数量或训练时长等算力信息。因此无法总结具体的计算资源。

## 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平

- **实验组数**：摘要中仅描述为“a series of PDEs”，未给出确切数量。此外提到对“data and symmetry noise”的鲁棒性实验以及高维动态系统潜力实验，但未列出消融实验。
- **充分性评估**：
    - 优点：包含对比实验、鲁棒性分析及高维潜力验证，覆盖多个维度。
    - 不足：缺少明确的数据集列表、超参数敏感性分析、与多种先进方法的系统比较（仅与“其他对称性方法”对比），也未报告统计显著性检验。因此实验数量与客观性有待进一步确认。

## 6. 论文的主要结论与发现

- **主要结论**：基于微分不变量的方程发现方法能够无损缩减搜索空间，同时严格保证发现的方程满足指定对称性，显著优于其他利用对称性的方法。
- **具体发现**：
    - DI-SINDy在PDE发现任务中的成功率和准确性优于其他对称性知情方法。
    - 该方法对数据和对称噪声具有较强的鲁棒性。
    - 在解决高维动态系统方面展现出显著潜力。

## 7. 优点：方法或实验设计上有哪些亮点

- **方法亮点**：
    - 利用微分不变量将对称性先验以无损方式嵌入，避免了暴力搜索，大幅降低搜索空间。
    - 保证发现方程天然符合物理对称性，增强可解释性与泛化能力。
    - 该方法与现有方程发现方法（如SINDy）无缝集成，通用性强。
- **实验亮点**：
    - 考察了噪声鲁棒性（数据噪声与对称噪声），贴近真实应用场景。
    - 初步验证了高维系统的可行性（尽管详细结果未给出）。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验覆盖不足**：
    - 未明确列出测试的所有PDE类型与参数，难以判断方法普适性。
    - 缺少与无先验方法的全面对比（仅与其他对称性方法对比）。
    - 未报告超参数调节细节与统计显著性。
- **偏差风险**：
    - 过于依赖对称群的预先指定，若对称性推测错误，可能导致方程发现失败或产生误导。
    - 方法可能仅适用于具有明确连续对称性的系统，离散对称性或混合对称性处理未提及。
- **应用限制**：
    - 高维系统下微分不变量计算可能变得复杂，且稀疏回归效率需进一步验证。
    - 未讨论数据稀疏或噪声剧烈时的表现边界。

（完）
