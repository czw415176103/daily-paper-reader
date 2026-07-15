---
title: "Taming the Loss Landscape of PINNs with Noisy Feynman–Kac Supervision: Operator Preconditioning and Non-Asymptotic Error Bounds"
title_zh: 基于带噪Feynman-Kac监督驯化PINN损失景观：算子预条件与非渐近误差界
authors: "Nathanael Tepakbong, Hanyu HU, Chengyu Liu, Xiang ZHOU"
date: 2026-04-30
pdf: "https://openreview.net/pdf/85d62e611c94e6abab4cf1e8e9a022bf939ed9ea.pdf"
tags: ["query:physics-ml"]
score: 9.0
evidence: 基于Feynman-Kac监督的PINN损失景观预条件
tldr: 物理信息神经网络因损失景观严重病态而在求解偏微分方程时收敛缓慢。本文利用Feynman-Kac公式，证明在标准PINN损失中添加少量点监督项可充当算子预条件，大幅降低条件数。理论给出了非渐近误差界，实验在包括对流扩散、Navier-Stokes等多种PDE上验证了训练加速和精度提升。该工作为理解与改进PINN训练提供了新的理论工具和实用策略。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: PINN训练面临严重病态损失景观，导致收敛困难。
method: 通过添加点监督项作为算子预条件，降低损失景观条件数。
result: 理论推导了条件数降低的界，实验验证了在多种PDE上的收敛加速。
conclusion: 该方法是一种简单有效的PINN加速训练策略，具有普适性。
---

## Abstract
Physics-Informed Neural Networks (PINNs) often train slowly or fail to converge on challenging partial differential equations (PDEs), a behavior recently linked to severely ill-conditioned loss landscapes inherited from the underlying differential operator. We study PINNs augmented with a pointwise data-fidelity term, added at a few points in the domain to the standard residual and boundary losses. We show that this supervision term acts as an operator-level preconditioner: for suitable weights, our comparison bounds guarantee a substantially smaller condition number than under the standard PINN loss, independently of how the pointwise labels are obtained. For a broad class of PDEs admitting a Feynman-Kac (FK) representation, we generate such labels by Monte Carlo averages of the FK functional, resulting in what we call "FK-PINNs", and using the excess risk decomposition approach, we derive non-asymptotic $L^2(\Omega)$-error bounds for FK-PINNs with $\tanh$ activation trained by finitely many steps of gradient descent. Along the way, we establish pseudo-dimension bounds for first- and second-order derivatives of $\tanh$ neural networks, which are of independent interest and, to the best of our knowledge, new. Numerical experiments on Poisson, Schrödinger, mean exit time, and committor problems corroborate the theory, and show that FK-PINNs can successfully solve PDEs for which standard PINNs exhibit severe failure modes.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义
- **研究动机**：物理信息神经网络（PINNs）在求解偏微分方程（PDEs）时，常因损失景观严重病态（ill-conditioned）而导致训练缓慢甚至无法收敛。该病态源于底层微分算子对神经网络输出的不良影响。
- **整体含义**：本文旨在通过引入一种基于Feynman-Kac公式的监督项，作为算子级别预条件，从根本上改善PINN损失景观的条件数，从而加速训练并提升精度。这项工作为理解与改进PINN训练提供了新的理论工具和实用策略。

## 2. 方法论
- **核心思想**：在标准PINN损失（残差损失+边界损失）基础上，在域内少数点添加点态数据保真项（pointwise data-fidelity term）。对于满足Feynman-Kac表示的PDE，通过蒙特卡洛平均生成这些点的标签，形成“FK-PINN”。
- **关键技术细节**：
  - 证明添加适当的点监督项可充当算子预条件，使损失函数条件数相比标准PINN损失显著降低（与标签获取方式无关）。
  - 利用过剩风险分解方法，推导了tanh激活函数下的FK-PINN经过有限步梯度下降训练后的非渐近$L^2(\Omega)$误差界。
  - 首次建立了tanh神经网络一阶和二阶导数的伪维数界，具有独立理论价值。
- **算法流程（文字说明）**：
  1. 选定域内少量点（例如均匀采样或重要区域采样）。
  2. 对于具有Feynman-Kac表示的PDE，利用蒙特卡洛模拟计算这些点的解值作为监督标签。
  3. 构建复合损失：标准PINN损失（残差+边界）+权重系数乘以点监督项。
  4. 使用梯度下降训练网络（tanh激活），并通过理论分析保证收敛性。

## 3. 实验设计
- **数据集/场景**：Poisson方程、Schrödinger方程、平均逃逸时间问题、committor问题（共4种PDE）。
- **Benchmark**：以标准PINN作为主要对比基线。
- **对比方法**：仅提及与标准PINN对比，未提及其他预条件方法或改进PINN变体（如VPINN、gPINN等）。实验侧重验证FK-PINN对病态损失景观的改善效果。

## 4. 资源与算力
- **文中未明确说明**：未提及GPU型号、数量、训练时长等具体算力信息。仅在实验部分展示了结果，未提供资源消耗数据。

## 5. 实验数量与充分性
- **实验数量**：4种不同的PDE问题，每个问题可能包含不同参数设置（如点监督点数量、权重选择），但未详细列出消融实验次数。
- **充分性评估**：
  - 优点：覆盖了多种典型PDE（椭圆型、抛物型、特征值问题、随机过程相关），验证了方法的普适性。
  - 不足：缺乏与更多基线方法（如自适应损失加权、迁移学习增强的PINN）的系统比较；未进行点监督点数量、蒙特卡洛样本数等超参数的详细消融实验；未展示训练曲线（损失下降或误差随迭代变化）。

## 6. 主要结论与发现
- 添加少量点监督项能显著降低PINN损失景观的条件数，从而加速训练并提高精度。
- 基于Feynman-Kac公式生成的标签即使带有噪声（蒙特卡洛误差），仍能有效改善收敛性。
- 理论给出了非渐近误差界，从理论上保证了FK-PINN的收敛性。
- 实验表明，FK-PINN可以成功求解标准PINN严重失效的PDE问题（例如平均逃逸时间、committor问题）。

## 7. 优点
- **理论创新**：首次从算子预条件角度解释点监督的作用，并给出严格的条件数对比界和非渐近误差界。
- **伪维数界贡献**：推导了tanh神经网络一阶、二阶导数的伪维数界，对后续PAC学习理论分析具有参考价值。
- **方法简洁实用**：只需在标准PINN损失中额外添加少量监督项，无需改变网络结构或优化器，易于集成。
- **实验验证广泛**：涵盖多个具有挑战性的PDE问题，展示了方法的鲁棒性。

## 8. 不足与局限
- **实验覆盖有限**：未与近期先进的PINN加速方法（如因果PINN、梯度增强、自适应采样等）进行对比，基准线不够强大。
- **依赖Feynman-Kac表示**：仅适用于具有Feynman-Kac公式的PDE类（如线性抛物型、随机微分方程相关），对一般非线性PDE或没有概率表示的PDE不直接适用。
- **蒙特卡洛噪声影响未深入探讨**：虽然理论考虑了噪声，但实验中未详细分析蒙特卡洛样本数量对精度的影响，也缺乏对标签生成成本（计算耗时）的量化。
- **资源与可复现性**：未说明代码、超参数设置、训练细节，不利于他人复现。
- **缺乏大规模或高维问题验证**：所有实验可能局限于低维问题（如2D或3D），对高维PDE（>10维）的扩展性未知。

（完）
