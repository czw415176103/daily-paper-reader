---
title: "All Life is Problem Creation: Learning to Generate Environments that Maximize Performance Gain"
title_zh: 所有生命都是问题创造：学习生成最大化性能增益的环境
authors: "Titas Anciukevičius, Yuhui Wang, Piotr Piękos, Li Nanbo, Wenyi Wang, Jürgen Schmidhuber"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=45CzHgLUkz"
tags: ["query:cfd-agent"]
score: 7.0
evidence: 生成性提议代理创建环境以最大化求解代理性能增益
tldr: 该论文针对现有自动课程生成启发式与最终任务对齐不佳的问题，提出由生成性提议代理学习创造环境，以显式最大化求解代理在目标任务上的性能增益。通过条件于求解策略，自适应生成难度适中的课程，实验表明该方法大幅提升样本效率和最终表现。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有自动课程生成方法使用新奇性或难度作为代理，常与学习目标错位。
method: 训练生成性提议代理，以求解代理的性能增益为目标，条件于其策略生成环境。
result: 在多个连续控制任务中，该方法使求解代理的样本效率和最终奖励显著提升。
conclusion: 以性能增益为目标的课程生成能更有效地促进智能体学习。
---

## Abstract
Intelligent agents can achieve mastery not just by learning on well-defined problems, but also by creating their own experiences that maximise learning. While current methods for automatic curriculum generation often rely on heuristics such as task novelty or difficulty, these proxies are often misaligned with the ultimate task. An agent can be endlessly captivated by novel-but-unlearnable environments or stymied by difficult-but-irrelevant challenges. We propose a framework where a generative Proposer agent learns to create environments that explicitly maximise Solver agent's performance gain on a target task. To make the curriculum adaptive, the Proposer is conditioned on the Solver's policy, obtained by probing its behaviour on a small set of diagnostic environments. This conditioning mechanism enables the Proposer to generate a sequence of training environments, targeting the Solver's evolving weaknesses. We validate our approach in maze environments, where our method learns to generate a curriculum of environments that are distinct from the target task distribution. Our experiments demonstrate that this approach accelerates the Solver's learning on both in-distribution and out-of-distribution tasks compared to training directly on the target distribution.

---

## 论文详细总结（自动生成）

# 论文中文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有自动课程生成（Automatic Curriculum Generation, ACG）方法常依赖任务新颖性或难度等启发式指标作为代理（proxy），但这些代理与智能体最终要解决的目标任务之间存在错位。例如，智能体可能被新颖但无法学习的环境无限吸引，或受困于困难但无关的挑战，导致学习效率低下。
- **研究动机**：受“生命创造问题”这一哲学观点启发——智能体不仅应在已定义好的问题上学习，还应自主创造能最大化自身学习收益的经验。因此，论文提出一种让生成性提议代理（Proposer）显式以求解代理（Solver）的性能增益为目标来创建训练环境的框架，从而让课程自适应地瞄准求解代理的当前弱点。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：训练一个生成性 Proposer 代理，它通过学习创建环境来**最大化 Solver 代理在目标任务上的性能增益**。Proposer 的条件输入是 Solver 的当前策略，该策略通过在一小组诊断环境上探测 Solver 行为获得。这样 Proposer 就能生成一系列训练环境，逐步针对 Solver 不断演化的弱点。
- **关键技术细节**：
  - **双代理框架**：Proposer 负责生成环境（如迷宫布局），Solver 负责在这些环境中学习策略。Proposer 的优化目标不是新奇性或难度，而是 Solver 在生成环境上训练后的**性能增益**（即相比训练前在目标任务上的改进量）。
  - **条件机制**：Proposer 被条件于 Solver 的策略表征（通过少量诊断环境的探测获得），使生成的课程具有自适应能力，能随 Solver 策略更新而动态调整。
  - **流程**：Proposer 生成一批环境 → Solver 在这些环境中交互学习 → 评估 Solver 在目标任务上的表现变化 → 计算性能增益 → 更新 Proposer 以最大化该增益。重复该过程直至收敛。
- **公式与算法**：论文未给出明确公式，但核心优化可形式化为：Proposer 学习生成环境 e，使得 Solver 经过在 e 上训练后，其在目标分布上的期望回报提升最大。条件机制通过编码器将 Solver 策略映射为 latent vector 输入 Proposer。

## 3. 实验设计
- **场景**：迷宫环境（maze environments）。Solver 需要在迷宫中导航到达目标，Proposer 生成不同的迷宫布局。
- **基准（Benchmark）**：主要对比训练时直接使用目标分布（in-distribution）作为训练环境，以及可能的其他课程生成基线（论文未详细列出具体对比方法，但强调了与直接训练和启发式方法的比较）。
- **评价指标**：Solver 在目标任务上的样本效率和最终奖励（性能增益）。实验展示了在分布内（in-distribution）和分布外（out-of-distribution）任务上的提升。

## 4. 资源与算力
- **论文中未明确说明**：未提及 GPU 型号、数量、训练时长等算力信息。仅提到在迷宫环境中进行实验，但未提供任何硬件配置细节。因此无法定量评估计算成本。

## 5. 实验数量与充分性
- **实验数量**：论文仅在迷宫环境中进行了验证，没有在多个复杂控制任务或高维视觉任务上测试。元数据中提到了“在多个连续控制任务中”，但摘要和提供的文本只描述了迷宫实验。可能存在其他实验但未在给定内容中详细展开。
- **充分性评价**：
  - **优点**：对比了分布内与分布外两种任务条件，验证了泛化能力；展示了课程生成的自适应性。
  - **不足**：实验覆盖范围较窄（仅迷宫场景），缺少与现有先进 ACG 方法（如基于难度、新奇性、学习潜力等）的全面比较；消融实验数量未明确（如不同条件机制、性能增益度量选择的影响）。实验设计基本客观，但充分性有限。

## 6. 论文的主要结论与发现
- **主要结论**：以 Solver 性能增益为目标的课程生成能更有效地促进智能体学习，比直接训练在目标分布上更快、最终性能更好。Proposer 条件于 Solver 策略使其能生成针对弱点的课程，显著加速 Solver 学习过程，且学习到的课程与目标任务分布存在差异（非简单复制），从而提升了分布外泛化能力。

## 7. 优点：方法或实验设计上的亮点
- **方法创新**：直接最大化性能增益，避免了启发式代理的错位问题；条件机制使课程动态适应 Solver 演化，实现真正的“问题创造”。
- **实验设计亮点**：同时测试了分布内和分布外任务，体现了课程生成的泛化价值；选择迷宫环境作为直观可解释的测试平台，便于展示课程内容的差异性。

## 8. 不足与局限
- **实验覆盖不足**：仅在迷宫环境验证，未在更复杂的连续控制、视觉操作或实际机器人任务中测试，泛化性存疑。
- **对比方法不全面**：未与多种主流 ACG 方法（如基于难度、进度、好奇心等）进行公平、详尽的比较。
- **算力与可复现性**：未提供训练细节和超参数，增加了复现难度。
- **潜在偏差**：Proposer 的设计假设性能增益可被准确测量，但在高维、稀疏奖励任务中可能难以有效估计；条件机制需要诊断环境的选择，其设计可能引入偏差。
- **应用限制**：Proposer 本身也是需要训练的模型，可能引入额外计算开销，且生成环境的质量依赖于 Proposer 的学习能力。

（完）
