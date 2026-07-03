---
title: Towards Coordinate- and Dimension-Agnostic Machine Learning for Partial Differential Equations
title_zh: 面向偏微分方程的坐标与维度无关的机器学习
authors: "George A Kevrekidis, Trung V. Phan, Soledad Villar, Yannis Kevrekidis, Juan M. Bello-Rivas"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=CC2vIx3GZM"
tags: ["query:physics-ml"]
score: 9.0
evidence: 坐标和维度无关的机器学习用于PDE识别，与物理信息ML相关
tldr: 现有数据驱动PDE学习方法依赖固定坐标和维度，无法泛化。本文提出坐标和维度无关的表示方法，在低维学习PDE后可推广到高维流形。该方法推动物理信息机器学习向空间解放方向发展。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 传统PDE学习方法无法泛化到不同坐标或维度空间。
method: 使用坐标与维度无关的表示来学习PDE，摆脱对固定坐标系的依赖。
result: 在低维数据上学习PDE，成功推广到高维流形。
conclusion: 空间解放的PDE学习为科学发现提供更强泛化能力。
---

## Abstract
Machine learning methods for data-driven identification of partial differential equations (PDEs) are typically defined for a fixed number of spatial dimensions and a particular choice of coordinates in which the data have been collected. This dependence prevents the learned equation from generalizing to other spaces. In this work, we reformulate the problem in terms of coordinate- and dimension- independent representations, paving the way toward what we might call “spatially liberated" PDE learning. In this work, we propose an approach to learning PDEs by expressing them in a way that is independent of the coordinate system and even the underlying manifold where the equation is defined. This allows us to learn a PDE in low-dimensional spaces and generalize to higher-dimensional spaces with different
geometric properties. We provide extensive numerical experiments that demonstrate that our approach allows for robust transferability across various geometric contexts. We show that the dynamics learned in one space can be used, without retraining, to make accurate predictions in other spaces with different dimensions, coordinate systems, boundary conditions, and curvatures, by recomputing invariant
features.

---

## 论文详细总结（自动生成）

# 基于论文《Towards Coordinate- and Dimension-Agnostic Machine Learning for Partial Differential Equations》的详细总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：现有的数据驱动偏微分方程（PDE）识别方法通常假设数据在**固定空间维度**和**特定坐标系**下采集，导致学到的方程无法泛化到其他几何空间（如不同维度、不同坐标系的流形）。
- **整体含义**：为了突破这一局限，本文追求“空间解放”（spatially liberated）的 PDE 学习——即学习到的方程应独立于坐标系和流形，从而可以在低维空间训练后直接推广到高维或不同几何结构的问题中。这为物理信息机器学习在更广泛场景下的应用奠定了基础。

## 2. 方法论：核心思想、关键技术细节与流程
- **核心思想**：将 PDE 学习问题重新表述为**坐标与维度无关的表示**（coordinate- and dimension-independent representations），使得方程的表达形式不依赖于数据采集时的特定坐标系，甚至不依赖于方程定义所在的底层流形。
- **关键技术细节**：
  - 利用**不变特征**（invariant features）来描述动力学，这些特征在不同坐标系和维度下保持一致性。
  - 在低维流形上学习 PDE 的表示，然后通过**重新计算不变特征**（recomputing invariant features）将该表示迁移到高维或不同曲率的流形，无需重新训练。
- **流程**（文字描述）：
  1. 对低维空间数据提取与坐标无关的特征。
  2. 基于这些特征学习 PDE 的进化规律（如算子或系数）。
  3. 面对目标高维或不同几何的空间，同样提取不变特征，并直接应用已学到的规律进行预测。

## 3. 实验设计
- **使用的数据集/场景**：文中仅提及“extensive numerical experiments”，未列出具体数据集名称，但说明涵盖了：
  - 不同维度的空间（低维→高维）
  - 不同坐标系（如笛卡尔、极坐标等）
  - 不同边界条件
  - 不同曲率的流形（如平面、球面）
- **Benchmark**：未明确提及特定的标准 benchmark 数据集或问题。
- **对比方法**：文中未列出任何对比方法（可能是因为该方向较新，尚无直接可比的方法）。

## 4. 资源与算力
- **未明确说明**：论文未提及使用的 GPU 型号、数量、训练时长等算力信息。实验中可能使用了常规的计算资源，但作者未给出具体细节。

## 5. 实验数量与充分性
- **实验数量**：文中仅泛泛描述“extensive numerical experiments”，没有列出具体组数或消融实验配置。
- **充分性与客观性**：
  - **不足**：缺乏与现有方法的定量对比，没有消融实验，也没有提供误差分析或统计显著性指标。实验覆盖的多样性（不同维度、坐标、曲率等）表明有探索性，但细节缺失使得实验的可复现性和说服力有限。
  - **公平性**：由于未与其他方法比较，难以判断其相对优势。

## 6. 主要结论与发现
- 所提出的坐标与维度无关方法可以实现**稳健的跨几何迁移**。
- **核心发现**：在某一空间（如低维、平直）学习到的动力学，**无需重新训练**，仅通过重新计算不变特征，即可在具有不同维度、坐标系、边界条件和曲率的其他空间中进行准确预测。

## 7. 优点
- **方法创新性强**：首次系统性地提出将 PDE 学习与坐标、维度解耦，推动了物理信息机器学习向“空间解放”方向发展。
- **泛化潜力大**：一旦在简单空间上训练，便可适用多种复杂几何环境，大幅降低训练成本。
- **概念清晰**：核心思想“不变特征”直观且符合物理直觉，可能具有较好的可解释性。

## 8. 不足与局限
- **实验细节匮乏**：未展示具体数据生成方式、仿真参数、误差度量等，难以评估结果可靠性。
- **缺乏对比基准**：没有与任何现有数据驱动 PDE 学习方法（如 PDE-Net、PINN 等）进行比较，无法体现性能优劣。
- **未讨论计算开销**：训练和推理时的复杂度、对高维流形是否仍高效等问题未涉及。
- **应用限制**：仅通过“重新计算不变特征”实现迁移，可能要求目标空间的不变特征可计算，对非常规或离散几何（如图形、点云）可能不适用。
- **被拒可能原因**：虽然 score 高（9.0），但 ICLR 2026 拒稿，推测评审可能认为方法验证不够充分、实验不够扎实，或与现有文献的联系不够紧密。

（完）
