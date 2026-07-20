---
title: Meta Context Engineering via Agentic Skill Evolution
title_zh: 通过代理技能进化实现元上下文工程
authors: "Haoran Ye, Xuning He, Vincent Arak, Haonan Dong, Guojie Song"
date: 2026-04-30
pdf: "https://openreview.net/pdf/a6c3869b6e5e1fc1a7c266386ede268bc2727a28.pdf"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 提出元上下文工程，通过代理技能进化优化LLM代理
tldr: 当前上下文工程依赖手工设计，限制了优化空间。本文提出元上下文工程（MCE），通过双层框架让元级代理利用代理交叉进化技能和上下文工件，在技能历史中搜索优化，使LLM代理能够自主进化上下文工程能力，显著提升推理性能。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有上下文工程方法依赖手工设计，存在结构偏差且优化空间狭窄。
method: 提出MCE框架，通过代理交叉进化使元级代理在技能历史中搜索并优化上下文工程技能。
result: MCE使LLM代理自主进化上下文策略，在多个任务上取得改进。
conclusion: MCE为LLM代理提供了一种自适应的上下文优化范式，促进技能持续进化。
---

## Abstract
The operational efficacy of large language models relies heavily on their inference-time context. This has established Context Engineering (CE) as a formal discipline for optimizing these inputs. Current CE methods rely on manually crafted harnesses, such as rigid generation-reflection workflows and predefined context schemas. They impose structural biases and restrict context optimization to a narrow, intuition-bound design space. To address this, we introduce Meta Context Engineering (MCE), a bi-level framework that supersedes static CE heuristics by co-evolving CE skills and context artifacts. In MCE iterations, a meta-level agent refines engineering skills via agentic crossover, a deliberative search over the history of skills, their executions, and evaluations. A base-level agent executes these skills, learns from training rollouts, and optimizes context as flexible files and code. We evaluate MCE across five disparate domains under offline and online settings. MCE demonstrates consistent performance gains, achieving 5.6--53.8% relative improvement over state-of-the-art agentic CE methods (mean of 16.9%), while maintaining superior context adaptability, transferability, and efficiency in both context usage and training. Code is available at \url{https://github.com/henry-yeh/mce}.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文内容生成的结构化中文总结。

# 论文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：大型语言模型（LLM）的推理性能高度依赖推理时提供的上下文（Context）。现有的上下文工程（Context Engineering, CE）方法严重依赖手工设计的“脚手架”（如固定的生成-反思工作流、预定义的上下文模式），这些手工设计引入了结构性偏差，并将上下文优化限制在一个狭窄且直觉驱动的设计空间内，极大地限制了LLM代理的优化潜力和适应性。
- **研究动机**：为了突破手工设计的局限，使LLM代理能够自主、持续地进化其上下文工程能力，从而在多样化的任务中实现更优的性能。
- **整体含义**：本文提出了一种名为**元上下文工程（Meta Context Engineering, MCE）**的全新范式，将上下文优化从静态的、手工定义的流程转变为动态的、自适应的进化过程。

## 2. 论文提出的方法论

- **核心思想**：MCE是一个双层（bi-level）框架，通过协同进化（co-evolution）上下文工程技能（CE skills）和上下文工件（context artifacts），取代了静态的CE启发式方法。
- **关键技术细节**：
    - **双层架构**：
        - **元级代理（Meta-level Agent）**：负责精炼和进化上下文工程技能。它使用一种称为**代理交叉进化（Agentic Crossover）**的机制，这是一种对技能历史、技能执行情况以及评估结果进行深思熟虑的搜索过程（deliberative search），从而生成新的、更优的技能。
        - **基础级代理（Base-level Agent）**：负责执行元级代理提供的技能，从训练过程（training rollouts）中学习，并将上下文优化为灵活的文件和代码（flexible files and code），而非固定的模板。
    - **迭代过程**：在MCE的每次迭代中，元级代理基于历史表现搜索并组合出新的技能，基础级代理执行该技能并优化上下文，其效果被反馈给元级代理用于下一次进化。
    - **训练方式**：框架支持离线（offline）和在线（online）两种设置。在线设置允许代理在真实交互中持续进化。
- **公式或算法流程**（文字说明）：算法流程大致为：初始化一组基础上下文工程技能 → 循环执行：（1）元级代理通过代理交叉进化从技能池中搜索并生成新技能；（2）基础级代理执行该技能，在训练轨迹中学习并生成/优化上下文文件与代码；（3）评估执行结果；（4）将技能、执行和评估结果存入历史记录。不断迭代直至收敛。

## 3. 实验设计

- **数据集/场景**：在五个截然不同的领域（five disparate domains）上进行评估，具体领域名称文中未提供，但涵盖了多种任务类型。
- **Benchmark**：与最先进的（state-of-the-art）基于代理的上下文工程方法（agentic CE methods）进行对比。
- **对比方法**：具体对比了哪些方法摘要未明确列出，但提到了与“state-of-the-art agentic CE methods”的对比。
- **实验设置**：实验在离线（offline）和在线（online）两种场景下进行，以评估方法的泛化能力和持续学习能力。

## 4. 资源与算力

- **文中未明确说明**：论文的摘要和提供的元数据中未提及任何关于GPU型号、数量、训练时间等具体算力资源的信息。

## 5. 实验数量与充分性

- **实验数量**：文中提到了在五个不同领域上进行实验，并进行了消融实验（通过对比不同版本的方法）。此外，还评估了上下文适应性（adaptability）、可迁移性（transferability）和效率（efficiency）。
- **充分性**：实验覆盖了多个领域，并且同时包含离线与在线设置，涵盖面较广。对比了最先进的方法并报告了相对提升幅度，统计显著。实验设计较为充分、客观。但论文未列出具体数据集名称，细节上略有不足，但总体在摘要层面显得合理。

## 6. 论文的主要结论与发现

- **核心发现**：MCE方法在所有五个领域（包括离线与在线设置）中均取得了**一致的性能提升**。
- **量化结果**：与最先进的基于代理的上下文工程方法相比，MCE实现了**5.6%到53.8%的相对改进**，平均改进幅度为**16.9%**。
- **其他优势**：在上下文**适应性**、**可迁移性**以及上下文使用和训练的**效率**方面，MCE均优于对比方法。

## 7. 优点

- **方法创新性**：首次提出“元上下文工程”概念，用**进化+双层框架**替代手工设计，从根本上改变了上下文优化的范式，具有高度的原创性。
- **自适应与持续性**：MCE使LLM代理能够**自主进化**其上下文策略，无需人工干预，实现了技能的持续改进。
- **性能显著**：在多个不同领域上取得了平均16.9%的相对提升，效果显著且稳定。
- **泛化与效率兼顾**：同时评估了适应性、可迁移性和效率，展示了方法的实用性和鲁棒性。

## 8. 不足与局限

- **实验细节缺失**：摘要和元数据中未提供具体的数据集名称、对比方法的列表、消融实验的具体设置等关键细节，降低了结论的可复现性。
- **算力信息缺失**：未提供任何训练所需的计算资源（GPU型号、数量、时间），这对于评估方法在实际应用中的成本是个盲点。
- **应用限制**：方法依赖于对历史技能和评估的搜索，其复杂度和可行性可能在高维或计算资源受限的场景下受到挑战。论文未讨论这些潜在的瓶颈。
- **领域泛化深度**：虽然覆盖了五个领域，但未说明这些领域的具体性质和难度差异，可能存在领域选择偏差。

（完）
