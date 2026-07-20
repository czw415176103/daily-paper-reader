---
title: "AutoSkills: Automatically Constructing Skill Knowledge Bases for Agents"
title_zh: AutoSkills：自动构建智能体技能知识库
authors: "Chenxi Wang, Zhuoyun Yu, Xin Xie, Wuguannan YAO, Runnan Fang, Shuofei Qiao, Kexin Cao, Guozhou Zheng, Xiang Qi, Peng Zhang, Shumin Deng"
date: 2026-01-23
pdf: "https://openreview.net/pdf/f79dafaf2db2ffad65376badc6fc56b450e70c63.pdf"
tags: ["query:cfd-agent"]
score: 9.0
evidence: 自动构建智能体技能知识库以提升其能力
tldr: 针对自进化范式下每个智能体孤立学习、效率低且泛化差的问题，提出AutoSkills框架，通过多层级技能设计、迭代技能精炼和跨环境技能迁移，自动构建即插即用的技能库。该框架将原始轨迹转化为战略计划、功能技能和原子技能的三层体系，使得技能能够跨智能体复用与泛化。实验证明AutoSkills显著提升了智能体在新任务上的适应速度和成功率。
source: ICML-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有自进化范式下智能体孤立学习，从有限经验和数据中挖掘技能，效率低且泛化差。
method: 提出AutoSkills，通过多层级技能设计、迭代精炼和跨环境迁移的自动化流水线，预先构建通用技能库。
result: 在多个LLM智能体基准测试中，AutoSkills构建的技能库使智能体在新任务上快速适应，性能显著提升。
conclusion: AutoSkills打破了单智能体自进化的局限性，为构建可迁移的智能体技能体系提供了新范式。
---

## Abstract
Learning from experience is crucial for creating more capable LLM-based agents. However, the prevailing self-evolving paradigm is fundamentally inefficient: it forces each agent to learn in isolation, redundantly mining experiences from its own limited capabilities and scarce training data, resulting in expertise that fails to generalize.
To break this cycle, we introduce AutoSkills, a framework that autonomously pre-builds a plug-and-play skill library. AutoSkills operates through a fully automated pipeline built on three synergistic innovations: **i) Multi-Level Skills Design**, which distills raw trajectories into three-tiered hierarchy of strategic plans, functional skills, and atomic skills; **ii) Iterative Skills Refinement**, which automatically revises skills based on execution feedback to continuously improve library quality; and **iii) Exploratory Skill Expansion**, which proactively generates and validates novel skills to expand coverage beyond seed training data.
Using this framework, we construct a reliable plug-and-play skill library using a state-of-the-art agent, GLM-4.6.
We conduct extensive experiments on challenging long-horizon, user-interactive benchmarks, including AppWorld, BFCL-v3, and $\tau^2$-Bench, demonstrating the effectiveness of AutoSkills. 
We also provide strategic insights into how experience transfer impacts model performance.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前大语言模型（LLM）智能体普遍采用“自进化（self-evolving）”范式——每个智能体孤立地从自身有限的经验和稀缺的训练数据中挖掘技能，导致学习效率低下、泛化能力差，且技能难以跨智能体复用。
- **研究动机**：为了打破这一低效循环，研究者希望找到一种自动化的、可复用的方式，预先构建通用的技能知识库，使新智能体能够“即插即用”，快速适应新任务。
- **整体含义**：论文提出 **AutoSkills** 框架，旨在通过自动化流水线，将原始轨迹（raw trajectories）转化为结构化的多层级技能库，从而实现技能的跨环境迁移与泛化，提升智能体的能力上限。

### 2. 论文提出的方法论：核心思想、关键技术细节、算法流程

- **核心思想**：构建一个完全自动化的流水线，将智能体执行任务时的原始轨迹（包括成功与失败的经验）提炼为三层技能体系，并通过迭代优化和主动扩展，生成可靠且可迁移的技能知识库。
- **关键技术细节**：
  - **i) 多层级技能设计（Multi-Level Skills Design）**：将原始轨迹蒸馏为三个层次的技能：
    - **战略计划（Strategic Plans）**：高层次的行动序列或策略，描述任务的整体执行逻辑。
    - **功能技能（Functional Skills）**：可复用的功能模块，如特定API调用、数据操作等。
    - **原子技能（Atomic Skills）**：最基础、不可再分的操作单元，例如单个函数调用或简单推理步骤。
  - **ii) 迭代技能精炼（Iterative Skills Refinement）**：根据实际执行反馈自动修正技能库中的错误或低效技能，持续提升库的质量。
  - **iii) 探索性技能扩展（Exploratory Skill Expansion）**：主动生成并验证新技能，以覆盖超出初始训练数据范围的任务场景，增强技能库的泛化能力。
- **算法流程（文字说明）**：
  1. **输入**：智能体（本文使用 GLM-4.6）在多个任务上的原始轨迹（包括状态、动作、结果）。
  2. **阶段1：蒸馏** → 使用LLM自动将轨迹分解为三层技能（战略→功能→原子），并建立层级关联。
  3. **阶段2：精炼** → 在新任务中测试技能库，收集失败案例；利用反馈信号（如执行结果、环境报错）自动修正技能（例如调整参数、合并冗余技能）。
  4. **阶段3：扩展** → 通过提示工程主动探索未覆盖的场景，生成候选技能并在虚拟环境中验证，通过后加入库中。
  5. **输出**：一个即插即用的技能库，可供新智能体直接加载使用。

### 3. 实验设计：使用数据集/场景、基准、对比方法

- **数据集/场景**：三个具有挑战性的长时程、用户交互式基准测试：
  - **AppWorld**：模拟移动端应用交互的复杂任务。
  - **BFCL-v3**：伯克利函数调用库第三版，涉及多步骤API调用。
  - **τ²-Bench**：面向多模态、长序列任务的基准。
- **基准（Benchmark）**：上述三个数据集本身即作为评估标准，主要测量智能体在新任务上的**适应速度**（学习曲线）和**任务成功率**。
- **对比方法**：论文本身未在摘要中明确列出对比方法。根据上下文，推测对比了“不加载技能库的原始智能体”或“自进化范式下的单一智能体”（即基线为GLM-4.6自身无技能库的表现）。具体对比方法需查看全文，摘要未详述。

### 4. 资源与算力

- **明确说明**：论文摘要及元数据中**未提及**具体使用的GPU型号、数量、训练时长等算力信息。
- **合理推测**：由于使用GLM-4.6作为种子智能体，模型本身较大，但技能库的构建主要依赖预训练模型进行推理（蒸馏、精炼、扩展），可能不需要大规模并行计算；实际算力需求取决于轨迹数据量和迭代次数，但文中未提供。

### 5. 实验数量与充分性

- **实验数量**：摘要仅提及在三个基准上进行了实验，并“做了广泛实验（extensive experiments）”。未列出具体实验组数、消融实验等细节。
- **充分性分析**：
  - **积极面**：覆盖了三个不同领域的长时程交互任务，具有一定的多样性。
  - **不足**：缺少与其他技能构建方法（如手动创建、强化学习等）的直接对比；消融实验（如去掉精炼/扩展模块的效果）未提及；未说明是否重复多次实验以评估方差；未在更多类型的智能体（如GPT-4、LLaMA）上验证可迁移性。因此，实验的充分性、客观性和公平性在摘要层面难以完全评估，需查看全文。

### 6. 论文的主要结论与发现

- **结论**：AutoSkills构建的技能库能显著提升LLM智能体在新任务上的适应速度和成功率，打破了单智能体自进化的局限性，为构建可迁移的智能体技能体系提供了新范式。
- **发现**：经验转移（experience transfer）对模型性能有重要影响，且多层级技能设计比单一层级更有效（通过战略-功能-原子三层分离，提高了复用性和泛化能力）。

### 7. 优点：方法或实验设计上的亮点

- **自动化程度高**：无需人工标注或手动设计技能，完全由LLM驱动自动蒸馏、精炼和扩展，降低了构建成本。
- **层次化结构**：三层技能体系兼顾了抽象策略和具体操作，既支持宏观规划，也支持细粒度复用。
- **主动扩展机制**：探索性技能扩展使技能库能超越初始训练数据，提升了对未知任务的覆盖能力。
- **即插即用**：构建的技能库可被不同智能体直接加载，避免了每个智能体的重复学习。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制

- **实验覆盖不足**：仅使用一个种子智能体（GLM-4.6），未验证在其他基础模型（如GPT-4、Claude）上的适用性。
- **偏差风险**：技能蒸馏和精炼依赖目标LLM本身的能力，可能存在“自我强化”偏差（模型难以纠正自身系统性错误）。
- **应用限制**：
  - 构建技能库需要大量原始轨迹数据，数据获取成本可能较高。
  - 精炼和扩展过程可能引入冗余或冲突技能，缺乏自动去重机制。
  - 当前仅测试了长时程交互任务，未涉及实时、动态环境或需要物理交互的场景。
- **算力与资源未报告**，难以评估工程可复现性。

（完）
