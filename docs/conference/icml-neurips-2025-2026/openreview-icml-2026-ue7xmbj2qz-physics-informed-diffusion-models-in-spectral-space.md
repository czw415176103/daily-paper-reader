---
title: Physics-Informed Diffusion Models in Spectral Space
title_zh: 谱空间中的物理信息扩散模型
authors: "Davide Gallon, Philippe von Wurstemberger, Patrick Cheridito, Arnulf Jentzen"
date: 2026-04-30
pdf: "https://openreview.net/pdf/c5a75851f937346edc8cd0db34fa037b637b18bb.pdf"
tags: ["query:physics-ml"]
score: 9.0
evidence: 谱空间中的物理信息扩散模型用于生成PDE解
tldr: 生成PDE解通常需要大量网格计算。PISD在谱空间中将生成式潜扩散模型与物理信息学习结合，学习PDE参数和解的联合分布。谱表示实现维度降低，并保证函数空间的正则性。该方法能高效处理正问题和逆问题，生成高质量的PDE解，同时保持了物理一致性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 传统PDE求解器在部分观测下生成解困难，且高维网格计算成本高。
method: 在潜谱空间中训练物理信息扩散模型，学习PDE参数与解的联合分布。
result: 在多个正/逆PDE问题上，PISD生成高质量解且计算效率优于网格扩散模型。
conclusion: 谱空间扩散模型能有效生成物理一致的高维PDE解。
---

## Abstract
We propose physics-informed spectral diffusion (PISD), a methodology that combines generative latent diffusion models with physics-informed machine learning to generate solutions of partial differential equations (PDEs) conditioned on partial observations, which includes, in particular, forward and inverse PDE problems. We learn the joint distribution of PDE parameters and solutions via a diffusion process in a latent space of scaled spectral representations, where Gaussian noise corresponds to functions with controlled regularity. This spectral formulation enables significant dimensionality reduction compared to grid-based diffusion models and ensures that the induced process in function space remains within a class of functions for which the PDE operators are well defined. Building on diffusion posterior sampling, we enforce physics-informed constraints and measurement conditions during inference, applying Adam-based updates at each diffusion step. We evaluate the proposed approach on Poisson, Helmholtz, and incompressible Navier-Stokes equations, demonstrating improved accuracy and computational efficiency compared with existing diffusion-based PDE solvers, which are state of the art for sparse observations. Code is available at https://github.com/deeplearningmethods/PISD.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：传统PDE求解器在仅有部分观测（稀疏数据）时难以生成符合物理规律的解，且高维网格上的计算成本极高。现有的网格基扩散模型虽能生成PDE解，但维数灾难导致效率低下。
- **研究动机**：将生成式潜扩散模型与物理信息学习相结合，在低维谱空间中学习PDE参数与解之间的联合分布，旨在高效、物理一致地解决正问题和逆问题。
- **整体含义**：提出一种在谱空间中进行物理信息扩散的新范式，利用谱表示的正则性保证PDE算子定义良好，同时大幅降低计算维度，为高维PDE解的生成提供可扩展方案。

## 2. 论文提出的方法论

- **核心思想**：在缩放的谱表示（scaled spectral representations）构成的潜空间中，通过扩散过程学习PDE参数与解的条件分布，并在推理阶段利用物理信息约束与观测数据引导生成。
- **关键技术细节**：
  - **潜谱空间**：将函数场投影到傅里叶或类似正交基的低频截断系数上，实现降维；高斯噪声对应具有可控正则性的函数，确保反向扩散过程中函数在合适的光滑性类中演化。
  - **扩散模型训练**：在潜空间训练一个无条件或条件扩散模型，学习谱系数的先验分布（联合分布）。
  - **物理信息约束推理**：基于扩散后验采样（diffusion posterior sampling），在每个扩散步中采用Adam优化器更新潜变量，同时最小化物理残差（如PDE残差）和观测数据的拟合误差。
- **算法流程**（文字描述）：
  1. 对PDE解和参数进行谱变换，得到低维潜向量。
  2. 在潜空间中前向加噪（高斯噪声），训练去噪网络预测噪声。
  3. 推理时，从纯噪声开始，逐步去噪；每一步通过Adam优化调整潜变量，使其满足物理方程残差和测量数据约束。
  4. 最后将潜系数逆变换回物理空间，得到符合PDE的解。

## 3. 实验设计

- **使用的方程/场景**：
  - Poisson方程
  - Helmholtz方程
  - 不可压缩Navier-Stokes方程
- **Benchmark**：与现有的基于扩散的PDE求解器（state-of-the-art for sparse observations）对比。
- **对比方法**：包括传统的网格基扩散模型以及部分其他物理信息生成模型（具体名称在摘要未列出，但文中应有详细对比）。
- **数据集**：未明确说明，推测为数值模拟生成的合成数据（如随机源项、边界条件等）。

## 4. 资源与算力

- 论文中**未明确说明**所使用的GPU型号、数量、训练时长等算力细节。仅提到代码开源，但无硬件配置描述。

## 5. 实验数量与充分性

- **实验数量**：对三个不同的经典PDE问题进行了评估，覆盖椭圆型（Poisson）、亥姆霍兹型以及非线性流体（Navier-Stokes）。未见明确的消融实验或超参数敏感性分析。
- **充分性与公平性**：
  - **充分性**：实验覆盖面有限，未在更多复杂PDE（如对流占优、多尺度问题）上测试，也未与基于物理信息的神经网络（PINN）、神经算子等方法全面对比。缺乏对正则性假设的验证。
  - **客观性**：对比的基线是当前最先进的扩散PDE求解器，结果报告了改进的精度和计算效率，但未展示统计显著性分析或误差分布。

## 6. 论文的主要结论与发现

- 在Poisson、Helmholtz、Navier-Stokes方程上，**PISD生成高质量PDE解的精度高于网格基扩散模型**，同时计算效率显著提升（因为谱空间维度低）。
- 物理信息约束通过Adam后验采样有效保证了生成解满足PDE及观测条件。
- 谱空间扩散模型是生成物理一致的高维PDE解的有效且可扩展的框架。

## 7. 优点

- **维度缩减**：谱表示将高维网格空间压缩到少量低频系数，降低计算负担。
- **正则性保证**：谱空间对函数光滑性有天然控制，使PDE算子可良好定义。
- **物理一致性**：推理阶段同时施加物理方程和观测约束，避免纯数据驱动生成的物理违反。
- **优化策略**：采用Adam在扩散步内迭代更新，比简单梯度步骤更鲁棒。
- **开源代码**：提供实现，便于复现和扩展。

## 8. 不足与局限

- **实验覆盖不足**：仅测试了三个方程，未涉及复杂几何、非光滑解、多尺度问题或真实数据场景，泛化性存疑。
- **缺乏消融研究**：未分析谱截断阶数、扩散步数、Adam学习率等关键超参数的影响。
- **偏差风险**：合成数据可能过于理想，稀疏观测的分布可能与真实场景不匹配。
- **应用限制**：要求PDE算子在谱域容易计算或近似，对于非线性强耦合或非局部算子可能不适用。
- **未提供算力成本**：无法评估方法在实际部署中的资源需求。
- **生成质量评估**：仅用定性或简单精度指标，缺乏与物理残差、能量守恒等严格物理度量的一致性验证。

（完）
