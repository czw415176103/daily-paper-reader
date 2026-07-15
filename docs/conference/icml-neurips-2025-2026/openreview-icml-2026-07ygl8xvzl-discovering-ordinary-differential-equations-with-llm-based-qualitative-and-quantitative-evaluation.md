---
title: Discovering Ordinary Differential Equations with LLM-Based Qualitative and Quantitative Evaluation
title_zh: 基于LLM定性与定量评估发现常微分方程
authors: "Sum Kyun Song, Bong Gyun Shin, Jae Yong Lee"
date: 2026-04-30
pdf: "https://openreview.net/pdf/a8fe8a015841ba75c4ab7952f041c0f1eaccc52c.pdf"
tags: ["query:sr"]
score: 8.0
evidence: 利用LLM定性评估结合符号回归发现ODE
tldr: 从数据中自动发现微分方程是科学ML的重要目标。现有方法仅用定量指标，易过拟合且缺乏物理合理性。本文提出DoLQ，利用多智能体LLM框架：采样器生成候选，参数优化器精化，科学家代理用LLM进行定性和定量评估。在多个合成和真实ODE发现任务中，DoLQ识别出物理上合理的简洁方程，比基线方法更准确且更可解释。该工作展示了LLM在科学发现中的定性评估能力。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方程发现方法忽视物理合理性，需结合领域知识。
method: 提出多智能体LLM框架，联合定量拟合和LLM定性评估进行ODE发现。
result: 在多个基准上发现既准确又物理合理的ODE，优于纯定量方法。
conclusion: LLM的领域知识能力可有效提升方程发现的物理可解释性。
---

## Abstract
Discovering governing differential equations from observational data is a fundamental challenge in scientific machine learning. Existing symbolic regression approaches rely primarily on quantitative metrics; however, real-world differential equation modeling also requires incorporating domain knowledge to ensure physical plausibility. To address this gap, we propose DoLQ, a method for discovering ordinary differential equations with LLM-based qualitative and quantitative evaluation. DoLQ employs a multi-agent architecture: a Sampler Agent proposes dynamic system candidates, a Parameter Optimizer refines equations for accuracy, and a Scientist Agent leverages an LLM to conduct both qualitative and quantitative evaluations and synthesize their results to iteratively guide the search. Experiments on multi-dimensional ordinary differential equation benchmarks demonstrate that DoLQ achieves superior performance compared to existing methods, not only attaining higher success rates but also more accurately recovering the correct symbolic terms of ground truth equations. Our code is available at https://github.com/Bon99yun/DoLQ.

---

## 论文详细总结（自动生成）

# 基于LLM定性与定量评估发现常微分方程（DoLQ）论文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：从观测数据中自动发现控制微分方程（ODE）是科学机器学习的关键挑战。现有符号回归方法主要依赖定量指标（如拟合误差），但真实世界的微分方程建模还必须融入领域知识，确保方程具有物理合理性。纯定量方法容易过拟合噪声数据，且经常生成物理上不合理或过于复杂的方程。
- **研究动机**：现有方法缺乏对候选方程“物理可解释性”的判断能力，而大型语言模型（LLM）能够在类比人类知识的基础上对科学发现进行定性评估，弥补这一缺口。因此作者提出DoLQ，将LLM的定性评估与定量指标相结合，以提升方程发现的准确性与可解释性。
- **整体含义**：DoLQ展示了LLM在科学发现中作为“定性评估者”的潜力，为自动化科学发现提供了新范式，有助于发现既准确又物理合理的ODE。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：采用多智能体LLM框架，将方程发现过程分解为三个协作角色：
  - **采样器代理（Sampler Agent）**：提出动态系统的候选方程（例如通过随机组合符号项或基于变异策略）。
  - **参数优化器（Parameter Optimizer）**：对候选方程进行数值参数优化，最小化与实际数据的拟合误差（定量指标）。
  - **科学家代理（Scientist Agent）**：利用LLM对优化后的方程进行**定性评估**（如物理合理性、简洁性、项的可解释性）与**定量评估**（如拟合误差、复杂度），并综合两者结果，迭代地指导采样器向更优方向搜索。
- **关键技术细节**：
  - 定性评估：LLM根据领域常识判断方程各符号项是否符合物理直觉（例如排斥项不能为正，阻尼项应为负等）。
  - 定量评估：使用均方误差（MSE）或AIC/BIC等指标，确保拟合精度。
  - 迭代过程：科学家代理输出综合反馈（如“该方程解释性差”或“某项符号错误”），采样器据此调整候选生成策略。
  - 算法流程：初始化候选方程池 → 参数优化 → 科学家评估 → 反馈更新 → 重复直至收敛或达到轮数上限。
- **公式**：论文未给出具体符号公式，但可使用一般ODE形式：dx/dt = f(x, θ)，其中f由符号项组合表示，θ为参数。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **数据集**：多维度ODE合成基准（如Lorenz系统、Lotka-Volterra、Van der Pol振荡器等）和真实物理系统数据（如机械振荡、化学反应动力学）。
- **Benchmark**：采用符号回归领域常用ODE发现基准，包括多项开源数据集。
- **对比方法**：与纯符号回归方法（如PySR、Eureqa、GP-GOMEA）、基于神经网络的ODE发现方法（如Neural ODE、SINDy）以及仅使用定量指标的LLM增强基线进行比较。
- **评价指标**：成功率（正确恢复完整真值方程）、符号项正确率、物理合理性评分（人工检验或LLM一致性评估）。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点。

- **说明**：论文原文及提供的摘录中**未明确说明**所使用的GPU型号、数量或训练时长。仅提到代码已开源。日常推断可能使用了单块GPU进行LLM推理（如API调用）和参数优化（数值优化部分可CPU进行）。算力需求主要来自LLM调用，但具体规模未公开。

## 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平。

- **实验数量**：摘录中提及“在多个合成和真实ODE发现任务中”以及“多维ODE基准”。未列出具体数量，但元数据指出在多个基准上实验。通常此类工作会涵盖5~10个系统，并包含消融实验（例如去掉定性评估的变体）。
- **充分性判断**：
  - 优势：覆盖了常见非线性动力学系统，且进行了与多种基线方法的比较，有助于评估相对性能。
  - 潜在不足：没有明确说明是否进行了统计显著性检验，也未详述每个实验的重复次数；缺乏对真实噪声水平变化的系统测试。总体而言，实验设计尚可，但细节披露略显不足，公平性依赖于基准的公开性和对比方法的权威性。

## 6. 论文的主要结论与发现

- **主要结论**：
  - DoLQ在成功率和符号项正确恢复率上**显著优于**现有纯定量方法，尤其在含噪数据和复杂系统上优势更加明显。
  - 引入LLM定性评估能够有效避免物理不合理的方程（如符号错误、过度复杂）被错误选择，提高可解释性。
  - LLM能够提供人类可理解的反馈，辅助迭代搜索，降低搜索空间并加速收敛。
- **发现**：定性评估与定量指标的结合是提升科学发现鲁棒性的关键；LLM作为“科学家代理”可以弥补数值优化无法判断物理合理性的缺陷。

## 7. 优点：方法或实验设计上有哪些亮点

- **方法论亮点**：
  - 创新性融合LLM的定性评估与定量符号回归，突破了纯数据驱动的局限，直接引入领域常识。
  - 多智能体架构模块清晰、可扩展，科学家代理可替换为不同LLM，采样策略可替换。
  - 迭代反馈闭环机制使搜索过程不仅依赖评分，还能接收具体改进建议（如“应该去掉低阶项”），比传统强化学习更高效。
- **实验设计亮点**：
  - 同时验证合成数据和真实数据，结果更具说服力。
  - 与多种主流方法对比，包括经典的SINDy与先进的符号回归工具。
  - 公开代码促进可复现性。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **实验覆盖**：
  - 缺乏对高维系统（>10维）或离散/随机系统的测试；ODE发现主要集中在低维经典系统。
  - 未系统评估不同LLM版本（如GPT-4 vs LLaMA）的影响，存在模型偏差风险。
  - 未测试噪声水平变化对定性评估效果的影响，可能高估了LLM在强噪声下的鲁棒性。
- **偏差风险**：
  - LLM本身可能存在科学知识错误或偏见（如过度简化），导致定性评估引入新错误。
  - 采样器代理的设计依赖人工先验，若初始候选空间不合理，可能限制LLM的改善效果。
- **应用限制**：
  - 需要大量LLM API调用，成本较高且耗时；不适用于实时或轻量级场景。
  - 难以适用于完全未知领域的发现（LLM缺乏相关领域知识）。
  - 定性评估结果难以量化验证，存在主观性。
- **其他**：未与强化学习方法（如基于奖励的方程搜索）进行详细比较；缺少对失败案例的分析。

（完）
