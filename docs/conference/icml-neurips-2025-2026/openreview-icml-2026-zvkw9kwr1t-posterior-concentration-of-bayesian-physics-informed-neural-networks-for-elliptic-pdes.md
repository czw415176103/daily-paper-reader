---
title: Posterior Concentration of Bayesian Physics-Informed Neural Networks for Elliptic PDEs
title_zh: 贝叶斯物理信息神经网络求解椭圆型PDE的后验压缩性
authors: "Yuxuan Zhao, Yulong Lu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/ea90e300def3fce551c851ff2833767f1bd42d9d.pdf"
tags: ["query:physics-ml"]
score: 8.0
evidence: 研究贝叶斯PINN求解椭圆PDE的后验压缩率，适用于正反问题
tldr: 针对贝叶斯PINN在椭圆PDE中的理论性质进行研究，证明了在合适的先验下，后验分布以接近最优的速率压缩到真实解，且该先验是率自适应的，无需预先知道解的平滑性。这一结果为贝叶斯PINN在正反问题中的不确定性量化提供了坚实的理论保证。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 贝叶斯PINN的理论收敛性质尚不明确，影响其可靠性。
method: 构造了适合神经网络的先验，并推导了椭圆PDE解的后验压缩率。
result: 证明后验以近乎最优的速率收敛到真实解，且先验自适应平滑度。
conclusion: 该理论为贝叶斯PINN在科学计算中的严谨应用奠定了基础。
---

## Abstract
We study the posterior contraction rate of Bayesian Physics-Informed Neural Networks (PINNs) for solving a general class of elliptic partial differential equations (PDEs). We focus on learning of the elliptic equation with a non-homogeneous Dirichlet boundary condition from independent and noisy measurements collected both inside the domain and on the boundary. Assuming that the PDE admits a  strong solution in a  Hölder space and using with a suitably constructed prior on the neural network weights, we prove that the posterior distribution  concentrates around the exact solution at a near-minimax rate. Furthermore, the chosen prior is  *rate-adaptive*: the posterior contracts at an (almost) optimal rate without prior knowledge of the  smoothness level of the exact solution. Our results provide statistical guarantees for uncertainty quantification of PDEs via Bayesian PINNs.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：贝叶斯物理信息神经网络（Bayesian PINNs）在求解偏微分方程（PDEs）时，能够提供不确定性量化，但其理论收敛性质尚不明确，影响了方法在实际科学计算中的可靠性和可信度。
- **核心问题**：对于椭圆型PDE（非齐次Dirichlet边界条件），当从域内和边界收集独立带噪声的观测数据时，贝叶斯PINN的后验分布是否能够以接近最优的速率收缩到真实解？以及该速率能否在不依赖解平滑度先验知识的情况下自动适应？
- **整体含义**：本文为贝叶斯PINN提供了严格的后验压缩率理论保证，证明其在正问题和反问题中均能实现近乎最优的不确定性量化，为后续在物理科学、工程领域的严谨应用奠定了数学基础。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：在神经网络权重上构造合适的先验分布，使得后验分布能以接近极小极大最优的速率集中于真实解，且该先验是“率自适应”的——无需预先知道真实解的Hölder光滑性水平。
- **关键技术细节**：
  - 假设PDE存在Hölder空间中的强解。
  - 先验的构造方式：对神经网络权重赋予适当先验，使网络函数空间具有合适的逼近能力和正则化效果。
  - 利用贝叶斯公式得到后验分布，并通过理论推导证明后验压缩率的上界。
  - 推导过程中结合了PDE的变分形式、神经网络的逼近理论以及贝叶斯非参数统计中的后验压缩率分析方法。
- **算法流程（文字说明）**：
  1. 定义椭圆PDE正问题：给定PDE和边界条件，收集域内和边界上的独立噪声观测。
  2. 构造先验：在神经网络参数上选用合适的分布（例如高斯过程或层次先验）。
  3. 定义似然：基于PDE残差和边界条件残差建立似然函数，通常使用高斯噪声模型。
  4. 计算后验：通过MCMC或变分推断得到近似后验（本文理论工作未指定具体算法）。
  5. 证明后验压缩率：利用先验的逼近性质和PDE的正则性，推导后验概率质量向真实解收缩的速度。

## 3. 实验设计

- **数据集/场景**：论文为纯理论分析工作，未涉及具体数值实验或仿真数据。
- **Benchmark**：无实验基准比较。
- **对比方法**：无实验对比。文中理论结果与极小极大最优速率进行比较。

## 4. 资源与算力

- **未明确说明**：论文为理论性质研究，没有报告任何GPU型号、数量、训练时长等计算资源信息。推测不涉及大规模数值实验。

## 5. 实验数量与充分性

- **实验数量**：零组实验。论文章节全部为数学推导和证明。
- **充分性与公平性**：由于是理论工作，无需实验验证。但理论结果的充分性体现在证明的严谨性和假设的合理性上。作者在经典Hölder空间框架下给出了完整的后验压缩率上界，且证明了自适应性质，理论充分。

## 6. 论文的主要结论与发现

- 在适当构造的先验下，贝叶斯PINN的后验分布以近乎极小极大最优的速率压缩到椭圆PDE的真实解。
- 该先验是率自适应的：后验收缩速率自动适应真实解的Hölder光滑性水平，无需先验知晓该光滑性参数。
- 这一结果为贝叶斯PINN在正问题和反问题中的不确定性量化提供了理论保障，表明该方法在统计意义上可靠。

## 7. 优点

- **理论贡献突出**：首次为贝叶斯PINN建立了后验压缩率的上界，填补了该领域理论空白。
- **率自适应性质**：先验无需人工调参（如光滑性水平），提高了方法的实用性和鲁棒性。
- **覆盖正反问题**：不仅适用于PDE正问题，也为反问题的不确定性量化提供了理论基础。
- **证明框架通用**：针对一般椭圆型PDE，具有较大推广潜力。

## 8. 不足与局限

- **仅理论无实验**：缺乏数值实验验证理论结果（如有限样本下的收敛行为、不同先验选择的实际表现）。
- **假设较强**：要求PDE存在Hölder空间中的强解，且噪声为独立同分布高斯类型，实际应用中可能偏离。
- **未涉及计算可行性**：理论后验压缩率假设后验精确计算/采样，实际MCMC或变分推断的近似误差未被讨论。
- **局限于椭圆型PDE**：未扩展到其他类型PDE（如抛物型、双曲型）或更复杂的耦合系统。

（完）
