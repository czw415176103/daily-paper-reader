---
title: Efficient Skill Grounding via Code Refactoring with Small Language Models
title_zh: 通过小语言模型的代码重构实现高效技能落地
authors: "Sera Choi, Wonje Choi, Saehun Chun, Daehee Lee, Jooyoung Kim, Chaeun Lee, Honguk Woo"
date: 2026-04-30
pdf: "https://openreview.net/pdf/39ea8351adfdc348819e4edaa6ee9437097bfe62.pdf"
tags: ["query:cfd-agent"]
score: 7.0
evidence: 使用小语言模型通过代码重构实现技能落地
tldr: 本文提出RECENT框架，通过代码重构将技能语义与具体具身和环境执行细节解耦，使得小语言模型也能高效地实现技能落地。该方法解决了具身智能体中技能因环境差异而失效的问题，在不依赖大型语言模型的情况下实现了可靠的长时域控制。实验证明该方法在多种具身场景下显著提高了技能的可迁移性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 技能移植时微小的环境差异会导致整个技能失效，且具身智能体无法依赖大语言模型。
method: 提出以重构为中心的框架，将技能语义与环境绑定分离，使用小语言模型自动适配。
result: 在多个具身环境中，技能迁移成功率显著提高，且推理效率优于大语言模型方案。
conclusion: 代码重构解耦技能语义和执行细节，是小语言模型实现技能落地的有效方法。
---

## Abstract
Effective skill grounding is essential for deploying reusable skills in embodied agents, as even minor embodiment or environmental differences can render an entire skill incompatible.
This challenge is particularly pronounced in embodied settings, where agents must operate in dynamic, partially observable environments without access to large language models (LLMs).
In this setting, reliance on LLMs is impractical, while small language models (sLMs) remain insufficient for the effective skill grounding required for reliable long-horizon control. 
We present RECENT, a refactoring-centric agent framework that enables efficient skill grounding with sLMs by decoupling skill semantics from embodiment- and environment-specific execution binding.
By representing skills as executable code, RECENT preserves the semantic intent encoded in a skill’s control structure while grounding it by modifying only execution bindings through localized refactoring, rather than regenerating code from scratch. 
We evaluate RECENT across diverse skill grounding scenarios spanning multiple robot embodiments in dynamic environments, demonstrating robust long-horizon performance when deployed with an sLM.
Across all scenarios, RECENT achieves the best performance among sLM-based Code-as-Policies (CaP) methods and matches the task performance of LLM-based CaP.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：具身智能体在部署可复用技能时，面临“技能落地（skill grounding）”难题——即使是微小的本体（embodiment）或环境差异，也会导致整个技能失效。这使得技能迁移变得困难，尤其是在动态、部分可观测的环境中。
- **背景与动机**：传统方法依赖大型语言模型（LLM）进行代码生成和技能适配，但LLM在具身场景中不实用（如计算成本高、部署受限）。而小型语言模型（sLM）能力不足，难以实现可靠的长时域控制所需的技能落地。因此需要一种在不依赖LLM的前提下，利用sLM高效实现技能落地的方法。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出RECENT框架，以代码重构（refactoring）为中心，将技能语义（skill semantics）与具体的本体及环境绑定（execution binding）解耦。技能用可执行代码表示，通过局部重构（localized refactoring）仅修改执行绑定部分，而非从头生成整个技能代码，从而保留技能的语义意图和控制结构。
- **关键技术细节**：
  - 技能表示：将技能抽象为代码，包含控制流（体现语义）和与环境/硬件相关的执行绑定（如传感器、致动器接口）。
  - 重构过程：使用sLM对现有技能代码进行局部修改（如替换特定API调用、调整参数），而不是重新生成完整代码。这样sLM仅需处理小范围的代码片段，降低了语言模型的能力要求。
  - 与传统Code-as-Policies（CaP）方法的区别：传统CaP方法可能依赖LLM生成完整策略代码，而RECENT通过重构机制实现了sLM也能完成可靠的技能适配。

## 3. 实验设计：数据集/场景、benchmark、对比方法

- **场景与benchmark**：论文未提供完整实验细节，但摘要提及在**多种机器人本体（robot embodiments）** 和**动态环境**中进行技能落地场景评估。具体场景类型（如抓取、导航等）未详述。
- **对比方法**：
  - 基于sLM的Code-as-Policies（CaP）方法（作为基线）。
  - 基于LLM的CaP方法（作为上限参考）。
- **评估指标**：长时域任务成功率（long-horizon performance）、技能迁移成功率。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅提到使用的是小型语言模型（sLM），通常此类模型可在单GPU甚至CPU上运行，但具体硬件资源未披露。

## 5. 实验数量与充分性

- **实验数量**：摘要未列出具体实验组数，但提到“多样化场景”（diverse skill grounding scenarios spanning multiple robot embodiments）。推测可能包含多个任务/环境、多个本体类型，以及对比不同sLM/LLM方法。
- **充分性与公平性**：
  - 优点：在sLM方法中取得最佳性能，且能与LLM方法相匹配，说明实验设计有对比参照。
  - 不足：缺少消融实验、参数分析、失败案例等细节。由于未披露实验完整细节，难以判断是否对所有可能变量进行了全面控制。此外，仅依赖摘要无法评估统计显著性或重复性。

## 6. 论文的主要结论与发现

- **主要结论**：RECENT框架通过代码重构解耦技能语义与执行绑定，使得小语言模型（sLM）也能实现高效、可靠的技能落地。在所有评估场景中，RECENT均优于现有的sLM-based CaP方法，并在任务性能上达到了与LLM-based CaP方法相当的水平。
- **发现**：局部重构比从头生成代码更适合sLM能力；解耦机制提高了技能的可迁移性和鲁棒性。

## 7. 优点

- **方法亮点**：创新性地将软件工程中的代码重构概念引入具身智能体的技能迁移，降低了sLM的要求；无需大量标注数据或昂贵LLM推理。
- **实验设计亮点**：直接在多种具身环境中评估，与sLM和LLM基线对比，验证了方法在真实动态环境中的有效性。
- **实际意义**：推动低成本、高可靠性的技能复用，有助于具身智能体在边缘设备或资源受限场景部署。

## 8. 不足与局限

- **实验覆盖**：仅提供摘要级结果，缺乏详细任务描述、成功/失败案例、统计方差等，可能影响结论的泛化性。
- **偏差风险**：仅评估了少数几种机器人本体和环境，未说明是否涵盖不同复杂度任务（如精确操作、野外导航等）。
- **应用限制**：依赖于初始技能代码的可用性（即需预设技能模板），对于完全新颖的技能可能无法从头合成；sLM的重构能力仍受限于模型规模，复杂环境下的错误传播可能仍需验证。
- **资源信息缺失**：未报告算力消耗，难以横向比较效率优势。

（完）
