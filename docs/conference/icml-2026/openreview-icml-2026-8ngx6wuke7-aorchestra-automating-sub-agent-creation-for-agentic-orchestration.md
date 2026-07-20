---
title: "AOrchestra: Automating Sub-Agent Creation for Agentic Orchestration"
title_zh: AOrchestra：自动化子智能体创建以实现智能体编排
authors: "Jianhao Ruan, Zhihao Xu, Yiran Peng, Fashen Ren, Zhaoyang Yu, Xinbing Liang, Jinyu Xiang, Yongru Chen, Bang Liu, Chenglin Wu, Yuyu Luo, Jiayi Zhang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/36c153ef1e0fd61721e2a0c3dc385a3f4da61ec5.pdf"
tags: ["query:cfd-agent"]
score: 4.0
evidence: 提出自动化子智能体创建框架，通过专业化分工改进智能体技能
tldr: 该论文针对现有语言智能体在多任务协作中的灵活性不足，提出AOrchestra框架。其核心是一种统一的智能体抽象，将智能体建模为（模型，任务，工具，上下文）元组，并据此动态创建专用子智能体。实验证明AOrchestra提升了复杂长期任务的成功率和效率。该工作为智能体技能提升和自动化编排提供了新思路。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有子智能体范式缺乏动态抽象，导致适应性差，需要人工设计。
method: 提出统一智能体抽象（模型，任务，工具，上下文），并基于此动态创建专用子智能体。
result: 在多个长期任务上，AOrchestra提升了任务成功率和计算效率。
conclusion: AOrchestra的自动化编排方法显著增强了智能体系统的适应性和技能复用。
---

## Abstract
Language agents have shown strong promise for task automation. Realizing this promise for increasingly complex, long-horizon tasks has driven the rise of a subagent-as-tools paradigm for multi-turn task solving. However, existing designs still lack a dynamic abstraction view of sub-agents, thereby hurting adaptability: sub-agents are either context-isolated threads that lack specialization, or static roles that require human-engineering.
We address this challenge with a unified, framework-agnostic agent abstraction that models any agent as a tuple (Model, Task, Tools, Context). This tuple acts as a compositional recipe for capabilities, enabling the system to spawn specialized executors for each task on demand. 
Building on this abstraction, we introduce an agentic system AOrchestra, where the central orchestrator concretizes the tuple at each step: it curates task-relevant context, selects tools and models, and delegates execution via on-the-fly automatic agent creation.
Such designs enable reducing human engineering efforts, and remain framework-agnostic with plug-and-play support for diverse agents as task executors. It also enables a controllable performance–cost trade-off, allowing the system to approach Pareto-efficient.
Across three challenging benchmarks and environments (GAIA, SWE-Bench, Terminal-Bench), AOrchestra achieves 16.28% relative improvement against the strongest baseline when paired with Gemini-3-Flash.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机与背景）

- **背景**：语言智能体在任务自动化方面展现潜力，但面对日益复杂的长期、多轮任务，现有“子智能体作为工具”（subagent-as-tools）范式存在适应性问题。
- **核心问题**：现有子智能体设计缺乏动态抽象视角——子智能体要么是上下文隔离的线程（缺乏专业化），要么是静态角色（需要人工工程设计），导致系统灵活性不足、人力成本高。
- **研究目标**：提出一种统一、框架无关的智能体抽象方法，实现按需动态生成专用子智能体，提升多任务执行的成功率、效率与适应性。

## 2. 方法论：核心思想与关键技术细节

- **核心思想**：将任意智能体建模为一个四元组 `(Model, Task, Tools, Context)`，该元组作为能力的组合配方，使系统能够按需为每个任务实例化专门执行器。
- **关键技术细节**：
    - **统一智能体抽象**：模型（Model）指定推理引擎，任务（Task）定义目标，工具（Tools）提供操作接口，上下文（Context）包含任务相关历史与环境信息。
    - **中心编排器（Orchestrator）**：在每个步骤中负责任务分解、上下文筛选、工具选择、模型选择，并动态创建专用于当前子任务的智能体。
    - **自动子智能体创建**：通过“即需即用”方式，根据任务需求自动生成子智能体，无需人工预设角色或技能。
    - **框架无关与即插即用**：支持接入多种现成智能体作为任务执行器，降低工程集成成本。
    - **性能-成本可控权衡**：通过调整元组参数（如模型大小、工具数量）实现接近帕累托最优的折中。

## 3. 实验设计

- **数据集与场景**：三个具有挑战性的基准测试环境：
    - **GAIA**（通用AI助手评估）
    - **SWE-Bench**（软件工程任务）
    - **Terminal-Bench**（终端命令执行）
- **对比方法**：与最强基线（未明确具体名称）进行对比，使用 **Gemini-3-Flash** 作为基础模型。
- **评价指标**：任务成功率（可能包含计算效率指标，原文未详述）。

## 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及所使用的GPU型号、数量、训练时长、推理成本等具体算力信息。仅在结果中提到了“性能–成本可控权衡”的概念性优势。

## 5. 实验数量与充分性

- **实验规模**：覆盖三个不同领域的基准测试（GAIA、SWE-Bench、Terminal-Bench），每个任务可能包含多组子任务，但原文未给出具体实验组数。
- **充分性评估**：
    - **优点**：横跨通用AI、软件工程、终端操作三个维度，具有一定多样性。
    - **不足**：未提供消融实验细节（例如对元组中各组件的贡献验证），也未对比多个基线方法的完整列表，缺失统计显著性检验和误差分析。因此，实验的全面性和可重复性证据有限。

## 6. 主要结论与发现

- 在三个基准测试上，AOrchestra 相比最强基线实现了 **16.28% 的相对提升**（基于 Gemini-3-Flash）。
- 自动化子智能体创建框架能够**减少人工工程设计投入**，并**提升复杂长期任务的成功率和计算效率**。
- 统一的智能体抽象有助于**技能复用**和**系统适应性**，使编排过程更加灵活。

## 7. 优点

- **创新性**：提出将智能体建模为四元组，突破了传统静态角色或孤立线程的局限性，实现了动态专业化分工。
- **自动化**：自动创建子智能体，减少人工干预，适合长期、多变的复合任务。
- **框架无关性**：支持即插即用多种基础智能体，降低迁移成本。
- **可控折中**：允许在性能与成本之间进行帕累托优化，具有实际部署价值。

## 8. 不足与局限

- **实验细节缺失**：未公开消融实验、各组件贡献分析、不同模型或工具的对比结果，削弱了结论的严谨性。
- **计算资源未报告**：缺乏算力消耗、推理延迟等关键工程指标，难以评估实际部署可行性。
- **泛化性未知**：仅测试了三种基准，是否适用于更多真实世界场景（如机器人控制、多模态任务）有待验证。
- **仅使用单一基础模型**：结果基于 Gemini-3-Flash，未说明在其他模型（如GPT-4、Llama 3）上的表现，存在模型偏见风险。
- **偏差风险**：未讨论任务复杂性对动态创建开销的影响，也未处理子智能体之间依赖冲突或死锁问题。

（完）
