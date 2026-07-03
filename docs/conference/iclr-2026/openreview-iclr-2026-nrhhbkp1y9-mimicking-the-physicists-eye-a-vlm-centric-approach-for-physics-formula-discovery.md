---
title: "Mimicking the Physicist's Eye: A VLM-centric Approach for Physics Formula Discovery"
title_zh: 模仿物理学家的眼睛：以VLM为中心的物理公式发现方法
authors: "Jiaqi Liu, Songning Lai, Pengze Li, Di Yu, Zhou wenjie, Yiyang Zhou, Peng Xia, Zijun Wang, Xi Chen, SHIXIANG TANG, LEI BAI, Wanli Ouyang, Mingyu Ding, Huaxiu Yao, Aoran Wang"
date: 2025-09-04
pdf: "https://openreview.net/pdf?id=nRhHbKP1y9"
tags: ["query:sr"]
score: 9.0
evidence: 多模态视觉语言模型用于基于视觉和轨迹数据的物理公式发现
tldr: 该论文提出VIPER-R1多模态模型，综合视觉感知、轨迹数据和符号推理，模仿物理学家通过观察发现公式的过程，直接回应了物理信息机器学习用于科学发现和方程提取的需求。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有符号回归方法依赖单模态数据，忽略视觉表征。
method: 集成视觉、轨迹和符号推理的多模态模型VIPER-R1。
result: 在物理公式发现任务上超越单模态方法。
conclusion: 展示了多模态在科学发现中的重要作用。
---

## Abstract
Automated discovery of physical laws from observational data in the real world is a grand challenge in AI. Current methods, relying on symbolic regression or LLMs, are limited to uni-modal data and overlook the rich, visual phenomenological representations of motion that are indispensable to physicists. This "sensory deprivation" severely weakens their ability to interpret the inherent spatio-temporal patterns within dynamic phenomena. To address this gap, we propose VIPER-R1, a multimodal model that performs Visual Induction for Physics-based Equation Reasoning to discover fundamental symbolic formulas. It integrates visual perception, trajectory data, and symbolic reasoning to emulate the scientific discovery process. The model is trained via a curriculum of Motion Structure Induction (MSI), using supervised fine-tuning to interpret kinematic phase portraits and to construct hypotheses guided by a Causal Chain of Thought (C-CoT), followed by Reward-Guided Symbolic Calibration (RGSC) to refine the formula structure with reinforcement learning. During inference, the trained VIPER-R1 acts as an agent: it first posits a high-confidence symbolic ansatz, then proactively invokes an external symbolic regression tool to perform Symbolic Residual Realignment (SR^2). This final step, analogous to a physicist's perturbation analysis, reconciles the theoretical model with empirical data. To support this research, we introduce PhysSymbol, a new 10,000-instance multimodal corpus. Experiments show that VIPER-R1 consistently outperforms state-of-the-art VLM baselines in accuracy and interpretability, enabling more precise discovery of physical laws.

---

## 论文详细总结（自动生成）

# 论文总结：VIPER-R1——以VLM为中心的物理公式发现

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：从真实世界观测数据中自动发现物理定律是AI领域的重大挑战。现有方法（如符号回归、大语言模型）局限于单模态数据（如轨迹、数值），忽略了物理学家不可或缺的、包含丰富时空模式的**视觉现象表征**。这种“感官剥夺”严重削弱了模型对动态现象中内在时空模式的理解能力。
- **整体含义**：本文提出一种多模态模型VIPER-R1，通过融合视觉感知、轨迹数据和符号推理，模仿物理学家“观察运动→提出猜想→公式验证”的科学发现过程，旨在提升物理公式自动发现的准确性和可解释性。

## 2. 方法论：核心思想与关键技术细节
- **核心思想**：以视觉语言模型（VLM）为中心，整合视觉表征（运动相位图）和轨迹数据，并通过课程式训练和强化学习实现符号公式推理。
- **训练流程**（分为三个阶段）：
  1. **运动结构归纳（Motion Structure Induction, MSI）**：采用监督微调（SFT），让模型学会解读运动相位图，并基于**因果链式思考（Causal Chain of Thought, C-CoT）** 构建假设，即逐步生成因果推理路径。
  2. **奖励引导符号校准（Reward-Guided Symbolic Calibration, RGSC）**：通过强化学习（RL）优化公式结构，根据奖励信号调整符号表达式。
- **推理流程**：训练后的VIPER-R1作为智能体，先输出高置信度的符号假设（symbolic ansatz），然后主动调用外部符号回归工具进行**符号残差对齐（Symbolic Residual Realignment, SR²）**——该步骤类比物理学家的扰动分析，将理论模型与经验数据之间的残差进行修正，最终得到更精确的公式。

## 3. 实验设计
- **数据集**：作者构建了**PhysSymbol**——一个包含10,000个实例的多模态语料库，涵盖多种物理运动场景（视觉相位图+轨迹数据+对应公式）。
- **基准测试**：未明确说明标准benchmark名称，但实验基于PhysSymbol进行。
- **对比方法**：与当前最先进的视觉语言模型（VLM）基线进行对比，包括准确性和可解释性指标。

## 4. 资源与算力
- 文中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅提及训练过程包含SFT和RL阶段，但具体资源消耗未披露。

## 5. 实验数量与充分性
- 由于仅提供摘要，**未给出具体的实验组数**（如消融实验、不同数据集上的对比、不同超参数的影响等）。论文宣称“在准确性和可解释性上一致超越SOTA VLM基线”，但缺乏详细的实验统计和消融分析。因此，实验的充分性和客观性无法从摘要中全面判断，需要参考完整论文。

## 6. 主要结论与发现
- VIPER-R1在物理公式发现任务上性能优于所有单模态和多模态VLM基线，展现出更高的公式准确性和更好的可解释性。
- 多模态融合（视觉+轨迹）对理解物理运动模式至关重要，因果链式思考和符号残差对齐机制有效提升了公式发现的质量。
- 该方法展示了视觉语言模型在科学发现中的巨大潜力，为自动物理定律发现提供了新范式。

## 7. 优点
- **多模态集成创新**：首次将视觉感知（运动相位图）与轨迹数据、符号推理系统性地结合，更贴合真实物理学家的工作方式。
- **课程式训练策略**：从MSI（监督微调）到RGSC（强化学习）的渐进式训练，先学基础模式再优化输出，符合人类认知规律。
- **因果链式思考（C-CoT）**：引入显式的因果推理步骤，提升了模型的可解释性和推理可靠性。
- **外部工具协作**：推理时调用符号回归工具进行残差对齐（SR²），相当于将机器学习模型与经典符号回归方法结合，兼顾灵活性与精确性。
- **构建专用数据集**：PhysSymbol为后续研究提供了标准化多模态物理公式发现基准。

## 8. 不足与局限
- **实验覆盖有限**：仅基于自建数据集PhysSymbol（10,000实例），未在真实世界复杂物理场景（如噪声、多物体交互）或更广泛物理定律集合上验证，泛化性存疑。
- **算力与效率未披露**：缺乏训练成本细节，难以评估方法的实际部署可行性。
- **对预训练基座依赖**：VLM的初始能力（如视觉理解、符号推理）直接影响最终性能，但未分析不同基座的影响。
- **可扩展性风险**：当前方法可能仅适用于运动学型物理规律，对于含偏微分方程或非线性动力系统的复杂定律发现能力未知。
- **潜在偏差**：数据集可能偏向特定类型的运动，导致模型在未见过的场景上性能下降。

（完）
