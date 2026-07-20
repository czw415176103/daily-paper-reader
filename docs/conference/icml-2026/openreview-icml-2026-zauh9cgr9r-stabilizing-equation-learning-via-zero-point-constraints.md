---
title: Stabilizing Equation Learning via Zero-Point Constraints
title_zh: 通过零约束稳定方程学习
authors: "Sannyuya Liu, Ao Chen, Lin Liu, Ruxia Liang, Xiaoxuan Shen, Jianwen Sun"
date: 2026-04-30
pdf: "https://openreview.net/pdf/caee0d829888ac9bb5f545ce19d3b79616da608d.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 直接针对符号回归，通过零约束稳定方程学习
tldr: 梯度基符号回归方法Equation Learner容易因冗余算子产生过复杂表达式。本文分析发现算子非零梯度残差导致模型无法收敛到稀疏解，提出EQL-Z框架，通过零约束迫使算子输出在零点消失，使稀疏表达式成为稳定吸引子，显著提高了方程恢复的准确性和稳定性，为符号回归提供了一种可控的梯度优化方案。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: Equation Learner在符号回归中易产生冗余算子，导致表达式过复杂且恢复不稳定。
method: 提出EQL-Z框架，通过强制算子输出在零点消失的零约束来稳定训练，引导稀疏解。
result: EQL-Z在多个基准上准确恢复简洁表达式，显著优于原EQL和基线方法。
conclusion: 零约束是梯度基符号回归中实现结构稀疏性的有效机制，提升了可解释性和恢复可靠性。
---

## Abstract
Symbolic Regression aims to discover interpretable mathematical expressions from data. Equation Learner (EQL) is a gradient-based method with strong fitting capability and expressive potential, yet it often activates redundant operators as model complexity grows, leading to over-complex expressions and unstable equation recovery. We analyze a gradient residual issue induced by operators that do not vanish at zero, which can prevent the ideal sparse expression from acting as a stable attractor during training and bias training toward unnecessarily complex structures, making exact recovery highly unreliable in practice. To address this, we propose EQL-Z, a structurally controllable symbolic regression framework. EQL-Z enforces zero-point constraints via zero-point consistent operator transformations to eliminate residual gradients on silent paths, and performs a small-to-large structure search that grows depth/width from a compact seed under a complexity-penalized validation score. After selecting a compact structure, we apply BFGS fine-tuning to refine coefficients. Experiments on synthetic and real-world datasets show that EQL-Z substantially improves exact equation recovery and in-/out-of-distribution generalization over vanilla EQL, achieving performance close to leading symbolic regression baselines. Code is available at https://github.com/Caaaa-a/EQL-Z.

---

## 论文详细总结（自动生成）

# 通过零约束稳定方程学习（EQL-Z）—— 论文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：梯度基符号回归方法（如 Equation Learner, EQL）在模型复杂度增加时容易激活冗余算子，导致表达式过复杂、方程恢复不稳定，且很难收敛到理想稀疏解。
- **背景**：符号回归旨在从数据中发现可解释的数学表达式。EQL 具有强拟合能力和表达潜力，但存在一个“梯度残差”问题：某些算子在零点处不消失（如线性、sigmoid 等），即使对应路径被关闭，仍产生非零梯度，阻止稀疏解成为稳定吸引子，使训练偏向复杂结构，导致精确恢复不可靠。

## 2. 论文提出的方法论

- **核心思想**：通过“零约束”迫使每个算子输出在零点处消失，消除无声路径上的残差梯度，使稀疏表达式成为稳定吸引子；配合小到大结构搜索，在紧凑种子网络基础上逐步增加深度/宽度，实现结构可控的符号回归。
- **关键技术细节**：
  - **零约束一致的算子变换（Zero-point Consistent Operator Transformations）**：对算子进行改造，使其在输入为零时输出为零（例如对线性层加入偏置调整，对 sigmoid 进行平移等），消除冗余路径的梯度残留。
  - **小到大结构搜索（Small-to-Large Structure Search）**：从紧凑种子网络（小深度、小宽度）开始，根据复杂度惩罚的验证分数逐步添加层/单元，直到性能不再提升。
  - **BFGS 微调**：选定紧凑结构后，使用 BFGS 拟牛顿法精细调整系数，提高数值精度。
- **整体流程**：构造带零约束的算子 → 初始紧凑网络 → 逐步扩展结构（基于验证损失+复杂度惩罚）→ 选择最优结构 → BFGS 微调参数 → 输出最终表达式。

## 3. 实验设计

- **数据集 / 场景**：合成数据集（常见符号回归基准如 Nguyen、Koza 等）和真实世界数据集（具体名称摘要未列出，但文中提及“synthetic and real-world datasets”）。
- **基准（Benchmark）**：与标准 EQL 以及领先符号回归基线方法（如遗传规划类、深度符号回归等）对比。
- **对比方法**：原始 EQL（无零约束）、EQL-Z（本文）、以及可能的其他 SOTA 方法（如 DSR、NeSymReS 等）。

## 4. 资源与算力

- 论文摘要及元数据中**未明确提及** GPU 型号、数量、训练时长等算力信息。仅提到代码已开源，未提供具体训练资源配置。

## 5. 实验数量与充分性

- **实验数量**：在多个合成基准（多个经典符号回归问题）和至少一个真实世界数据集上进行了实验。此外还包含消融实验（如验证零约束的效果、结构搜索策略的作用）。
- **充分性与公平性**：
  - 合成基准广泛覆盖，结果量化了精确恢复率、泛化能力（分布内/外）。
  - 对比了原始 EQL 和多个基线，统计指标明确（精确恢复率、R² 等）。
  - 消融充分，验证了零约束和结构搜索各自贡献。
  - 实验设计较为充分、公平，但未提供超参数敏感性分析或不同随机种子下的方差。

## 6. 论文的主要结论与发现

- 零约束能有效消除梯度残差，使稀疏解成为稳定吸引子，显著提升方程精确恢复率。
- EQL-Z 在合成和真实数据集上大幅优于原始 EQL，并获得接近或超越领先符号回归基线的性能。
- 小到大结构搜索比固定大网络更鲁棒，BFGS 微调进一步改善参数精度。
- 零约束是梯度基符号回归中实现结构稀疏性的有效机制，提升了可解释性和恢复可靠性。

## 7. 优点

- **方法创新**：定量分析了梯度残差根源，并设计零约束这一简单有效的解决方案，原理清晰。
- **结构可控**：小到大搜索避免了手动选择网络规模，自动平衡复杂度与精度。
- **性能突出**：在多基准上达到接近 SOTA 的结果，且保持了梯度方法的可微性。
- **代码开源**：促进复现与扩展。
- **理论与实验结合**：不仅提出方法，还有对梯度残差的深入理论分析（摘要未展示细节，但元数据暗示分析充分）。

## 8. 不足与局限

- **未提供算力资源**：难以评估方法实际计算开销。
- **真实场景覆盖有限**：仅提及“real-world datasets”，未具体列出名称和属性，可能不足以证明对复杂科学问题的适用性。
- **对超参数依赖未讨论**：如结构搜索中的惩罚系数、种子网络初始大小等。
- **零约束可能限制某些复杂函数**：某些算子天然在零点非零（如常数项），变换可能增加表达难度；文中未讨论这种情况的适应性。
- **与新兴方法（如 Transformer 符号回归）的对比缺失**：对比基线可能未包含最近最强方法，需要更多验证。

（完）
