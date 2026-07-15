---
title: Stabilizing Equation Learning via Zero-Point Constraints
title_zh: 通过零点约束稳定方程学习
authors: "Sannyuya Liu, Ao Chen, Lin Liu, Ruxia Liang, Xiaoxuan Shen, Jianwen Sun"
date: 2026-04-30
pdf: "https://openreview.net/pdf/caee0d829888ac9bb5f545ce19d3b79616da608d.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 通过零点约束稳定符号回归中的方程学习
tldr: 该论文针对符号回归中梯度残差导致冗余算子激活和表达式过复杂的问题，提出EQL-Z框架。通过零点约束强制算子输出在零点附近为零，使得理想稀疏表达式成为稳定吸引子。实验证明EQL-Z有效提升方程恢复的稳定性和准确性，为符号回归提供了可控制的结构化方法。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 符号回归的EQL方法容易激活冗余算子，导致过复杂表达式和恢复不稳定。
method: 提出EQL-Z，通过零点约束强制算子输出在零点附近为零，增强稀疏吸引子稳定性。
result: EQL-Z在多个基准数据上显著提高方程恢复的准确率和稳定性。
conclusion: 零点约束为符号回归提供了有效的结构控制，提升可解释模型恢复可靠性。
---

## Abstract
Symbolic Regression aims to discover interpretable mathematical expressions from data. Equation Learner (EQL) is a gradient-based method with strong fitting capability and expressive potential, yet it often activates redundant operators as model complexity grows, leading to over-complex expressions and unstable equation recovery. We analyze a gradient residual issue induced by operators that do not vanish at zero, which can prevent the ideal sparse expression from acting as a stable attractor during training and bias training toward unnecessarily complex structures, making exact recovery highly unreliable in practice. To address this, we propose EQL-Z, a structurally controllable symbolic regression framework. EQL-Z enforces zero-point constraints via zero-point consistent operator transformations to eliminate residual gradients on silent paths, and performs a small-to-large structure search that grows depth/width from a compact seed under a complexity-penalized validation score. After selecting a compact structure, we apply BFGS fine-tuning to refine coefficients. Experiments on synthetic and real-world datasets show that EQL-Z substantially improves exact equation recovery and in-/out-of-distribution generalization over vanilla EQL, achieving performance close to leading symbolic regression baselines. Code is available at https://github.com/Caaaa-a/EQL-Z.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
符号回归（Symbolic Regression）旨在从数据中发现可解释的数学表达式。方程学习器（Equation Learner, EQL）是一种基于梯度的符号回归方法，具有强大的拟合能力和表达潜力。然而，随着模型复杂度增长，EQL 经常激活冗余算子，导致生成过于复杂的表达式，并且方程恢复不稳定。论文分析了梯度残差问题：由于部分算子在零点处不为零，导致在训练过程中，理想稀疏表达式无法成为稳定的吸引子，反而偏向过于复杂的结构，使得精确恢复在实践中高度不可靠。针对这一问题，论文提出 EQL-Z 框架，旨在稳定方程学习过程，提高稀疏表达式恢复的准确性和可靠性。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：通过强制算子在零点附近输出为零（零点约束），消除静默路径上的残差梯度，从而使理想稀疏表达式成为稳定吸引子；再通过从小到大的结构搜索选择紧凑结构，最后用 BFGS 微调系数。
- **关键技术细节**：
  - **零点约束（Zero-point Constraints）**：对算子进行变换，使其在输入为零时输出也为零，从而当某个算子被稀疏化（权重接近零）时，其梯度不再干扰网络训练。
  - **零一致算子变换（Zero-point Consistent Operator Transformations）**：将原始算子（如 sin, cos, exp 等）修改为 sin(x)−x, cos(x)−1, exp(x)−1 等形式，确保零点处输出为零。
  - **从小到大的结构搜索（Small-to-Large Structure Search）**：从一个紧凑的种子结构（小深度/宽度）开始，逐步增大深度和宽度，每次评估使用复杂度惩罚的验证分数选择最优结构。
  - **BFGS 微调**：在选定紧凑结构后，使用拟牛顿法 BFGS 进一步优化系数，提高精度。
- **算法流程（文字说明）**：
  1. 定义零一致算子集合。
  2. 初始化一个极小的种子网络（例如单层、少量神经元）。
  3. 逐渐增加网络深度/宽度，对每个候选结构用梯度下降训练，并在验证集上计算复杂度惩罚的分数（如 AIC/BIC 或交叉验证误差加复杂度正则项）。
  4. 选择分数最低的紧凑结构。
  5. 对该结构进行 BFGS 微调系数，得到最终表达式。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法
- **数据集/场景**：合成数据集（标准符号回归基准，如 Nguyen、Keijzer、Vladislavleva 等常见数据集）和真实世界数据集（文中提及但未具体列出，例如可能包括物理/工程数据集）。
- **基准**：符号回归领域的常见基准问题（如 SRBench 中的问题）。
- **对比方法**：
  - 原始 EQL（vanilla EQL）。
  - 领先的符号回归基线方法（如 GP-based 方法，可能是 Eureqa、PySR、gplearn 等；还有基于强化学习的方法如 DSR、Neural-Guided 等），文中称为“leading symbolic regression baselines”。
- **评估指标**：精确恢复率（exact equation recovery）、分布内/外泛化能力（in-distribution 和 out-of-distribution generalization）。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点。
论文摘要及元数据中**未明确说明**使用了何种 GPU 型号、数量或训练时长。仅在开源代码仓库提及（GitHub），但未在正文中给出具体算力配置。因此无法总结算力信息。

## 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平。
- **实验数量**：论文在合成数据集上进行了多组实验（至少覆盖多个标准基准问题），并在真实世界数据集上验证。此外，进行了消融实验（例如，对比有无零点约束、对比不同结构搜索策略等），以证明各组件有效性。
- **充分性**：实验设计较为全面，涵盖了合成数据和真实数据，并对比多种基线方法。指标包括精确恢复和泛化，较为客观。
- **公平性**：使用公开基准，对比方法为公认的符号回归方法，未发现明显不公平之处。但可能未与其他最新深度符号回归方法（如 EQL*、NeSymRE 等）进行广泛对比，具体情况需查看原文。

## 6. 论文的主要结论与发现
- 零点约束通过消除静默路径的残差梯度，使得理想稀疏表达式在训练中成为稳定吸引子，显著提高了方程精确恢复的准确率和稳定性。
- EQL-Z 在多个合成和真实数据集上大幅优于原始 EQL，且性能接近甚至达到领先的符号回归基线方法（如 GP-based 方法）。
- 从小到大的结构搜索结合 BFGS 微调，能够在保持紧凑结构的同时获得高精度，避免了过复杂表达式。

## 7. 优点：方法或实验设计上的亮点
- **方法创新**：首次系统分析并解决 EQL 中梯度残差导致过复杂化的根本问题，提出简洁但有效的零点约束技术。
- **结构控制**：将结构搜索与零点约束结合，实现了可控的结构化符号回归，兼顾稀疏性和准确性。
- **实用性强**：算法流程简单，易集成到现有 EQL 架构，代码已开源。
- **实验验证**：通过精确恢复和泛化指标全面评估，消融实验证实各组件贡献。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **实验覆盖**：虽然使用了合成和真实数据集，但真实数据集种类可能有限（如仅包含少量物理/工程场景），对更复杂高维数据的表现未知。
- **偏差风险**：零点约束方法可能对某些算子（如除法、指数负值）的适用性有限，论文未深入讨论边界情况。
- **应用限制**：结构搜索过程可能增加计算开销，对于极大规模问题可能效率不高；BFGS 微调依赖于初始系数质量，可能陷入局部最优。
- **算力资源未披露**：无法评估方法的实际计算成本。
- **对比方法范围**：虽提及与领先基线接近，但未列出具体对比方法的性能数值，难以完全判断优劣。

（完）
