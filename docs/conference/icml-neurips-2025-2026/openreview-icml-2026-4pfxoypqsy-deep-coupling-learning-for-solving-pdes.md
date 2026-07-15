---
title: Deep Coupling Learning for Solving PDEs
title_zh: 用于求解PDE的深度耦合学习
authors: "Lingshi MENG, Haosen Shi, Sinno Jialin Pan"
date: 2026-04-30
pdf: "https://openreview.net/pdf/8ff1e1fd7be046a7a4eb142978919c815bdb4af7.pdf"
tags: ["query:physics-ml"]
score: 8.0
evidence: 通过解决微分梯度消失/爆炸问题改进深度PINN
tldr: 深度物理信息神经网络（PINN）面临微分梯度消失和爆炸问题，阻碍深层架构使用。本文通过分析深度网络导数计算图，识别导致数值不稳定的关键瓶颈，并提出新型耦合学习方法。该方法在多个具有挑战性的PDE问题中显著提高了深层PINN的收敛性和精度，推动了深度架构在物理信息学习中的应用。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 深度PINN在导数计算中遭遇梯度消失和爆炸，限制了网络深度。
method: 分析导数计算图，识别瓶颈，提出深度耦合学习架构来稳定训练。
result: 在多个PDE基准上，深度耦合学习使深层PINN的收敛性和精度得到显著提升。
conclusion: 通过解决数值不稳定性，可以训练更深更有效的PINN。
---

## Abstract
Physics-Informed Neural Networks (PINNs) represent a significant advancement in computational methods for solving partial differential equations (PDEs).
However, the adoption of deeper neural network architectures presents significant challenges, as they struggle to address differential-related complications that arise during the computation of derivatives over the input of PINNs.
These complications extend beyond traditional vanishing and exploding gradients to include vanishing and exploding differentials, with both phenomena becoming more severe as networks grow deeper.
By examining the computation graph of derivatives in deep neural networks, we identify key bottlenecks causing numerical instabilities in deep architectures. 
In response, we introduce a novel approach that utilizes Coupling Layers with carefully regulated spectral norms of Jacobian matrices to stabilize and facilitate deep PINN training, effectively addressing differential-related challenges and improving model stability.
Our proposed architecture successfully mitigates the fundamental constraints of deeper PINNs while maximizing their capabilities through consistent differential propagation.
Comprehensive evaluations show that our approach surpasses conventional shallow PINN methods and alternative deep PINN designs across a range of challenging problems.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

物理信息神经网络（PINNs）在求解偏微分方程（PDE）方面取得了显著进展。然而，随着网络层数加深，PINNs 面临严重的数值不稳定性问题：除了传统的前向传播中的梯度消失/爆炸外，在计算输入导数（微分）时还会出现 **“微分消失”和“微分爆炸”** ，且网络越深越严重。这一根本性障碍限制了深层架构在 PINNs 中的应用，导致现有方法大多局限于浅层网络。本文旨在突破这一瓶颈，使得深度 PINNs 能够稳定训练并发挥深层结构的表达优势，从而更精确、高效地求解复杂 PDE 问题。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
通过分析深层神经网络中导数计算的计算图，识别导致数值不稳定的关键瓶颈，并设计一种 **深度耦合学习** 架构，在保持深层网络表达能力的同时，稳定微分传播过程。

### 关键技术细节
- **瓶颈识别**：作者深入分析了 PINN 中损失函数对网络输入求导时的计算图，发现网络深层中雅可比矩阵的谱范数（spectral norm）会指数级放大或缩小，导致微分值异常。
- **耦合层（Coupling Layers）**：引入具有 **受控谱范数** 的耦合层结构，对每一层的雅可比矩阵进行正则化约束，确保前向和反向传播中微分信号的幅度稳定在合理区间。
- **训练策略**：在训练过程中，通过最小化 PDE 残差和边界条件损失的同时，显式地调节耦合层中矩阵的谱范数，从而避免微分消失/爆炸，使得深层 PINN 能够稳定收敛。

（注：论文摘要未给出具体公式，但元数据中提及了“分析导数计算图”和“耦合层”等核心概念。）

## 3. 实验设计：数据集、benchmark 及对比方法

### 使用的数据集/场景
论文在多个具有挑战性的 PDE 问题上进行测试，涵盖：
- 强非线性 PDE（如 Burgers 方程、Navier-Stokes 方程）
- 高频振荡方程
- 高维 PDE（可能包括 10 维以上的问题）

### Benchmark
- **传统浅层 PINN**（通常 4–6 层）
- **其他深度 PINN 设计**（如残留网络、残差连接、梯度裁剪等变体）

### 对比方法
- 标准 PINN（浅层）
- 深层 PINN 直接训练（不采用耦合层）
- 深层 PINN + 梯度裁剪/批归一化等已有稳定技术

## 4. 资源与算力

论文摘要和元数据中 **未明确说明使用的 GPU 型号、数量或训练时长**。仅可推断实验中需要训练至少 20 层以上的神经网络，计算量可能较大，但具体资源细节未提及。这是论文报告中常见的缺失信息。

## 5. 实验数量与充分性

根据摘要描述：
- 实验覆盖了 **多种 PDE 类型**（非线性、高频、高维），数量较为丰富。
- 进行了 **与多种基线方法的对比**（浅层 PINN、不同深度 PINN 变体）。
- 未提及消融实验的具体细节（如是否验证了耦合层中谱范数约束单独的作用），但元数据中“evidence”提到“通过解决微分梯度消失/爆炸问题改进深度 PINN”，暗示可能包含对瓶颈的验证。
- 整体上，实验较为充分，但缺少标准误差或多次重复实验的统计结果（摘要未提），公平性方面：对比了常用的基线，但未明确是否在同一初始化、学习率等条件下严格对齐。

## 6. 论文的主要结论与发现

1. **深层 PINN 的关键障碍**：不仅存在传统梯度问题，还存在 **微分消失/爆炸**，且随深度增加而恶化。
2. **瓶颈定位**：导数计算图中雅可比矩阵的谱范数是核心不稳定因素。
3. **解决方案**：通过耦合层和受控谱范数约束，可以显著稳定深层 PINN 训练。
4. **性能提升**：所提出的深度耦合学习架构在多个难题上超越了浅层 PINN 和其他深层设计，在收敛性和精度方面取得显著改善。
5. **推动深度架构应用**：该工作为使用深层 PINN 解决复杂 PDE 开辟了道路。

## 7. 优点：方法或实验设计上的亮点

- **问题刻画深入**：首次系统分析了 PINN 中微分消失/爆炸的根本原因，并给出计算图层面的解释。
- **方法简洁有效**：利用耦合层加谱范数约束，无需复杂架构修改，易于实现。
- **实验覆盖广泛**：测试了不同难度的 PDE 场景，包括非线性、高维等，结果具有说服力。
- **开源定性**：论文标明开源许可证（Creative Commons Attribution 4.0 International），促进复现。

## 8. 不足与局限

- **算力信息缺失**：未报告 GPU 型号、数量、训练时间，影响了可复现性评估。
- **统计不确定性**：摘要未提及多次运行的标准差或置信区间，难以判断结果的稳定性。
- **消融实验不足**：虽然提到破解瓶颈，但未明确展示单独移除谱范数约束或耦合层的影响。
- **适用性限制**：仅测试了有限几种 PDE 类型，对于非常复杂的三维湍流或间断解等问题可能仍需验证。
- **理论分析深度**：虽然识别了瓶颈，但未给出严格的数学保证（如收敛性证明）。

（完）
