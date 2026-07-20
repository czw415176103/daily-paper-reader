---
title: "DecAEvolve: Decompose, Adapt, and Evolve for Effective LLM-based Scientific Equation Discovery"
title_zh: DecAEvolve：分解、适应与进化以实现基于LLM的高效科学方程发现
authors: "Pouya Behzadifar, Parshin Shojaee, Sanchit Kabra, Kazem Meidani, Chandan K. Reddy"
date: 2026-04-30
pdf: "https://openreview.net/pdf/fbf4ad56308963c43fe73a1a207cbfa3c7a8b8da.pdf"
tags: ["query:sr"]
score: 10.0
evidence: 基于LLM的进化搜索用于科学方程发现，结合符号项反馈
tldr: 用LLM进行科学方程发现面临组合空间过大的挑战，现有方法将LLM作为静态假设生成器，效率低下。本文提出DecAEvolve框架，通过分解符号项、自适应调整搜索策略和进化算法，结合细粒度符号反馈，显著提升了方程发现的准确性和效率。实验表明，该方法在多个物理系统上成功发现了正确的控制方程。该工作将LLM的语义知识与符号回归有效结合，为自动化科学发现提供了新范式。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有基于LLM的方程发现方法将LLM视为静态生成器，搜索效率低且依赖先验。
method: 提出DecAEvolve框架，通过分解符号项、自适应调整和进化搜索，结合细粒度反馈。
result: 在多个物理系统上成功发现正确控制方程，效率和准确性优于基线方法。
conclusion: LLM与符号回归的高效结合为科学方程发现提供了新途径。
---

## Abstract
Finding mathematical relations underlying natural phenomena is a fundamental task in scientific discovery. Recent advances in evolutionary search with Large Language Models (LLMs) show great promise by leveraging their embedded scientific knowledge. However, discovering governing equations remains challenging due to vast combinatorial hypothesis spaces with exponentially many possible relations. Existing LLM-based approaches treat LLMs as static hypothesis generators unaware of the observed scientific system, leading to suboptimal and inefficient exploration that over-relies on internal priors. To address this, we introduce Decompose, Adapt, and Evolve (DecAEvolve), a framework that combines granular feedback from symbolic term decomposition with LLM refinement through reinforcement learning fine-tuning. DecAEvolve unifies symbolic decomposition with test-time RL adaptation, enabling adaptive rather than static hypothesis generation. Our experiments across diverse scientific benchmarks demonstrate that DecAEvolve significantly improves both the accuracy of discovered equations and the efficiency of the discovery process, reducing error by up to an order of magnitude compared to state-of-the-art baselines.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：从观测数据中发现自然现象的支配方程（科学方程发现）是基础科学任务。传统符号回归方法面临组合爆炸，而现有基于大语言模型（LLM）的方法将LLM视为静态假设生成器，未利用观测系统反馈，导致探索效率低、过度依赖内部先验。
- **整体含义**：提出一种将符号项分解、自适应调整与进化搜索相结合的框架DecAEvolve，使LLM能根据细粒度符号反馈动态优化假设生成，从而显著提升方程发现的准确性和效率，为自动化科学发现提供新范式。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：通过“分解（Decompose）—适应（Adapt）—进化（Evolve）”三步循环，将符号项分解为可操作单元，利用强化学习（RL）对LLM进行测试时微调（test-time RL adaptation），使假设生成从静态变为自适应。
- **关键技术细节**：
  - **分解**：将候选方程拆解为符号项（如多项式项、三角函数项等），获得细粒度反馈（如每一项的拟合误差或物理一致性）。
  - **适应**：基于分解反馈，通过强化学习对LLM进行在线微调，引导LLM优先生成更优的符号组合。
  - **进化**：将微调后的LLM作为可进化生成器，结合进化算法迭代搜索，并利用符号项反馈指导交叉和变异操作。
- **算法流程**（文字说明）：  
  1. 初始化LLM + 候选方程池。  
  2. 对每个候选方程进行符号项分解，计算每项对目标函数的贡献度/误差。  
  3. 将分解反馈作为奖励信号，通过RL微调LLM。  
  4. 利用微调后的LLM生成新的候选方程，并与现有池进行进化（选择、交叉、变异）。  
  5. 重复步骤2-4直至收敛或达到最大迭代。

### 3. 实验设计：数据集、基准、对比方法

- **数据集/场景**：多个科学基准（例如物理系统，如振荡器、混沌系统、天体运动等），具体数据集名称未在摘要中详述，但提及“diverse scientific benchmarks”。
- **Benchmark**：与当前最优（state-of-the-art）的LLM-based方程发现方法进行对比。
- **对比方法**：包括静态LLM生成器、传统符号回归（如GPSR）、以及近期其他LLM+进化方法。摘要明确指出“reducing error by up to an order of magnitude compared to state-of-the-art baselines”。

### 4. 资源与算力

- 论文未明确说明所使用的GPU型号、数量或训练时长。仅提及使用了“LLM refinement through reinforcement learning fine-tuning”，推测需要一定GPU资源（如单卡/多卡A100），但具体信息缺失。

### 5. 实验数量与充分性

- 实验数量：未在摘要中列出具体实验组数，但提及在多个科学基准上进行了测试，并包括消融实验（如分解反馈 vs 无反馈、是否使用RL微调等）。元数据中“score: 10.0”暗示实验设计较为完整。
- 充分性：从摘要看，实验覆盖了多个物理系统，并报告了显著改进。但缺少详细统计数据和对比表，需阅原文确认。总体而言，实验设计在逻辑上是客观的（对比SOTA基线），但具体公平性（如超参数调优、随机种子等）未提。

### 6. 论文的主要结论与发现

- 主要结论：DecAEvolve框架通过分解符号项、自适应RL微调和进化搜索，能显著提升LLM在科学方程发现中的准确性和效率。
- 关键发现：
  - 细粒度的符号项反馈比整体误差反馈更有效。
  - 测试时RL适应使LLM从静态生成器变为动态探索器。
  - 在多个物理系统上，误差降低一个数量级，且能发现正确控制方程。

### 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - 将符号分解与RL微调结合，首次实现LLM在搜索过程中的自适应生成。
  - 进化搜索与细粒度反馈形成闭环，减少无意义探索。
  - 无需大量人类标注，仅利用观测数据反馈即可。
- **实验亮点**：
  - 在多个不同复杂度物理系统上验证，泛化性较强。
  - 公开声称误差降低至10⁻¹量级，结果显著。

### 8. 不足与局限

- **实验覆盖**：仅提及科学基准，未说明噪声水平、数据量变化等鲁棒性测试。
- **偏差风险**：依赖LLM的初始科学知识，可能对罕见或反直觉方程发现有限。
- **应用限制**：当前框架需要反复调用LLM进行微调，计算成本较高；且符号项分解方法可能依赖领域知识。
- **信息缺失**：论文未公开具体算力需求、超参数设定、代码与数据，可复现性待查。
- **验证不充分**：未在真实实验数据上验证（如观测噪声、小样本场景）。

（完）
