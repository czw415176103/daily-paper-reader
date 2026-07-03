---
title: Automated Stateful Specialization for Adaptive Agent Systems
title_zh: 自适应代理系统的自动化有状态特化
authors: "Myan Vu, Harrish Ayyanar, PANG JIANG, Anwiketh Reddy, Mayank Goel"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=UESTP6dR1K"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 自主发现专家原型并培养专业知识
tldr: 该论文提出ASpec框架，通过进化搜索自主发现专家代理原型，并通过经验培养其专业知识，实现了无需人工干预的自适应代理系统，对代理技能进化研究有重要意义。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有代理设计缺乏自适应性和深度任务专业知识积累。
method: 提出ASpec框架，通过进化搜索发现专家原型并基于经验培养。
result: 在多种任务上展示了自适应能力和专业知识积累。
conclusion: 为创建可重新配置的专业代理团队提供了新方向。
---

## Abstract
Current automated agent design frameworks produce either static workflows that lack adaptability or per-query optimizers that prevent the accumulation of deep, agent-level task expertise. We propose a new direction that reconciles these paradigms: creating stateful teams of specialist agents that accumulate knowledge over time and can be reconfigured for novel tasks entirely without human intervention. To this end, we introduce \textsc{ASpec}, a framework that manages this full agent lifecycle by first autonomously \textbf{discovering} specialist archetypes via evolutionary search and then \textbf{cultivating} their expertise through experience, mirroring how human experts learn through practice and reflection. We further introduce a lightweight hierarchical control policy, "retain-then-escalate," which governs when to leverage the established agent system versus when to adapt its structure. Through comprehensive experiments, we demonstrate that this approach leads to significant performance gains on expert-level scientific benchmarks like GPQA while matching the state-of-the-art on broader domain tasks, demonstrating a promising path toward agent systems that are simultaneously expert, adaptive, and efficient. We will release the code at https://github.com/myanvoos/ASpec.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前自动化代理设计框架存在两大缺陷：静态工作流缺乏适应性，以及逐查询优化器阻碍了深度、代理级别的任务专业知识的积累。
- **研究动机**：期望创建一种既能自适应又能积累专业知识、且无需人工干预即可重新配置以适应新任务的有状态专家团队。
- **整体含义**：该工作试图调和上述两种范式的矛盾，迈向同时具备**专家性**、**适应性**和**效率**的代理系统。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出 **ASpec** 框架，管理代理的完整生命周期，通过两个阶段实现：
  - **发现（Discover）**：利用进化搜索（evolutionary search）自主发现专家原型（specialist archetypes）。
  - **培养（Cultivate）**：通过经验（实践与反思）培养这些原型的专业知识，模拟人类专家学习过程。
- **关键技术细节**：
  - 引入轻量级分层控制策略 **“retain-then-escalate”**：决定何时使用现有代理系统，何时需要调整系统结构。
  - 算法流程（文字描述）：
    1. 初始化代理种群；
    2. 进化搜索迭代：评估代理性能，选择、交叉、变异生成新原型；
    3. 固定专家原型后，通过在实际任务中积累经验（如缓存、参数更新等）培养其领域知识；
    4. 运行时，控制策略首先尝试使用现有专家团队；若失败则触发结构调整（如重新分配角色、创建新专家）。
- **未提供具体公式**：摘要中未包含数学公式。

## 3. 实验设计：使用了哪些数据集/场景、benchmark、对比方法

- **数据集/场景**：提到了 **GPQA**（专家级科学基准）以及更广泛的领域任务（具体任务名称未列出）。
- **Benchmark**：GPQA 作为专家级科学基准。
- **对比方法**：与当前最优方法（state-of-the-art）进行比较。但摘要中未提及具体对比方法名称（如 GPT-4、AutoGPT 等）。
- **注意**：论文摘要未详细说明实验设置，仅有概括性描述。

## 4. 资源与算力

- **资源与算力**：论文正文中**未提及**使用的 GPU 型号、数量、训练时长等任何算力信息。需要进一步阅读原文才能获得。

## 5. 实验数量与充分性

- **实验数量**：仅说明进行了“全面实验”（comprehensive experiments），但未给出具体实验数量或消融实验的组数。
- **充分性评估**：从摘要看，实验覆盖了专家级基准和广泛领域任务，但缺少：
  - 明确的消融实验（如对发现、培养、控制策略各模块的贡献分析）；
  - 不同任务类型（如对话、代码、推理等）的详细结果；
  - 与多种基线方法的系统对比表格。
  - 结论：实验**不够充分公开**，客观性和公平性尚无法全面评判，需要参考完整论文。

## 6. 论文的主要结论与发现

- **主要结论**：
  1. ASpec 在专家级科学基准 GPQA 上取得显著性能提升；
  2. 在更广泛的领域任务上达到与当前最优方法相当或更优的结果；
  3. 验证了该框架能同时实现**专家性**、**适应性**和**效率**三个目标。
- **发现**：自主发现专家原型并经验培养的策略有效克服了静态工作流和逐查询优化的不足。

## 7. 优点：方法或实验设计上的亮点

- **方法优点**：
  - 自动化：无需人类干预即可发现和培养专家原型；
  - 有状态：代理积累专业知识，而非每次从头计算；
  - 分层控制：“retain-then-escalate”策略平衡了稳定性和灵活性；
  - 生物启发：模仿人类实践-反思的学习方式。
- **实验亮点**：
  - 在专家级高难度基准 GPQA 上验证了方法有效性；
  - 与多种领域任务的结果对比显示了方法的泛化能力。

## 8. 不足与局限

- **实验局限性**：
  - 未提供详细的实验配置、数据集列表、对比算法和指标数值，无法独立复现或验证；
  - 缺乏消融实验、灵敏度分析等来支撑各模块的贡献；
  - 仅在有限的任务类型（科学基准+广泛领域任务）上测试，对更复杂或具对抗性的场景未提及。
- **偏差风险**：
  - 未讨论进化搜索的计算成本及随专家数量增长的可扩展性；
  - 未说明“retain-then-escalate”策略的阈值如何设定及其鲁棒性。
- **应用限制**：
  - 需要持续的在线经验积累，可能不适合需要即时响应且训练周期极短的应用；
  - 进化搜索可能产生冗余或低效的专家原型，缺乏对探索-利用平衡的深入分析。

（完）
