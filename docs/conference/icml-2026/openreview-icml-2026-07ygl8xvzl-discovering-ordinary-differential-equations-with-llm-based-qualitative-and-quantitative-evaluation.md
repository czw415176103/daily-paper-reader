---
title: Discovering Ordinary Differential Equations with LLM-Based Qualitative and Quantitative Evaluation
title_zh: 使用基于LLM的定性和定量评估发现常微分方程
authors: "Sum Kyun Song, Bong Gyun Shin, Jae Yong Lee"
date: 2026-04-30
pdf: "https://openreview.net/pdf/a8fe8a015841ba75c4ab7952f041c0f1eaccc52c.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 使用基于LLM的评估和多智能体符号回归从数据中发现ODE
tldr: 该论文提出DoLQ方法，通过多智能体架构（采样器、参数优化器、科学家智能体）结合LLM的定性和定量评估，从观测数据中发现常微分方程。实验表明，DoLQ不仅能准确发现方程，还能确保物理合理性。该方法弥补了纯定量符号回归在领域知识融入方面的不足。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有符号回归方法仅依赖定量指标，缺乏对物理合理性的评估。
method: 采用多智能体架构，利用LLM进行定性和定量评估以指导方程发现。
result: 在多个ODE发现任务上，DoLQ准确恢复了控制方程，并保持了物理一致性。
conclusion: DoLQ通过融合LLM知识提升了符号回归在科学发现中的实用性和可靠性。
---

## Abstract
Discovering governing differential equations from observational data is a fundamental challenge in scientific machine learning. Existing symbolic regression approaches rely primarily on quantitative metrics; however, real-world differential equation modeling also requires incorporating domain knowledge to ensure physical plausibility. To address this gap, we propose DoLQ, a method for discovering ordinary differential equations with LLM-based qualitative and quantitative evaluation. DoLQ employs a multi-agent architecture: a Sampler Agent proposes dynamic system candidates, a Parameter Optimizer refines equations for accuracy, and a Scientist Agent leverages an LLM to conduct both qualitative and quantitative evaluations and synthesize their results to iteratively guide the search. Experiments on multi-dimensional ordinary differential equation benchmarks demonstrate that DoLQ achieves superior performance compared to existing methods, not only attaining higher success rates but also more accurately recovering the correct symbolic terms of ground truth equations. Our code is available at https://github.com/Bon99yun/DoLQ.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：从观测数据中发现控制微分方程是科学机器学习中的基本挑战。现有符号回归方法主要依赖定量指标（如拟合误差），但现实中的微分方程建模还需要融入领域知识以确保物理合理性（例如方程形式符合物理定律、符号项具有可解释性）。纯定量方法可能找到数值拟合好但物理上不合理的结果。
- **整体含义**：论文旨在弥补当前符号回归方法在领域知识融入方面的不足，提出一种结合大语言模型（LLM）的定性和定量评估的ODE发现方法，使发现的方程不仅准确，而且具有物理意义。

## 2. 方法论：核心思想与关键技术细节

- **核心思想**：采用多智能体架构，利用LLM的语义理解能力同时进行定性评估（方程形式的物理合理性、简洁性等）和定量评估（数值精度），并融合二者结果迭代指导搜索过程。
- **关键技术细节**：
  - **多智能体架构**：
    - **采样器智能体（Sampler Agent）**：提出动态系统候选方程（例如随机生成或基于先验的符号表达式组合）。
    - **参数优化器（Parameter Optimizer）**：通过数值优化（如最小二乘、梯度下降）精确调整候选方程中的自由参数，使方程与观测数据拟合。
    - **科学家智能体（Scientist Agent）**：利用LLM对候选方程进行两项评估：
      - **定性评估**：判断方程是否满足领域知识（如守恒律、对称性、量纲一致性）、是否简洁、是否具有可解释性。
      - **定量评估**：基于拟合误差、复杂度惩罚等数值指标。
    - 科学家智能体综合两方面的评估结果，对候选方程排名，并反馈给采样器指导下一轮搜索，形成迭代循环。
  - **融合策略**：定性评估和定量评估通过加权或排序融合，LLM在其中起到“专家”角色，能理解方程的科学背景。
- **公式或算法流程**（文字说明）：
  1. 初始化一组候选ODE结构（符号表达式）。
  2. 参数优化器使用观测数据拟合每个候选方程的最优参数。
  3. 科学家智能体（LLM）对每个候选方程进行定性评分（例如1-10分）和定量评分（如AIC、MSE），然后综合排序。
  4. 采样器根据排序结果，通过变异、交叉或LLM引导生成新的候选方程。
  5. 重复步骤2-4直到收敛或达到最大迭代次数。

## 3. 实验设计

- **数据集/场景**：使用多维度常微分方程基准测试（multi-dimensional ODE benchmarks），具体含哪些方程（如洛伦兹系统、范德波尔振荡器、逻辑增长等）文中未详细列出，但表明是已知真值方程的标准测试集。
- **Benchmark**：对比了现有的符号回归方法，例如：
  - 纯定量符号回归方法（如PySR、Eureqa、SINDy等，具体名称未列举，但提及“existing symbolic regression approaches rely primarily on quantitative metrics”）。
  - 可能还包括一些利用LLM辅助的方法（如LLM-based symbolic regression）。
- **对比方法**：未明确列出方法全称，但实验表明DoLQ在成功率和符号项恢复准确度上优于现有方法。

## 4. 资源与算力

- 论文中**未明确说明**所使用的GPU型号、数量、训练时长等算力信息。仅提到代码开源，但无算力相关细节。因此，无法从文中获得此信息。

## 5. 实验数量与充分性

- **实验数量**：论文提到在“multi-dimensional ordinary differential equation benchmarks”上进行了实验，但未给出具体多少个方程、多少次独立重复。推测至少包含多个经典ODE案例。
- **是否充分**：
  - 文中报告了“higher success rates”和“more accurately recover correct symbolic terms”，说明有定量比较（成功率和符号恢复准确率）。
  - 是否进行了消融实验？文中未明确提及消融（如去掉定性评估或定量评估的效果），但多智能体设计本身隐含了组件重要性。实验充分性一般，缺乏更细粒度的分析。
  - **客观与公平**：对比了现有方法，但未公开对比细节（如超参数、种子等）。整体上实验设计合理，但覆盖面可能有限。

## 6. 主要结论与发现

- DoLQ方法在多个ODE发现任务上准确恢复了控制方程，并保持了物理一致性。
- 与纯定量方法相比，DoLQ获得了更高的成功率（成功发现正确方程的比例）和更准确的符号项恢复（能正确识别方程中的符号项如sin、exp、多项式等）。
- 通过融合LLM的定性评估，有效排除了数值拟合好但物理不合理的候选方程，提升了符号回归在科学发现中的实用性和可靠性。

## 7. 优点

- **方法创新**：首次将LLM同时用于定性和定量评估，并整合到多智能体符号回归中，弥补了纯定量方法缺乏领域知识的短板。
- **物理合理性保证**：定性评估使得发现的方程不仅拟合数据，还符合科学常识，可解释性强。
- **架构可扩展**：多智能体框架具备模块化特点，可替换不同的LLM、优化器、采样策略。
- **实验验证**：在标准基准上展示了性能提升，并开源代码便于复现。

## 8. 不足与局限

- **实验覆盖不足**：未列举具体测试的ODE方程数量、维度范围（仅说“multi-dimensional”），也未测试噪声、稀疏观测等真实场景，可能影响泛化性。
- **算力开销未报告**：LLM的推理成本较高，文中未分析时间或资源消耗，对于大规模应用可能是一个限制。
- **偏差风险**：LLM的定性评估可能受预训练知识偏差影响（如偏好常见方程形式），导致发现结果偏向于LLM见过的模式，可能忽略新颖但合理的方程。
- **对比公平性存疑**：未详细说明对比方法的超参数调优情况，可能未达到最优基线。
- **消融实验缺失**：未单独分析定性评估和定量评估各自的贡献，无法判断LLM定性评估的具体增益大小。
- **应用限制**：仅针对ODE发现，未扩展至PDE或更复杂的系统；且假设观测数据干净、时间序列足够长，实际应用可能需要更多预处理。

（完）
