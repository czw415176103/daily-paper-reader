---
title: "DecAEvolve: Decompose, Adapt, and Evolve for Effective LLM-based Scientific Equation Discovery"
title_zh: DecAEvolve：分解、适应与进化实现有效的基于LLM的科学方程发现
authors: "Pouya Behzadifar, Parshin Shojaee, Sanchit Kabra, Kazem Meidani, Chandan K. Reddy"
date: 2026-04-30
pdf: "https://openreview.net/pdf/fbf4ad56308963c43fe73a1a207cbfa3c7a8b8da.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 基于LLM的进化搜索方程发现
tldr: 现有基于LLM的方程发现方法将LLM视为静态假设生成器，无法感知观测系统，导致探索效率低下。DecAEvolve框架通过分解、适应和进化策略，结合符号项的细粒度反馈，有效引导LLM搜索组合空间。实验表明该方法在多个科学领域比基线更高效地发现正确方程，推动了LLM在符号回归中的应用。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有LLM方程发现方法将LLM视为静态生成器，忽视观测系统，导致搜索效率低下。
method: 提出DecAEvolve框架，通过分解符号项、适应观测数据反馈、进化搜索策略，利用LLM生成候选方程。
result: 在多个科学数据集上，DecAEvolve相比基线方法发现正确方程的成功率更高且搜索成本更低。
conclusion: 结合LLM的先验知识和数据反馈的进化框架能有效提升方程发现性能。
---

## Abstract
Finding mathematical relations underlying natural phenomena is a fundamental task in scientific discovery. Recent advances in evolutionary search with Large Language Models (LLMs) show great promise by leveraging their embedded scientific knowledge. However, discovering governing equations remains challenging due to vast combinatorial hypothesis spaces with exponentially many possible relations. Existing LLM-based approaches treat LLMs as static hypothesis generators unaware of the observed scientific system, leading to suboptimal and inefficient exploration that over-relies on internal priors. To address this, we introduce Decompose, Adapt, and Evolve (DecAEvolve), a framework that combines granular feedback from symbolic term decomposition with LLM refinement through reinforcement learning fine-tuning. DecAEvolve unifies symbolic decomposition with test-time RL adaptation, enabling adaptive rather than static hypothesis generation. Our experiments across diverse scientific benchmarks demonstrate that DecAEvolve significantly improves both the accuracy of discovered equations and the efficiency of the discovery process, reducing error by up to an order of magnitude compared to state-of-the-art baselines.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：科学发现的核心任务之一是寻找描述自然现象的数学关系（即方程发现）。近年来，大型语言模型（LLM）结合进化搜索的方法展现出潜力，因为LLM内置了丰富的科学知识。
- **现有挑战**：候选方程的组合空间呈指数级增长，现有基于LLM的方法将LLM视为静态的假设生成器，无法感知观测到的科学系统，导致探索效率低下、过度依赖内部先验，容易陷入次优解。

## 2. 论文提出的方法论
- **核心思想**：提出 **DecAEvolve（Decompose, Adapt, and Evolve）** 框架，通过将符号项分解、观测数据反馈与LLM的强化学习（RL）微调相结合，实现自适应的假设生成，而非静态生成。
- **关键技术细节**：
  - **符号项分解（Decompose）**：将候选方程拆解为基本的符号项，获得细粒度的反馈信号。
  - **适应（Adapt）**：在测试时（test-time）引入RL机制，利用观测数据对符号项的反馈调整LLM的行生策略。
  - **进化（Evolve）**：通过进化搜索迭代优化候选方程，同时利用LLM的生成能力探索新组合。
- **算法流程**：论文未提供具体的公式或伪代码，但整体流程可概括为：1）初始方程候选集由LLM生成；2）对每个候选方程进行符号项分解并与观测数据拟合得到反馈；3）基于RL对LLM进行测试时微调，使其偏向于产生更优的符号组合；4）重复进化搜索直到收敛。

## 3. 实验设计
- **数据集与场景**：使用了**多样化的科学基准数据集**（具体名称未在摘要中列出，推测涵盖物理学、生物学等领域的方程发现任务）。
- **Benchmark与对比方法**：与当前最先进的（state-of-the-art）基线方法进行对比（基线名称未具体给出，可能包括传统符号回归方法及其他LLM+进化方法）。
- **指标**：方程发现准确性和发现过程的效率（如搜索成本、误差等）。

## 4. 资源与算力
- 论文摘要及元数据中**未明确说明**所使用的GPU型号、数量、训练时长等硬件信息。此部分信息缺失，可能需要在全文或附录中查找。

## 5. 实验数量与充分性
- **实验数量**：摘要仅提及“多组科学基准实验”，未提供具体数字。推断至少包括多个不同领域的数据集以及与多个基线的比较。
- **充分性与公平性**：从“误差降低高达一个数量级”这一显著结果看，实验应该覆盖了典型场景；但未提及详细的消融实验或统计显著性检验，且基线方法的具体设置未知，因此公平性需参考全文进一步确认。

## 6. 主要结论与发现
- **主要结论**：DecAEvolve显著提高了方程发现的准确性和发现过程的效率，相对于当前最先进基线，误差降低高达一个数量级。
- **具体发现**：结合LLM的先验知识与观测数据反馈的进化框架，能够克服静态生成器对内部先验的过度依赖，实现更有效的探索。

## 7. 优点
- **方法论创新**：首次将符号项分解与测试时RL适应统一在进化搜索中，实现了自适应的假设生成，突破了传统方法“静态生成器”的局限。
- **效率与精度**：在误差降低一个数量级的同时，搜索成本更低，体现出更优的探索-利用平衡。
- **可解释性**：符号项分解提供了细粒度反馈，有助于理解和优化方程结构。
- **通用性**：在多个科学领域数据上验证，表明框架具有较好的跨领域迁移潜力。

## 8. 不足与局限
- **实验覆盖不完整**：摘要未提供具体数据集名称、基线方法细节、消融实验结果及统计置信度，难以全面评估方法的鲁棒性。
- **计算资源未披露**：缺少训练/推理所需的算力成本，不利于其他研究者复现和评估实际可用性。
- **潜在偏差风险**：LLM的先验知识可能对某些领域的方程有偏向，测试时RL适应能否完全消除这种偏差尚需验证。
- **应用限制**：方法依赖于符号项分解的质量，对于高度非线性或隐式方程可能效果下降；RL微调可能带来稳定性问题。

（完）
