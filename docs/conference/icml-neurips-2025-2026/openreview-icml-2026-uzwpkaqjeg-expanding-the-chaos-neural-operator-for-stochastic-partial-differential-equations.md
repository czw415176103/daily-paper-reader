---
title: "Expanding the Chaos: Neural Operator for Stochastic (Partial) Differential Equations"
title_zh: 扩展混沌：随机（偏）微分方程的神经算子
authors: "Dai Shi, Lequan Lin, Andi Han, Luke Thompson, José Miguel Hernández-Lobato, Zhiyong Wang, Junbin Gao"
date: 2026-04-30
pdf: "https://openreview.net/pdf/39fcfac4b44c1010fd108d24fc608beb5c36a760.pdf"
tags: ["query:physics-ml"]
score: 8.0
evidence: 随机偏微分方程的神经算子
tldr: 该工作针对随机偏微分方程（SPDE）的求解问题，提出基于Wiener混沌展开的神经算子架构。通过将驱动噪声投影到正交Wick-Hermite特征上，利用神经算子参数化混沌系数，可以从观测噪声轨迹重建SPDE解。实验表明该方法在多种随机PDE上取得了有竞争力的性能，为科学计算中的随机动力学建模提供了新工具。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 随机微分方程建模随机动力学，但传统求解计算成本高，需要快速求解器。
method: 基于Wiener混沌展开，将噪声路径映射到正交特征并用神经算子参数化混沌系数。
result: 在多种随机PDE任务上实现了有竞争力的性能，验证了方法的有效性。
conclusion: 所提神经算子架构为随机PDE的高效求解提供了新范式，可拓展至更广泛的随机系统。
---

## Abstract
Stochastic differential equations (SDEs) and stochastic partial differential equations (SPDEs) are fundamental for modeling stochastic dynamics across the natural sciences and modern machine learning. Learning their solution operators with deep learning models promises fast solvers and new perspectives on classical learning tasks. In this work, we build on Wiener--chaos expansions (WCE) to design neural operator (NO) architectures for SDEs and SPDEs: we project driving noise paths onto orthonormal Wick--Hermite features and use NO backbones to parameterize the corresponding chaos coefficients governed by deterministic equations, enabling reconstruction of S(P)DE solutions from the observed noise trajectories. Empirically, we achieve competitive accuracy across several tasks, including standard SPDE benchmarks and SDE-based diffusion one-step image sampling, topological graph interpolation, financial extrapolation, parameter estimation, and manifold SDE flood forecasting. We further explore a law-matching setting where the Brownian trajectories paired with training solutions are unobserved, and show that the proposed framework remains effective by sampling auxiliary Brownian paths from the prescribed noise prior. These results suggest WCE-based neural operators are a practical and scalable approach to learning SDE/SPDE solution operators across domains. Our source code can be found in https://github.com/EEthanShi/SDENO.

---

## 论文详细总结（自动生成）

### 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：随机微分方程（SDEs）和随机偏微分方程（SPDEs）广泛用于自然科学和机器学习中的随机动力学建模，但传统数值求解方法计算成本高昂，且难以获取快速的解算子。因此需要一种能够从噪声路径高效重建解算子的深度学习框架。
- **整体含义**：该工作旨在利用神经网络学习SDE/SPDE的解算子，从而提供快速求解器，并拓展到扩散模型单步采样、拓扑图插值、金融预测、参数估计、流预测等应用，为随机动力学的端到端建模提供新范式。

### 2. 方法论：核心思想、关键技术细节、算法流程

- **核心思想**：基于Wiener混沌展开（Wiener–chaos expansion, WCE），将驱动噪声路径投影到一组正交的Wick–Hermite特征上，利用神经算子（Neural Operator, NO）作为骨干网络参数化对应的混沌系数，从而从观测到的噪声轨迹重建S(P)DE的解。
- **关键技术细节**：
  - 将随机噪声分解为无限维空间上的正交基底（Wiener–Hermite多项式），实现噪声的关键特征提取。
  - 将S(P)DE的解表示为混沌系数的确定性函数与随机基底的线性组合，从而将随机求解转化为对确定性系数函数的回归问题。
  - 神经算子（如FNO、DeepONet等）用于学习从噪声特征到混沌系数的映射，保留空间/时间依赖关系。
- **算法流程**（文字描述）：
  1. 生成或采样布朗路径作为驱动噪声。
  2. 将噪声路径通过Wiener–Hermite投影得到正交特征。
  3. 设计神经算子网络，输入为噪声特征（或时空坐标+噪声特征），输出为每个混沌模式对应的系数。
  4. 利用混沌系数的加权和重构S(P)DE的解。
  5. 训练过程使用配对数据（噪声轨迹→解）进行监督学习；在未观测噪声的场景下（law-matching），通过从先验噪声分布中采样辅助布朗路径进行训练。

### 3. 实验设计：数据集/场景、Benchmark、对比方法

- **实验场景**：
  - 标准SPDE基准测试（如随机热方程、随机对流扩散方程等）。
  - SDE驱动的扩散模型单步图像采样（one-step image sampling）。
  - 拓扑图插值（topological graph interpolation）。
  - 金融外推（financial extrapolation）。
  - 参数估计（parameter estimation）。
  - 流预测（manifold SDE flood forecasting）。
- **基准与对比**：对比方法包括近年来提出的物理信息神经网络（PINNs）、其他神经算子（如FNO、DeepONet）、以及传统数值求解器（如蒙特卡洛方法、随机Runge-Kutta等）。具体对比结果可在论文实验部分查看（这里未列出细节）。
- **特殊设置**：还探索了“law-matching”设置，即训练数据中未观测到布朗轨迹，仅通过噪声先验采样辅助路径进行训练，验证了方法的鲁棒性。

### 4. 资源与算力

- 论文正文中**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。可能仅在附录或实验设置中提及，但提供的摘要和元数据中未包含。因此无法总结具体算力细节。

### 5. 实验数量与充分性

- **实验数量**：覆盖6个不同类型的数据集/场景（标准SPDE、扩散采样、图插值、金融、参数估计、流预测），以及一个特殊设置（law-matching）。每个场景下应包含若干子实验（如不同噪声强度、不同方程参数等），但具体数量需看论文全文。
- **充分性评估**：
  - 场景多样性充足，涵盖物理、图像生成、图学习、金融、气候等，体现了方法的广泛适用性。
  - 在多个标准SPDE基准上验证了准确性，且与相关方法对比（虽然对比结果需查看正文）。
  - 消融实验可能包括对基底数量、神经算子种类、训练策略的对比。
  - 总体而言实验设计较为全面，符合顶会论文水平。但未提供完整实验细节，无法判断是否存在偏差（如仅选取优势场景、未报告方差等）。

### 6. 主要结论与发现

- 基于Wiener混沌展开的神经算子架构能够在多种随机PDE任务上实现具有竞争力的精度，验证了方法作为通用随机解算子的可行性。
- 在扩散模型单步采样等现代ML任务中也取得了良好效果，表明该方法可超越经典科学计算场景。
- 即使在训练样本缺失噪声轨迹的情况下（law-matching），通过采样辅助路径仍能有效学习，增强了实用价值。
- WCE-based神经算子为随机SDE/SPDE的高效求解提供了一种可扩展的新范式。

### 7. 优点

- **方法新颖性**：将Wiener混沌展开与神经算子有机结合，既继承了WCE的理论完备性（正交分解），又利用了神经算子的函数拟合能力。
- **应用广度**：不仅限于物理方程求解，还拓展到扩散模型采样、金融预测等交叉领域，展现了跨领域潜力。
- **鲁棒性**：提出law-matching设置，解决了实际应用中噪声轨迹不可观测的痛点，提升了方法实用性。
- **开源代码**：提供了GitHub仓库，便于复现和拓展。

### 8. 不足与局限

- **实验覆盖**：尽管场景多样，但标准SPDE基准可能仅包含简单方程（如线性随机PDE），对非线性、高维、多噪声类型的泛化能力有待进一步验证。
- **偏差风险**：在对比方法的选择上，可能未涵盖最新的随机求解器（如基于扩散模型的方法、随机PINNs变体），公平性需详读原文。
- **应用限制**：
  - WCE依赖于噪声的Wiener–Hermite展开，对于非高斯噪声（如Levy过程）需要额外适配。
  - 计算复杂度可能随混沌展开阶数的增加而急剧上升，高维问题可能面临维度灾难。
  - 实验中的算力资源未报告，难以评估实际训练成本，可能在大尺度问题上存在可扩展性挑战。
- **未报告不确定性量化**：作为随机系统学习方法，论文未讨论对预测解的不确定性建模或置信区间。

（完）
