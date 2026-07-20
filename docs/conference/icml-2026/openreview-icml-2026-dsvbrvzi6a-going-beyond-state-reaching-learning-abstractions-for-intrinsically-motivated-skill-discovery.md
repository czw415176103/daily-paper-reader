---
title: "Going Beyond State-Reaching: Learning Abstractions for Intrinsically Motivated Skill Discovery"
title_zh: 超越状态达到：为内在动机技能发现学习抽象
authors: "Akhil Bagaria, Anita De Mello Koch, George Konidaris"
date: 2026-01-21
pdf: "https://openreview.net/pdf/66d9c28ba79abeb37d72bd6c9bdd44b137f0951f.pdf"
tags: ["query:cfd-agent"]
score: 9.0
evidence: 通过内在动机学习可迁移的抽象选项进行技能发现
tldr: 现有选项发现算法通常针对状态的每一个方面设定子目标，导致选项泛化性差且数量爆炸。本文提出一种新算法，自动识别每个子目标的少量相关特征，学习抽象且可迁移的选项。在多个稀疏奖励图像环境中，该方法实现了快速探索和高效技能发现。该工作推动了内在动机技能学习的发展，提高了选项的实用性和可迁移性。
source: ICML-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有选项发现算法因子目标针对所有状态方面导致选项泛化差、数量爆炸。
method: 提出学习子目标相关特征子集的抽象选项发现算法，利用内在动机驱动。
result: 在稀疏奖励环境中显著加速探索，学习到可迁移的抽象选项。
conclusion: 证明了抽象特征子集对于选项泛化和内在动机技能发现的重要性。
---

## Abstract
Temporal abstraction via options can improve exploration in vast environments. However, existing option discovery algorithms find subgoals that target all aspects of the state simultaneously. This \textit{state-reaching} approach produces options that only apply in narrow regions of the state-space, eventually causing an explosion in the number of options that overwhelms the agent, and impedes progress on its primary task of reward maximization. We introduce an algorithm that instead identifies a small, relevant subset of features for each subgoal, yielding options that generalize broadly and accelerate exploration. Our approach learns abstract, transferrable options and achieves rapid exploration in three sparse-reward, image-based domains, including the Atari game \textsc{MontezumasRevenge}.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有基于选项（option）的时序抽象技能发现算法，在设置子目标时要求智能体同时到达状态的所有方面（即“状态达到”），导致学到的选项只能在状态空间非常狭窄的区域中泛化，最终产生大量选项，使智能体负担过重，阻碍其主要任务（奖励最大化）的进展。
- **研究背景**：在高维、稀疏奖励环境中，探索效率是关键挑战。选项（option）作为时间抽象的动作序列，可以显著提升探索速度。然而，传统的选项发现方法（如基于子目标的方法）要求子目标覆盖全部状态特征，限制了选项的可迁移性和泛化能力。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：不再要求子目标覆盖所有状态特征，而是自动识别每个子目标中**少量、相关**的特征子集，从而学习具有广泛泛化能力和可迁移性的**抽象选项**。
- **关键技术细节**：
  - 算法会在内在动机驱动下，为每个潜在子目标选择一组最相关的特征（例如，图像中只关心某物体的位置，忽略背景颜色等）。
  - 通过这种方式，选项的条件和效果被抽象化，使得同一选项可在不同状态上下文中复用。
  - 内在动机表现为探索奖励，引导智能体优先学习那些能在不同区域重复使用的抽象选项。
- **算法流程（文字描述）**：
  1. 初始化：定义状态空间、特征空间、子目标候选集。
  2. 对于每个子目标，使用某种特征选择机制（如互信息、注意力）识别一个小的相关特征子集。
  3. 基于该特征子集，通过内在动机奖励（如探索新奇特征组合）学习选项的终止条件和策略。
  4. 智能体在执行主任务（奖励最大化）的过程中，同时利用学到的抽象选项进行高效探索。
  5. 重复迭代，不断发现新的抽象选项，直至覆盖关键任务结构。

### 3. 实验设计：使用了哪些数据集/场景，benchmark，对比了哪些方法

- **实验场景**：三个稀疏奖励、基于图像的环境，包括Atari游戏 **Montezuma's Revenge**（该游戏以极难探索著称）。另外两个环境未在摘要中具体说明，但都是图像输入、稀疏奖励的复杂环境。
- **Benchmark**：对比的基线方法未在摘要中明确列出，但可以推断包括传统的“状态达到”子目标发现算法（如基于计数、基于原型的选项发现等），以及可能的随机探索策略。
- **对比方法**：论文应与至少一种标准选项发现算法（例如，DDO、SMDP-Based Option Discovery）以及无选项的基线（如DDQN）进行了比较。

### 4. 资源与算力：是否提及GPU型号、数量、训练时长

- **未明确说明**：在提供的摘要和元数据中，没有提及具体的GPU型号、数量或训练时长。通常此类强化学习论文会给出硬件信息，但此处缺失。我们可以指出“论文未提供算力细节”。

### 5. 实验数量与充分性：实验组数、消融实验、公平性

- **实验数量**：在三个稀疏奖励图像环境中进行了验证，包括Atari游戏Montezuma's Revenge。可能包含多次重复实验（如5个随机种子），但摘要未说明具体重复次数。
- **消融实验**：未在摘要中明确提及，但很可能包含对特征选择机制、内在动机设计等的消融实验，以验证各组件贡献。
- **充分性与公平性**：从描述看，实验环境具有挑战性且多样化，对比方法应为公平设置。但缺少具体对比数值和统计显著性说明，需谨慎评估。结论称“快速探索”等，可能需要更多定量指标支撑。

### 6. 论文的主要结论与发现

- 提出的算法能够自动学习抽象、可迁移的选项，这些选项在状态空间中具有广泛泛化能力，显著加速探索。
- 在三个稀疏奖励图像域（包括Montezuma's Revenge）中，该算法比传统“状态达到”方法更快地发现有效技能，进而更快获取主要任务奖励。
- 证明了识别子目标相关特征子集（而非全状态）对于选项泛化和内在动机技能发现至关重要。

### 7. 优点：方法或实验设计上的亮点

- **方法亮点**：突破了传统选项发现中“子目标覆盖所有状态维度”的局限性，通过特征选择实现了抽象化和可迁移性，使得选项数量可控，泛化能力增强。
- **实验设计亮点**：选择了极具挑战性的稀疏奖励图像域（如Montezuma's Revenge），这些环境是检验探索算法的“试金石”；同时包含多种环境，增加结论的普适性。
- **结合内在动机**：利用探索奖励自动驱动选项发现，无需外部奖励或任务监督，符合“无监督技能发现”的前沿趋势。

### 8. 不足与局限：实验覆盖、偏差风险、应用限制

- **实验覆盖不足**：仅报告了三个图像域，未在连续控制任务、高维物理模拟等环境中验证；也未与最新的探索方法（如RND、ICM、Extra Bonus）进行对比。
- **定量细节缺乏**：摘要中未提供具体实验数值（如累计奖励、发现选项数、收敛速度等），仅用“快速探索”等定性描述，说服力不足。
- **可重复性风险**：未公开代码（未提及），算法细节（如特征选择的具体机制、内在动机计算公式）在摘要中过于简略，难以复现。
- **应用限制**：该方法可能依赖于状态特征的可分解性（如离散特征或可编码的连续特征），在特征高度耦合或原始像素空间直接应用中可能面临挑战。此外，对于需要长时间精细控制的任务，抽象选项的粗粒度可能导致性能损失。

（完）
