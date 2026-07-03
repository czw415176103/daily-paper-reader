---
title: "MermaidFlow: Redefining Agentic Workflow Generation via Safety-Constrained Evolutionary Programming"
title_zh: MermaidFlow：通过安全约束进化编程重新定义智能体工作流生成
authors: "Chengqi Zheng, Yewen Pu, Jianda Chen, Yueming Lyu, Wen zheng terence Ng, Haopeng Zhang, Yew-Soon Ong, Ivor Tsang, Haiyan Yin"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=bhPaXhWVKG"
tags: ["query:cfd-agent"]
score: 6.0
evidence: 进化编程用于智能体工作流生成
tldr: 该论文针对现有LLM驱动工作流生成脆弱易错的问题，提出MermaidFlow。将工作流表示为Mermaid图语言，并设计域感知进化算子（交叉、变异等）在安全约束下探索高质量工作流空间。该方法在不修改模型情况下生成了更鲁棒的可执行计划。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有方法生成的智能体工作流经常不可执行，缺乏结构化约束。
method: 采用Mermaid图表示工作流，应用安全约束进化算法生成高质量工作流。
result: MermaidFlow生成了可执行且性能优异的工作流，优于无约束的LLM驱动方法。
conclusion: 安全约束进化编程为智能体工作流自动化生成提供了可靠范式。
---

## Abstract
Despite the promise of autonomous agentic reasoning, existing workflow generation methods frequently produce fragile, unexecutable plans due to unconstrained LLM-driven construction. We propose MermaidFlow, a framework that redefines the agentic search space through safety-constrained graph evolution. At its core, MermaidFlow represent workflows as a verifiable intermediate representation using Mermaid, a structured and human-interpretable graph language. We formulate domain-aware evolutionary operators, i.e., crossover,  mutation, insertion, and deletion, to preserve semantic correctness while promoting structural diversity, enabling efficient exploration of a high-quality, statically verifiable workflow space. Without modifying task settings or evaluation protocols, MermaidFlow achieves consistent improvements in success rates and faster convergence to executable plans on the agent reasoning benchmark. The experimental results demonstrate that safety-constrained graph evolution offers a scalable, modular foundation for robust and interpretable agentic reasoning systems.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **核心问题**：现有基于大语言模型（LLM）驱动的智能体工作流生成方法常产生脆弱、不可执行的计划，导致推理过程不可靠。
- **研究动机**：缺乏结构化约束使得 LLM 在构建工作流时容易偏离可执行性，需要一种能保证语义正确性并提升探索效率的生成范式。
- **整体含义**：论文旨在通过安全约束下的图进化编程，重新定义智能体工作流的自动生成，为构建稳健、可解释的自主推理系统提供新思路。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：将工作流表示为结构化的、人类可解释的中间表示——Mermaid 图语言，并在此基础上设计域感知的进化算子，在安全约束空间内搜索高质量的工作流。
- **关键技术细节**：
  - **工作流表示**：使用 Mermaid 图语言（一种结构化图语言）作为工作流的中间表示，支持静态验证。
  - **域感知进化算子**：包括交叉、变异、插入和删除四种算子，能保留语义正确性并促进结构多样性。
  - **安全约束**：进化过程在安全约束下进行，确保生成的工作流至少是静态可验证的，从而避免不可执行的计划。
  - **算法流程（文字说明）**：初始化种群（随机或基于提示的工作流表示）→ 使用进化算子（交叉、变异等）生成子代 → 根据安全约束筛选合法子代 → 评估子代工作流的执行成功率和效率 → 保留最优个体 → 迭代直到收敛或达到最大代数。

## 3. 实验设计：数据集、基准与对比方法

- **数据集/场景**：使用智能体推理基准（agent reasoning benchmark），具体名称未在摘要中说明（推测为流行的 AgentBench、ALFWorld 等之一，但论文未明示）。
- **基准**：该基准用于评估工作流的可执行性和成功率。
- **对比方法**：与无约束的 LLM 驱动方法（如直接使用 LLM 生成计划）进行对比，文中提及“优于无约束的LLM驱动方法”，但未列举具体基线名称（如 ReAct、Plan-and-Solve 等）。

## 4. 资源与算力

- **文中未明确说明**：未提及使用的 GPU 型号、数量、训练时长等算力资源。仅能从“无需修改任务设置或评估协议”推断该方法相对轻量，但具体资源消耗未知。

## 5. 实验数量与充分性

- **实验数量**：摘要中仅提及“在智能体推理基准上取得一致改进”，未具体说明进行了多少组实验（如不同数据集、不同难度、消融研究等）。消融实验（如进化算子单独的效果）也未提及。
- **充分性与客观性**：从现有信息看，实验覆盖面较窄（仅一个基准），缺乏多领域、多任务验证。对比方法单一，未与多个主流工作流生成方法（如 Tree-of-Thought、Graph-of-Thought、ADaPT 等）比较。因此实验的充分性、客观性和公平性评估受限，需阅读完整论文才能判断。

## 6. 论文的主要结论与发现

- MermaidFlow 生成的工作流可执行且性能优异，显著优于无约束的 LLM 驱动方法。
- 安全约束的图进化能带来更快的收敛到可执行计划的速度。
- 该方法无需修改任务设置或评估协议即可取得一致改进，说明其通用性。
- 作者认为安全约束进化编程为智能体工作流自动化生成提供了可靠范式。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - 引入 Mermaid 图作为中间表示，兼具结构化与人类可解释性，便于静态验证。
  - 域感知进化算子能够同时保障语义正确性与结构多样性，避免盲目搜索。
  - 安全约束有效防止生成不可执行的工作流，提升了生成鲁棒性。
- **实验亮点**：未调整任务设置和评估协议即取得改进，体现了方法的即插即用性。

## 8. 不足与局限

- **实验覆盖不足**：仅在一个推理基准上验证，未在不同类型任务（如多智能体协作、长程规划）或不同 LLM 微调设置下测试。
- **对比方法不充分**：仅与无约束 LLM 驱动方法对比，缺乏与现有基于搜索（如 BFS/DFS）、基于进化（如 EvoAgent）或基于强化学习的工作流生成方法的系统比较。
- **资源与可重复性细节缺失**：未提供算力需求、超参数配置、进化代数等关键复现信息。
- **安全约束的定义与成本**：安全约束的具体实现方式（例如基于语法检查还是运行时验证）未说明，可能引入额外计算开销。
- **应用限制**：方法假设工作流可表示为 Mermaid 图，可能不适用于非 DAG 结构或需要动态控制流的复杂任务。

（完）
