---
title: Discovering Scaling Exponents with Physics-Informed Müntz-Szász Networks
title_zh: 使用物理信息Muntz-Szasz网络发现缩放指数
authors: "Gnankan Landry Regis N'guessan, Bum Jun Kim"
date: 2026-04-30
pdf: "https://openreview.net/pdf/11c0664aaa263c89ab85f264b35e3cb024501260.pdf"
tags: ["query:physics-ml"]
score: 9.0
evidence: 使用Muntz-Szasz网络的物理信息神经网络发现缩放指数
tldr: "标准神经网络隐式处理幂律缩放，本文提出MSN-PINN，将缩放指数作为可训练参数，输出解及其缩放结构。理论证明了可辨识性，实验显示在噪声和稀疏采样下单指数恢复误差1-5%，拉普拉斯方程角点奇异指数恢复误差0.009%。该方法为物理系统缩放规律发现提供了新工具。"
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 标准神经网络无法显式学习物理系统的幂律缩放指数。
method: 提出了Muntz-Szasz物理信息网络，将缩放指数作为可训练参数，并基于幂律基函数构建网络。
result: "在实验中，单指数恢复误差1-5%，拉普拉斯方程角点奇异指数误差仅0.009%。"
conclusion: 该方法能高精度恢复缩放指数，并具有可辨识性理论保证。
---

## Abstract
Physical systems near singularities, interfaces, and critical points exhibit power-law scaling, yet standard neural networks leave the governing exponents implicit. We introduce physics-informed Müntz-Szász Networks (MSN-PINN), a power-law basis network that treats scaling exponents as trainable parameters. The model outputs both the solution and its scaling structure. We prove identifiability, or unique recovery, and show that, under these conditions, the squared error between learned and true exponents scales as $O(|\mu - \alpha|^2)$. Across experiments, MSN-PINN achieves single-exponent recovery with 1–5\% error under noise and sparse sampling. It recovers corner singularity exponents for the two-dimensional Laplace equation with 0.009\% error, matches the classical result of Kondrat'ev (1967), and recovers forcing-induced exponents in singular Poisson problems with 0.03\% and 0.05\% errors. On a 40-configuration wedge benchmark, it reaches a 100\% success rate with 0.022\% mean error. Constraint-aware training encodes physical requirements such as boundary condition compatibility and improves accuracy by three orders of magnitude over naive training.

---

## 论文详细总结（自动生成）

### 论文的中文详细总结

#### 1. 核心问题与整体含义（研究动机和背景）
- **动机**：物理系统在奇点、界面和临界点附近表现出幂律缩放行为，但标准神经网络（如PINN）隐式地处理这些缩放指数，无法显式地学习和恢复控制物理行为的缩放指数。
- **意义**：发现和量化缩放指数对于理解物理系统的本质（如应力集中、相变临界指数、角点奇异解）至关重要。传统的数值方法依赖于理论推导或数值渐近分析，而本文尝试使用神经网络直接学习这些指数，为物理规律发现提供新工具。

#### 2. 方法论：核心思想、关键技术与公式/算法流程
- **核心思想**：提出**物理信息Müntz-Szász网络（MSN-PINN）**，一种基于幂律基函数的神经网络，将缩放指数作为可训练参数，同时输出方程的解及其缩放结构。
- **关键技术细节**：
  - 网络架构使用Müntz-Szász多项式基（一种完备的幂律函数族）代替标准激活函数或傅立叶基，使得缩放指数成为网络参数的一部分。
  - 结合物理信息损失函数（偏微分方程残差、边界条件、初始条件），通过梯度下降同时优化缩放指数和网络权重。
  - **可辨识性理论**：证明在一定条件下，缩放指数可以被唯一恢复（identifiability），且学习到的指数与真实指数之间的平方误差为 \( O(|\mu - \alpha|^2) \)。
  - **约束感知训练**：编码物理需求（如边界条件兼容性），显著提高精度。
- **算法流程**（文字描述）：
  1. 定义幂律基函数族，参数化未知缩放指数。
  2. 构建MSN网络输出形式为基函数的线性组合。
  3. 将缩放指数加入可训练参数集。
  4. 构造包含PDE残差、边界/初始条件、以及可能的正则化项的损失函数。
  5. 使用梯度下降法联合优化网络权重和缩放指数。
  6. 训练后直接从网络参数中读取缩放指数。

#### 3. 实验设计：数据集/场景、Benchmark、对比方法
- **使用的场景/数据集**：
  - 单指数恢复：在噪声和稀疏采样下测试单一幂律指数恢复精度（误差1-5%）。
  - 二维拉普拉斯方程角点奇异指数：恢复Kondrat'ev（1967）经典结果，误差0.009%。
  - 奇异泊松问题（受迫振动）：恢复由强迫项导致的指数，误差0.03%和0.05%。
  - 楔形区域基准（40种配置）：达到100%成功率，平均误差0.022%。
- **Benchmark**：没有明确提及其他基线方法，但对比了“朴素训练”（未使用约束感知训练），结果显示约束感知训练将精度提高了三个数量级。
- **对比方法**：仅内部对比（约束感知 vs 朴素训练），未与其他文献方法（如传统PINN、稀疏辨识方法等）进行定量对比。

#### 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量或训练时长。在论文的简短摘要和元数据中均未提及算力需求，因此无法总结。可能作者未做详细披露。

#### 5. 实验数量与充分性
- **实验数量**：共提及5个主要实验场景：单指数恢复（1-5%误差）、拉普拉斯角点（0.009%）、两项奇异泊松问题（0.03%和0.05%）、40配置楔形基准（100%成功率）。其中楔形基准算是一次大规模测试（40种配置）。
- **消融实验**：仅提到“约束感知训练”vs“朴素训练”的对比，可以视为一种消融。未提及对其他超参数（如网络深度、宽度、基函数数量）的消融。
- **充分性评价**：实验覆盖了不同类型的缩放现象（单指数、角点奇异、受迫奇异），精度非常高。但缺乏与现有方法（如传统PINN、物理信息基函数展开、稀疏回归等）的系统性对比，且未在更复杂的问题（如多指数耦合、高维问题）上验证。总体来看，实验设计侧重于展示方法能力，但公平性和全面性仍有提升空间。

#### 6. 主要结论与发现
- MSN-PINN能够高精度地恢复物理系统的缩放指数，在多种问题中达到<1%的相对误差。
- 可辨识性理论得到验证，即训练过程中缩放指数可以被唯一且准确地学习。
- 约束感知训练显著提升精度（三个数量级），对于边界条件兼容性的物理先验编码至关重要。
- 方法在噪声和稀疏采样下依然保持鲁棒（单指数恢复误差仅1-5%）。

#### 7. 优点
- **理论贡献**：提供了缩放指数可辨识性的理论保证，并给出误差上界，这是现有PINN方法通常缺乏的。
- **方法创新**：将Müntz-Szász基函数引入神经网络，直接以参数形式处理缩放指数，相比隐式处理更具可解释性。
- **实验精度**：在拉普拉斯角点奇异问题中误差极低（0.009%），媲美甚至超过经典数值方法的精度。
- **约束感知训练**：有效融合物理领域知识，是提高可训练性和精度的实用技巧。

#### 8. 不足与局限
- **实验覆盖有限**：仅测试了少数特定问题（拉普拉斯、泊松、楔形），未涉及更复杂的非线性PDE（如Navier-Stokes、相场方程）或多指数耦合情况。
- **缺乏外部对比**：未与其他机器学习或数值方法（例如标准PINN、稀疏识别、渐近匹配方法）进行公平定量对比，削弱了结论的普遍性。
- **算力与效率分析缺失**：未报告训练时间、网络规模及收敛速度，难以评估方法的实际计算成本。
- **泛化风险**：幂律基函数假设限制了模型仅适用于具有幂律缩放行为的系统，对于非幂律或混合缩放（如对数修正）可能不适用。
- **可复现性**：论文内容过于简短，缺乏详细的网络架构、损失函数权重、优化器设置等，会使得后续复现困难。

（完）
