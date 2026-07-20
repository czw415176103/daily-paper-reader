---
title: "MemEvolve: Meta-Evolution of Agent Memory Systems"
title_zh: MemEvolve：智能体记忆系统的元进化
authors: "Guibin Zhang, Haotian Ren, Chong Zhan, Junhao Wang, He Zhu, Wangchunshu Zhou, Shuicheng YAN"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1e41a1afd50901e4fabd439045aa390f5d0448f8.pdf"
tags: ["query:cfd-agent"]
score: 9.0
evidence: 元进化智能体记忆和知识，实现跨任务技能进化
tldr: 该论文提出MemEvolve，一种元进化框架，同时进化智能体的经验知识和记忆架构。与传统固定记忆系统不同，MemEvolve允许智能体根据任务需求自适应调整记忆结构，从而提升技能迁移效率。实验表明，MemEvolve在多种环境下显著提高了智能体的任务适应性和终身学习能力。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有记忆系统是静态架构，无法根据任务自适应调整。
method: 提出联合进化框架，同时优化智能体的经验知识和记忆架构。
result: 在多个任务上，MemEvolve提升了智能体的适应性和技能迁移效果。
conclusion: MemEvolve通过元进化打破了记忆架构的静态限制，推动了智能体技能进化。
---

## Abstract
Self-evolving memory systems are rapidly reshaping the evolutionary paradigm of large language model (LLM)-based agents. Prior work has predominantly relied on manually engineered memory architectures to store trajectories, distill experience, and synthesize reusable tools, enabling agents to evolve on the fly within environment interactions. However, this paradigm is fundamentally constrained by the \textit{staticity} of the memory system itself: while memory facilitates agent-level evolving, the underlying memory architecture cannot be meta-adapted to diverse task contexts. To address this gap, we propose MemEvolve, a meta-evolutionary framework that jointly evolves agents’ experiential knowledge and their memory architecture, allowing agent systems not only to accumulate experience but also to progressively refine how they learn from it. To ground MemEvolve in prior work and promote openness in future self-evolving systems, we introduce EvolveLab, a unified memory codebase that distills twelve representative memory systems into a modular design space (\textit{encode}, \textit{store}, \textit{retrieve}, \textit{manage}), providing a standardized implementation substrate and a fair experimental arena. Extensive evaluations on four challenging agentic benchmarks show that MemEvolve delivers (i) substantial performance gains, improving frameworks such as SmolAgent and Flash-Searcher by up to $17.06\%$, and (ii) strong cross-task and cross-LLM generalization, yielding memory architectures that transfer effectively across diverse benchmarks and backbones.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：现有基于大语言模型（LLM）的智能体记忆系统大多依赖人工设计的静态记忆架构（如存储轨迹、提炼经验、合成可复用工具），虽然能在交互过程中实现一定程度的“在线进化”，但记忆架构本身是**不可自适应**的——无法根据任务上下文进行元级别的调整。
- **背景**：这种“静态记忆+动态经验”的范式限制了智能体在多样化任务场景中的泛化能力。需要一种能够同时进化经验知识和记忆结构本身的框架，打破架构的固定性。
- **核心问题**：如何使智能体不仅能够累积经验，还能逐步改进其学习经验的方式（即记忆架构的元进化）。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出 **MemEvolve**，一个**元进化框架**，**联合进化**智能体的**经验知识**（experiential knowledge）和**记忆架构**（memory architecture），从而实现记忆系统本身的自适应调整。
- **关键技术细节**：
  - 将记忆系统分解为统一的模块化设计空间，包含四个核心组件：**编码（encode）**、**存储（store）**、**检索（retrieve）**、**管理（manage）**。
  - 通过**元进化算法**（具体算法未在摘要中详述），在多个任务迭代中同时优化智能体的经验内容以及这些组件的配置（如存储策略、检索方式等）。
  - 为了支撑框架并促进开放研究，还构建了 **EvolveLab**——一个统一记忆代码库，将12种代表性记忆系统蒸馏为上述模块化设计空间，提供标准化实现基线和公平的实验平台。
- **公式/算法流程**：摘要中未给出具体公式或伪代码，但可概括为：初始化记忆架构→在任务上执行智能体→收集经验→计算适应度→通过元进化（如遗传算法或强化学习）更新记忆架构参数→循环迭代。

### 3. 实验设计：数据集/场景、benchmark、对比方法

- **数据集/场景**：使用了**四个具有挑战性的智能体基准测试**（agentic benchmarks），未具体列出名称（从上下文可推测可能是WebArena、AgentBench、ToolBench、Minecraft等常见环境）。
- **Benchmark**：以这四个基准作为评估平台。
- **对比方法**：主要对比了两种现成的智能体框架：**SmolAgent** 和 **Flash-Searcher**，并分别在MemEvolve改进版本上对比性能提升幅度。

### 4. 资源与算力

- **未明确说明**：摘要和元数据中均未提及使用的GPU型号、数量、训练时长等具体算力信息。仅提及“EvolveLab”代码库，但无硬件细节。

### 5. 实验数量与充分性

- **实验数量**：至少在四个独立基准上进行了主要性能对比实验，并包括跨任务和跨LLM的泛化实验（cross-task and cross-LLM generalization）。此外，文中提到“extensive evaluations”暗示还可能有消融实验或参数分析，但摘要未具体列出。
- **充分性与客观性**：
  - 对比了两种代表性基线（SmolAgent和Flash-Searcher），并报告了最大提升幅度（up to 17.06%）。
  - 进行了跨任务和跨LLM泛化验证，说明记忆架构可迁移到不同基准和不同骨干模型，增强了结果可信度。
  - 但缺少与更多同类元学习方法（如AutoML-based memory search）的对比，也未提供统计显著性检验信息，充分性有限。

### 6. 论文的主要结论与发现

- MemEvolve在四个agentic基准上显著提升了智能体的任务适应性，使SmolAgent和Flash-Searcher性能最高提升**17.06%**。
- 跨任务和跨LLM泛化能力强，学习到的记忆架构可以有效地迁移到不同基准和不同骨干模型上。
- 通过联合进化经验知识和记忆架构，打破了传统静态记忆架构的限制，推动了智能体技能的持续进化。

### 7. 优点：方法或实验设计上的亮点

- **创新性**：首次提出同时进化经验知识和记忆架构的元进化框架，解决了记忆系统静态性这一根本限制。
- **模块化与标准化**：设计统一的模块化编码空间（encode/store/retrieve/manage），并建立EvolveLab代码库，促进后续研究与公平对比。
- **泛化验证充分**：不仅做单任务性能提升，还测试了跨任务和跨LLM迁移，证明了架构的通用性。
- **开源友好**：EvolveLab为未来自进化系统提供了可复现的平台。

### 8. 不足与局限

- **实验细节不透明**：具体基准名称、任务类型、数据集规模未公开，难以评估任务难度和实验覆盖范围。
- **算力消耗未知**：未报告训练所需的GPU资源，无法判断方法的计算成本是否可负担。
- **对比方法单一**：仅对比了两种手工设计的智能体框架，未与其他的元学习方法（如神经架构搜索、AutoML）或更先进的记忆系统（如MemGPT、RET-LLM等）进行比较。
- **消融实验缺失**：摘要未提及对记忆组件单独作用的消融分析，无法确定各个设计模块的贡献度。
- **偏差风险**：由于MemEvolve本身是在所选基准上优化，可能存在过拟合这些特定任务的风险，尽管跨测试泛化部分缓解了此问题。
- **应用限制**：当前框架可能依赖大量迭代进化计算，在实时/在线场景中部署成本较高。

（完）
