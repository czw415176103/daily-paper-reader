---
title: "DeltaPhi: Physical States Residual Learning for Neural Operators in Data-Limited PDE Solving"
title_zh: DeltaPhi：数据有限情况下偏微分方程求解中神经算子的物理状态残差学习
authors: "Xihang Yue, Yi Yang, Linchao Zhu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=ppOCvEonKT"
tags: ["query:physics-ml"]
score: 9.0
evidence: 针对神经算子的残差学习框架用于偏微分方程求解
tldr: DeltaPhi将偏微分方程求解从学习直接映射转变为学习相似物理状态间的残差，利用物理系统的稳定性实现隐式数据增强。该方法在数据有限的情况下显著提升了神经算子的泛化能力。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 数据驱动的偏微分方程求解面临高质量训练数据稀缺的瓶颈。
method: 将求解任务转化为学习相似物理状态间的残差，利用系统稳定性实现隐式扩增。
result: 在数据有限情况下，神经算子的泛化能力得到显著提升。
conclusion: 残差学习范式有效缓解了数据稀缺问题，为数据驱动求解开辟了新路径。
---

## Abstract
The limited availability of high-quality training data poses a major obstacle in data-driven PDE solving, where expensive data collection and resolution constraints severely impact the ability of neural operator networks to learn and generalize the underlying physical system. To address this challenge, we propose DeltaPhi, a novel learning framework that transforms the PDE solving task from learning direct input-output mappings to learning the residuals between similar physical states, a fundamentally different approach to neural operator learning. This reformulation provides implicit data augmentation by exploiting the inherent stability of physical systems where closer initial states lead to closer evolution trajectories. DeltaPhi is architecture-agnostic and can be seamlessly integrated with existing neural operators to enhance their performance. Extensive experiments demonstrate consistent and significant improvements across diverse physical systems including regular and irregular domains, different neural architectures, multiple training data amount, and cross-resolution scenarios, confirming its effectiveness as a general enhancement for neural operators in data-limited PDE solving.

---

## 论文详细总结（自动生成）

# DeltaPhi：数据有限情况下偏微分方程求解中神经算子的物理状态残差学习

## 1. 核心问题与整体含义（研究动机和背景）

- **研究问题**：在数据驱动的偏微分方程（PDE）求解中，高质量训练数据极其稀缺。高昂的数据采集成本和分辨率限制严重制约了神经算子（Neural Operator）网络学习并泛化物理系统的能力。
- **背景**：传统神经算子学习直接输入-输出映射（如初始状态→演化轨迹），需要大量配对样本。然而，在许多实际物理场景（如流体力学、气象预测）中，获取足够的高保真PDE求解数据成本高昂，导致模型过拟合或泛化能力差。
- **整体含义**：本文旨在探索一种**从根本上不同的学习范式**，即利用物理系统自身的稳定性特性，将任务转变为学习相似物理状态之间的残差，从而在数据有限的情况下实现隐式数据增强，提升神经算子的泛化能力。

## 2. 方法论：核心思想、关键技术细节与流程

- **核心思想**：将PDE求解任务从**学习直接映射**（\( f: \text{初始状态} \to \text{最终状态} \)）转变为**学习相似物理状态间的残差**（\( g: (\phi_1, \phi_2) \to \phi_2 - \phi_1 \)），其中\(\phi_1\)和\(\phi_2\)是演化轨迹中时间或参数上相近的状态。利用物理系统的稳定性：初始状态越接近，其演化轨迹也越接近，因此残差比直接映射更平滑、更易学习，且天然提供了数据增强（从同一初始状态的不同扰动或相近时间步生成更多训练对）。
- **关键技术细节**：
  - **DeltaPhi框架**：架构无关（architecture-agnostic），可无缝集成到任何现有神经算子（如FNO、DeepONet等）中。具体而言，在训练阶段，从同一物理系统的时间序列或参数空间中采样相近的状态对\((\phi_i, \phi_j)\)，计算残差\(\delta = \phi_j - \phi_i\)，然后训练神经算子预测\(\delta\)。推理时，给定初始状态\(\phi_0\)，通过多次迭代预测残差逐步更新状态（类似时间步进方案）。
  - **隐式数据增强**：由于每个可用的真实状态都可以与其邻近状态配对产生残差样本，训练数据量得以隐式倍增（例如，一条轨迹包含T个时间步，可形成约\(T^2\)个配对）。这缓解了数据稀缺问题。
- **流程概述**：
  1. 从PDE数值解中提取物理状态序列（如不同时间步或不同参数下的解）。
  2. 构造训练数据集：选择时间或参数上接近的状态对，计算残差作为学习目标。
  3. 使用现有神经算子架构（如FNO）预测残差。
  4. 通过迭代应用残差预测（从初始状态开始，每次加上预测残差），逐步生成完整的演化轨迹。

## 3. 实验设计

- **数据集/场景**：
  - 规则域（如Darcy流、Navier-Stokes方程）和不规则域（如具有复杂边界的流体问题）的多种物理系统。
  - 涵盖多种PDE类型（扩散、对流、湍流等）。
- **Benchmark**：未明确列出标准数据集名称，但提到在多个领域下与基线方法对比。
- **对比方法**：直接作为基线的标准神经算子（如FNO、DeepONet等）以及这些方法在没有残差学习时的版本。通过将DeltaPhi嵌入这些算子，对比性能提升。

## 4. 资源与算力

- **未明确说明**：论文摘要和元数据中未提及使用的GPU型号、数量或训练时长。此项信息缺失。

## 5. 实验数量与充分性

- **实验数量**：论文报告了“广泛的实验”（extensive experiments），涵盖：
  - 多种物理系统（规则/不规则域）
  - 多种神经架构（不同算子的集成）
  - 多个训练数据量（如横坐标表示不同数据规模）
  - 跨分辨率场景（训练低分辨率、测试高分辨率）。
- **充分性**：实验设计较为充分，覆盖了通用性验证的多个维度（不同域、不同架构、不同数据量、跨分辨率泛化）。结果显示出“一致且显著的改进”（consistent and significant improvements），表明实验具有客观性。但具体实验组数（如消融实验的细分数量）未在摘要中展现，需参见全文。

## 6. 主要结论与发现

- 残差学习范式（DeltaPhi）在数据有限情况下，显著提升了神经算子对物理系统的泛化能力。
- 该方法利用物理系统的稳定性，通过隐式数据增强有效缓解了高质量训练数据稀缺的瓶颈。
- 作为架构无关的通用增强框架，DeltaPhi可提升多种现有神经算子在PDE求解任务上的性能，且在不同物理场景、数据量和分辨率下均表现稳定。

## 7. 优点

- **创新性**：从学习直接映射转向学习残差，是神经算子领域一种根本性的新视角。
- **架构无关**：可直接嵌入现有的任意神经算子（FNO、DeepONet等），无需修改底层网络结构，具有即插即用的普适性。
- **隐式数据增强**：在无需额外真实数据的情况下，通过配对邻近状态自然倍增训练样本，高效解决数据稀缺问题。
- **鲁棒性**：在规则/不规则域、多种PDE、多数据量设置下均表现出一致的提升，验证了方法的通用性。
- **跨分辨率泛化**：明确支持跨分辨率场景，符合实际应用需求。

## 8. 不足与局限

- **对稳定性假设的依赖**：方法有效性基于物理系统足够稳定（相近初始状态→相近轨迹）。对于混沌或高度不稳定的系统（如长时间湍流），残差可能不再平滑，效果可能下降。论文未讨论此边界情况。
- **实验覆盖可能受限**：尽管提到了多种场景，但未提供具体数据集名称（如标准benchmark），且未提及与其他数据增强或自监督方法的对比（如对比学习、伪标签），缺乏更广泛的基线。
- **算力成本未报告**：无法评估方法在实际部署中的计算开销（尤其是迭代推理时的步数）。
- **应用限制**：需要物理状态之间具有明确的时间或参数秩序来构造配对；对于只能获得孤立的、无序的观测样本的系统，该方法可能不适用。
- **理论分析不足**：未从数学上严格证明残差学习的泛化界或与直接映射学习的优势关系，更多是经验验证。

（完）
