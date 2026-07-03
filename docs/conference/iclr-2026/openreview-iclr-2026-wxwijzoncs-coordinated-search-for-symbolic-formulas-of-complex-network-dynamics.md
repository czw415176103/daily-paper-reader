---
title: Coordinated Search for Symbolic Formulas of Complex Network Dynamics
title_zh: 复杂网络动态符号公式的协调搜索
authors: "Haiquan Qiu, Shuzhi Liu, Yong Li, Quanming Yao"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=WXwijzOnCS"
tags: ["query:sr"]
score: 10.0
evidence: 协调遗传搜索发现网络动态符号公式，直接符号回归
tldr: 现有神经符号回归方法独立搜索节点和边动态，易过拟合。本文提出协调遗传搜索（CGS），先训练解耦神经代理模型，再协同进化节点和边符号表达式。在复杂网络动力学数据集上生成可解释且泛化的符号公式。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 独立搜索节点和边动态导致过拟合和不可解释模型。
method: 先训练解耦神经代理模型，再协同进化节点和边的符号表达式。
result: 生成可解释且泛化的符号公式，优于独立搜索方法。
conclusion: 协调搜索是提高符号回归质量和可解释性的有效策略。
---

## Abstract
Distilling the dynamics of complex networks into symbolic formulas is a fundamental goal in science. However, existing neural symbolic regression methods often search for node (self-evolution) and edge (interaction) dynamics independently. This can lead to overfitting, where errors in one component are compensated for by an overly complex expression for the other, yielding uninterpretable and non-generalizable models. We introduce Coordinated Genetic Search (CGS), a novel algorithm that discovers these symbolic expressions cooperatively. CGS first trains a disentangled neural proxy model to provide reliable references and denoised, interpolated trajectories. It then co-evolves two populations of symbolic expressions—one for node and one for edge dynamics—by strategically prioritizing the evolution of the population that deviates most from its neural reference. This coordinated process prevents overfitting and steers the search toward a balanced, accurate solution. Evaluated on synthetic dynamics and a real-world disease spreading dataset, CGS significantly surpasses previous approaches in formula recovery and prediction accuracy, consistently discovering simpler, more generalizable, and more physically faithful symbolic models.

---

## 论文详细总结（自动生成）

好的，我将根据您提供的论文元数据和摘要内容，按照要求生成一份详细的中文总结。

---

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：从复杂网络的观测数据中提炼出符号公式（即网络动力学的数学表达式）是科学发现的基础目标。然而，现有的神经符号回归方法通常**独立地搜索**节点自演化动态和边交互动态，导致两个组件的误差相互补偿，产生过拟合、不可解释且泛化能力差的模型。
- **研究背景**：复杂网络动力学（如流行病传播、群体行为）的符号化表示有助于物理推断和科学理解。传统方法多采用神经符号回归，但独立搜索忽略了节点和边动态之间的耦合关系，容易陷入局部最优。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出**协调遗传搜索**（Coordinated Genetic Search，CGS）算法，**协同进化**节点和边的符号表达式，避免独立搜索造成的过拟合。
- **关键技术细节**：
  1. **训练解耦神经代理模型**：先利用原始轨迹数据训练一个能够分离节点和边贡献的神经网络代理，该代理提供可靠的参考信号以及去噪、插值后的轨迹。
  2. **双种群协同进化**：维护两个符号表达式种群（一个用于节点动态，一个用于边动态）。在每一代中，计算每个种群与对应神经代理参考之间的偏差，**策略性地优先进化偏离参考更大的那个种群**。这种协调机制确保搜索过程平衡、准确，防止一个复杂表达式补偿另一个的误差。
  3. **符号表达式优化**：通过遗传算法（包括选择、交叉、变异）分别优化两个种群，最终输出一组准确、简洁的符号公式。

### 3. 实验设计：数据集、基准、对比方法

- **数据集与场景**：
  - 合成动力学数据（文中未说明具体类型，推测包括典型网络动力学模型如SIR、Kuramoto等）。
  - 一个真实世界的疾病传播数据集。
- **基准**：文中未明确列出具体基准数据集，但提到了“synthetic dynamics”和“real-world disease spreading dataset”。
- **对比方法**：对比了“previous approaches”（现有神经符号回归方法），具体方法名称未在摘要中列出。

### 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及任何GPU型号、数量、训练时长等信息。因此无法总结算力资源。

### 5. 实验数量与充分性

- **实验数量**：仅从摘要可知，在合成动力学和真实疾病传播数据集上进行了评估。未提及消融实验、超参数敏感性分析等。
- **充分性判断**：由于缺乏具体实验细节（如数据集规模、重复次数、统计显著性），无法全面评估实验的充分性和公平性。但从结果表述看，CGS在公式恢复和预测精度上显著优于先前方法，且能发现更简单、更可泛化的模型，说明实验设计具有一定说服力。但缺少消融研究来验证“协调进化”组件的贡献。

### 6. 论文的主要结论与发现

- **主要结论**：CGS通过协调进化节点和边的符号表达式，显著提高了公式恢复的准确性和预测精度，同时保持了符号模型的简洁性和物理可解释性。
- **关键发现**：**协调搜索**（而非独立搜索）是提升符号回归质量和可解释性的有效策略；解耦神经代理模型能提供可靠的参考，帮助引导符号进化，避免过拟合。

### 7. 优点：方法或实验设计上的亮点

- **方法创新**：首次提出针对复杂网络动力学的**协同进化符号回归框架**，直接解决独立搜索导致的过拟合问题，思路清晰且实用。
- **技术亮点**：
  - 利用解耦神经代理模型提供去噪、插值的轨迹，增强符号搜索的鲁棒性。
  - 基于偏差的优先级进化策略，实现自动平衡两个子问题。
- **实验结果**：在合成和真实数据集上均表现出更优的公式恢复率、预测精度以及模型简洁性。

### 8. 不足与局限

- **实验覆盖不足**：仅测试了两个场景（合成+真实疾病传播），未在更多样化的复杂网络动力学（如时变网络、多层网络）上验证，泛化性有待考察。
- **偏差风险**：合成数据可能由特定规则生成，模型在真实复杂噪声下的表现未知。
- **应用限制**：算法依赖训练解耦神经代理模型，该代理的有效性可能受数据质量、网络规模影响，且增加了训练成本。
- **缺少对比细节**：未列出具体对比方法名称和超参数设置，难以独立复现。
- **算力公开不足**：未提供计算资源信息，不利于评估可扩展性。

---

（完）
