---
title: "CAED-Agent: an Agentic Framework to Automate Simulation-Based Experimental Design"
title_zh: CAED-Agent：自动化仿真实验设计的代理框架
authors: "Yang Zhang, Yadi Cao, Sophia Huiwen Sun, Rose Yu"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=nGihWDdQFI"
tags: ["query:cfd-agent"]
score: 4.0
evidence: 用于自动化物理仿真配置的代理框架
tldr: 配置物理仿真需要平衡精度与计算预算，传统方法在高维下失效。本文提出CAED-Agent，利用LLM自动化仿真实验设计，引入成本感知策略避免低效配置。实验表明，该方法在多个仿真场景中显著降低查询次数并找到更优配置，为智能代理在计算流体动力学等领域的应用提供了通用框架。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 物理仿真配置存在成本与精度的权衡，传统优化方法难以泛化。
method: 基于LLM的代理框架，结合成本感知采样与分配策略。
result: 在多个仿真任务中，在更低计算预算下找到更优配置。
conclusion: CAED-Agent为自动化仿真设计提供了高效、智能的解决方案。
---

## Abstract
Configuring physics-based simulations requires balancing granularity against computational budget, a dilemma we term **C**ost-**A**ware **S**imulation-Based **C**onfiguration **O**ptimization (CASCO). Traditional methods, such as Bayesian optimization or manual expert design, often struggle with the curse of high dimensionality or fail to generalize. Large Language Models (LLMs) offer promise for automating such workflows but, as we show experimentally, lack inherent cost awareness and frequently propose inefficient configurations. While inference-time scaling can improve the exploration width to find cost-efficient configurations, it demands prohibitively many simulator queries. We propose **C**ost-**A**ware **S**imulation **C**onfiguration **O**ptimization **Agent** (CASCO-Agent), an agentic framework guiding inference-time scaling via lightweight surrogates that predict low-dimensional metrics (accuracy, cost) rather than complete physics fields. This enables easier training and flexible adaptation to data availability, e.g., using Gaussian Processes in data-scarce regimes or Neural Networks when data is abundant. In experiments across 3 typical PDE solvers (elliptic, parabolic, and hyperbolic), CASCO-Agent consistently outperforms Bayesian optimization and LLM-based baselines, achieving success rates comparable to inference-time scaling with a ground truth simulator without incurring expensive simulation overhead.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义

- **研究动机**：物理仿真（如基于偏微分方程 PDE 的求解器）的配置需要在计算精度与计算预算之间权衡，高维配置空间下传统方法（如贝叶斯优化、手动专家设计）难以泛化或受限于维度灾难。
- **核心问题**：提出“成本感知仿真配置优化”（CASCO）这一新问题，旨在自动化找到最优仿真配置，同时考虑计算成本。
- **整体含义**：大型语言模型（LLM）有潜力自动化该流程，但缺乏成本感知能力，常提出低效配置。论文提出 CAED-Agent（实际文中应为 CASCO-Agent），利用轻量级代理引导 LLM 的推理时扩展，在避免昂贵仿真的前提下高效找到低成本高精度的配置。

## 2. 论文提出的方法论

- **核心思想**：构建一个基于 LLM 的代理框架（CASCO-Agent），通过轻量级代理模型（surrogate）预测仿真配置的精度和成本（低维指标，而非完整物理场），从而引导 LLM 的推理时探索。
- **关键技术细节**：
  - 代理模型：在数据稀缺时使用高斯过程（GP），数据充足时使用神经网络（NN）——灵活适配数据可用性。
  - 成本感知采样与分配策略：避免 LLM 盲目生成高成本配置，利用代理模型提前过滤无效或昂贵选项。
  - 将 LLM 作为“探索-利用”的决策者，结合代理模型的反馈进行推理时扩展。
- **公式与算法流程**（文字说明）：
  - 输入：初始仿真配置空间、成本约束。
  - 循环迭代：
    1. LLM 生成候选配置；
    2. 轻量级代理模型预测该配置的精度和成本；
    3. 根据预测得分决定是否实际运行昂贵仿真；
    4. 将仿真结果反馈给代理模型和 LLM，更新知识。
  - 输出：在预算内找到的帕累托最优配置集。

## 3. 实验设计

- **数据集/场景**：三种典型 PDE 求解器：椭圆型、抛物型、双曲型（如 Laplace 方程、热方程、波动方程等）。具体问题未详细说明，但覆盖了广谱物理仿真。
- **基准（Benchmark）**：真实仿真器（ground truth simulator）作为推理时扩展的 baseline。
- **对比方法**：
  - 贝叶斯优化（Bayesian Optimization）
  - 基于 LLM 的基线（即未集成成本感知的 LLM 直接配置）
  - 推理时扩展（scaling）但无代理的版本（需大量昂贵查询）
- **评估指标**：成功率（在给定预算下找到合格配置的比率）、查询次数、计算成本。

## 4. 资源与算力

- **明确说明的部分**：论文未提及具体 GPU 型号、数量、训练时长等算力细节。仅提到代理模型训练数据需求较低（预测低维指标而非物理场），但未公开计算环境。**需注意：实验资源配置信息缺失。**

## 5. 实验数量与充分性

- **实验数量**：在三种 PDE 类型上进行了对比实验，可能每个类型下包含多个样例或参数设置。但摘要未给出具体实验数量，也未提及消融实验（如不同代理模型类型、不同成本函数等）。**实验的充分性有限**。
- **客观性与公平性**：声称一致优于贝叶斯优化和 LLM baseline，成功率接近使用真实仿真器的推理时扩展，但未提供细节（如误差棒、统计显著性）。**可能存在选择性展示风险**，因无完整实验表。

## 6. 论文的主要结论与发现

- CAED-Agent（CASCO-Agent）能够在不产生昂贵仿真开销的情况下，获得与使用真实仿真器的推理时扩展相当的成功率。
- 在多种 PDE 求解器配置任务中，该方法显著优于贝叶斯优化和纯 LLM 基线。
- 轻量级代理模型（GP/NN）可有效预测成本和精度，引导 LLM 避免低效配置。
- 验证了“成本感知推理时扩展”这一新范式在物理仿真自动化中的有效性。

## 7. 优点

- **创新性**：将 LLM 代理与成本感知轻量代理结合，解决了传统仿真优化高维且成本高的问题。
- **通用性**：框架灵活，支持不同数据量下的代理模型（GP 或 NN），可适用于多种物理仿真场景。
- **实用性**：显著降低仿真查询次数（减少计算开销），同时保持配置质量，适合实际工程应用。

## 8. 不足与局限

- **实验覆盖不足**：仅在三种 PDE 类型上验证，未涉及更复杂仿真（如多物理场耦合、商业软件）。缺乏消融实验（如不同 LLM 选择、不同代理模型设计的影响）。
- **偏差风险**：论文选自 ICLR-2026-Rejected，可能因实验不充分或创新性争议而被拒。未提供详细实验设置和代码复现信息，公平性存疑。
- **应用限制**：依赖 LLM 的推理能力，对于需要实时响应的场景可能仍显昂贵；代理模型在极度高维时可能仍失效。
- **资源算力未公开**：无法评估其实际可重复性和能耗。

（完）
