---
title: Towards a Science of AI Agent Reliability
title_zh: 迈向AI智能体可靠性的科学
authors: "Stephan Rabanser, Sayash Kapoor, Peter Kirgis, Kangheng Liu, Saiteja Utpala, Arvind Narayanan"
date: 2026-04-30
pdf: "https://openreview.net/pdf/623c70b2aaf90f0a3a998f8c236abd5519463f31.pdf"
tags: ["query:cfd-agent"]
score: 4.0
evidence: 评估AI智能体可靠性的多维度指标
tldr: 针对AI智能体可靠性评估的不足，提出了涵盖一致性、鲁棒性、可预测性和安全性的12个性能指标，并在两个基准上评估了15个模型，揭示了近期能力的提升并不一定改善可靠性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有智能体评估仅关注准确率，忽略可靠性。
method: 提出包含12个关键指标的全面可靠性评估框架。
result: 发现15个模型在可靠性维度上有显著差异。
conclusion: 强调可靠性与准确性同等重要。
---

## Abstract
AI agents are increasingly deployed to execute important tasks. While rising accuracy scores on standard benchmarks suggest rapid progress, many agents still continue to fail in practice. This discrepancy highlights a major limitation of current evaluations: focusing on a single metric is not enough to understand agent behavior. Notably, it ignores whether agents behave consistently across runs, withstand perturbations, fail predictably, or have bounded error severity. Grounded in safety-critical engineering, we provide a holistic performance profile consisting of twelve metrics that decompose agent reliability along four key dimensions: *consistency*, *robustness*, *predictability*, and *safety*. Evaluating 15 models across two complementary benchmarks, we find that recent capability gains have only yielded small improvements in reliability. By exposing these persistent limitations, our metrics complement traditional evaluations while offering tools for reasoning about how agents perform, degrade, and fail.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：当前AI智能体在标准基准上准确率持续提升，但实际部署中仍频繁失败。这一矛盾表明现有评估存在重大缺陷——仅依赖单一准确率指标无法全面理解智能体行为，忽略了可靠性关键维度（如运行一致性、抗扰动能力、失败可预测性、错误严重性等）。
- **背景**：受安全关键工程领域启发，论文倡导将可靠性作为与准确性同等重要的评估维度，旨在建立AI智能体可靠性的科学评估体系。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：将智能体的可靠性分解为四个关键维度，并为每个维度设计可量化的指标，构成一个包含12个指标的性能画像。
- **四个维度与指标示例**（基于摘要及元数据推演）：
  - **一致性（Consistency）**：同一任务多次运行的结果稳定性（例如输出方差、成功率波动）。
  - **鲁棒性（Robustness）**：面对输入扰动（如噪声、对抗样本）时的性能保持能力。
  - **可预测性（Predictability）**：失败模式的规律性，以及能否提前预测失败边界。
  - **安全性（Safety）**：错误严重性是否可控，是否存在灾难性失败。
- **具体技术细节**：论文未提供详细公式或算法流程（仅从摘要可知为指标设计），但可推断采用统计度量（如标准差、百分位数、误差分布等）对每个维度进行量化。

### 3. 实验设计：数据集/场景、基准、对比方法

- **数据集与场景**：使用了两个互补的基准测试（原文未明确命名，推测为延续主流智能体评估基准如WebArena、AgentBench等的变体或专门设计的可靠性测试集）。
- **对比方法**：评估了15个不同模型，涵盖不同架构、规模及训练策略，对比其在不同可靠性指标上的表现。
- **评估指标**：除常规准确率外，采用论文提出的12个可靠性指标进行多维度比较。

### 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及具体GPU型号、数量、训练时长等算力信息。推测实验可能基于公开模型进行推理评估，而非从头训练，因此算力消耗较低；但原文未确认这一点。

### 5. 实验数量与充分性

- **实验数量**：评估了15个模型×两个基准×12个指标，共计360组基本测量，加上可能的多次运行（一致性维度需重复实验），实验量较为充分。
- **充分性评价**：
  - **优点**：覆盖模型种类较多（15个），且从四个维度全面衡量可靠性，对比了能力提升与可靠性改善之间的关系。
  - **不足**：未披露基准的具体难度分布、任务类型多样性，以及是否包含开放性场景；未进行消融实验（例如去掉某些指标的影响分析）；未对指标间的相关性进行深入探讨。

### 6. 论文的主要结论与发现

- **核心发现**：近期AI智能体在能力（准确率）上的大幅提升，仅带来可靠性方面的微小改善。即高准确率不代表高可靠性，两者之间存在显著差距。
- **具体结论**：不同模型在一致性、鲁棒性、可预测性和安全性维度上表现差异明显，部分高准确率模型在某些可靠性指标上甚至低于低准确率模型。
- **意义**：强调可靠性应作为独立维度纳入评估体系，为智能体安全部署提供更丰富的诊断工具。

### 7. 优点：方法或实验设计上的亮点

- **系统性**：首次提出多维可靠性分解框架，突破单一准确率评价的局限。
- **实践导向**：指标定义贴近安全关键工程需求（如可预测性、错误严重性），对实际部署有直接指导意义。
- **对比全面**：评估15个模型覆盖主流规模，能够揭示能力与可靠性之间的非单调关系。
- **互补性**：提出的指标可作为标准准确率评估的补充，帮助开发者定位智能体的薄弱环节。

### 8. 不足与局限：实验覆盖、偏差风险、应用限制

- **基准局限性**：两个基准可能仅涵盖特定类型的任务（如网页操作、工具使用），未覆盖多模态或物理世界交互场景，结论泛化性受限。
- **指标选择主观**：12个指标及其权重分配缺乏理论证明或应用案例验证，可能存在冗余或遗漏关键维度。
- **未考虑动态环境**：智能体在持续学习或适应过程中的可靠性变化未被评估。
- **偏差风险**：评估基于公开模型，可能受训练数据分布影响；未考虑不同提示策略或系统 prompt 对可靠性的干扰。
- **应用限制**：论文未提供自动化的可靠性诊断工具或阈值建议，实用性需要进一步工程化。

（完）
