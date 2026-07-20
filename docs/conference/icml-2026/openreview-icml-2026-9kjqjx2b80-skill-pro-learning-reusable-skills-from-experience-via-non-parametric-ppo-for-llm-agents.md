---
title: "Skill-Pro: Learning Reusable Skills from Experience via Non-Parametric PPO for LLM Agents"
title_zh: Skill-Pro：通过非参数PPO从经验中学习可重用技能以用于LLM代理
authors: "Qirui Mi, Zhijian Ma, Mengyue Yang, Haoxuan Li, Yisen Wang, Haifeng Zhang, Jun Wang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/1bccdd13cafb0f585fe10ee4bc4fb18649e4c959.pdf"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 通过非参数PPO为LLM代理学习可重用技能
tldr: 针对LLM驱动代理在重复场景中重复推理导致的计算冗余和不稳定性，提出Skill-Pro框架。该框架形式化Skill-MDP，将被动的情景叙述转化为可执行的技能，并引入非参数PPO，利用语义梯度实现技能的高效重用而不降低能力。实验表明，Skill-Pro使代理能够自主学习和复用技能，显著提升决策效率。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: LLM代理在重复场景中因缺乏经验重用导致计算冗余和不稳定。
method: 提出Skill-Pro框架，形式化Skill-MDP，结合非参数PPO实现技能自主学习和可靠重用。
result: 代理能够有效学习可重用技能，提升决策效率和稳定性。
conclusion: Skill-Pro为LLM代理的技能学习提供了有效途径，减少计算冗余。
---

## Abstract
LLM-driven agents excel at sequential decision-making but often rely on on-the-fly reasoning, re-deriving solutions even in recurring scenarios. This insufficient experience reuse leads to computational redundancy and instability. To bridge this gap, we propose **Skill-Pro**, a framework enabling agents to autonomously learn reusable procedural skills from interaction experiences without parameter updates. By formalizing a **Skill-MDP**, Skill-Pro transforms passive episodic narratives into executable Skills defined by activation, execution, and termination conditions to ensure executability. 
To achieve reliable reusability without capability degradation, we introduce **Non-Parametric PPO**, which leverages semantic gradients for high-quality candidate generation and a PPO Gate for robust Skill verification. Through score-based maintenance, Skill-Pro sustains compact, high-quality procedural memory.
Experimental results across in-domain, cross-task, and cross-agent scenarios demonstrate that Skill-Pro achieves superior reuse rates and significant gains with extreme memory compression. Visualized evolutionary trajectories and Skill distributions further reveal how Skill-Pro transparently accumulates, refines, and reuses procedural knowledge to facilitate long-term autonomy.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：大型语言模型（LLM）驱动的智能代理（Agent）在顺序决策任务中，通常依赖实时推理（on-the-fly reasoning），即使面对重复出现的场景也会重新推导解决方案，导致大量计算冗余和决策不稳定。这种缺乏经验重用的机制限制了代理的长期自主性和效率。
- **研究动机**：现有方法要么通过微调更新模型参数（成本高、可能遗忘旧能力），要么依赖显式记忆检索（但难以抽象出可重用的程序化技能）。因此，需要一种能够自动从交互经验中学习可复用技能、且不降低原有能力的方法。
- **整体含义**：论文提出 **Skill-Pro** 框架，使 LLM 代理能够自主地从被动的情景叙述（episodic narratives）中提取并保存可执行的技能，从而在类似场景中直接调用，显著提升决策效率并减少计算开销。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将强化学习中的技能学习引入 LLM 代理，形式化 **Skill-MDP**（技能马尔可夫决策过程），将原始 MDP 转化为以技能为动作的更高层次决策过程。技能被定义为由**激活条件**（activation）、**执行逻辑**（execution）和**终止条件**（termination）三元组组成的可执行程序。
- **关键技术细节**：
  - **Skill-MDP 构建**：将交互经验（如对话历史、行动序列）自动转化为技能候选。每个技能通过自然语言描述的条件和步骤封装。
  - **非参数 PPO（Non-Parametric PPO）**：这是核心创新。传统 PPO 需要参数化策略网络，但这里利用**语义梯度**（semantic gradients）生成高质量技能候选，再通过一个 **PPO Gate**（基于策略梯度思想的验证器）对候选技能进行鲁棒验证和筛选，确保技能可靠且不会导致性能下降。
  - **基于分数的维护机制**（Score-based maintenance）：技能池通过分数评估保持紧凑性和高质量，剔除低效或冗余技能，实现极端记忆压缩。
- **算法流程（文字说明）**：
  1. Agent 在环境中交互，收集经验。
  2. 构建 Skill-MDP，从经验中提取技能候选。
  3. 利用非参数 PPO 对候选技能进行迭代优化：语义梯度指导候选生成，PPO Gate 计算收益并筛选。
  4. 将验证通过的高分技能存入技能库。
  5. 在后续决策中，根据当前状态匹配激活条件，直接调用技能执行，避免重新推理。

### 3. 实验设计：数据集、场景与对比方法

- **实验场景**：覆盖三种分布外/泛化场景：
  - **域内场景（in-domain）**：与训练环境相同分布的任务。
  - **跨任务场景（cross-task）**：不同但相关的任务。
  - **跨代理场景（cross-agent）**：不同的代理架构或语言模型。
- **基准（Benchmark）**：未在原文中明确说明具体任务名称，可能使用了常见的 LLM 代理基准（如 ALFWorld、MiniWoB、WebShop 等）。但从“域内/跨任务/跨代理”设计看，实验环境应包含多种交互式任务。
- **对比方法**：结合“累积经验重用”和“技能学习”的典型基线，推测包括：
  - 无经验重用的标准 LLM Agent（如 ReAct 或 Chain-of-Thought 直接推理）。
  - 基于记忆检索的方法（如 Reflexion、MemoGPT）。
  - 基于参数微调的方法（如 Fine-tuning with RL）。
  - 可能还包括其他技能学习框架（如 SayCan、TaskLAMA）。

### 4. 资源与算力

- 论文摘要和元数据**未明确说明**使用的 GPU 型号、数量和训练时长。考虑到 Skill-Pro 是一种轻量级非参数方法（无需更新模型权重），其计算开销主要在于技能候选生成和 PPO Gate 验证，可能对算力要求较低。但具体细节（如每个实验的 GPU 资源）在提供的文本中缺失。

### 5. 实验数量与充分性

- **实验数量**：至少在三类场景（in-domain、cross-task、cross-agent）下进行了评估，还包含可视化技能轨迹和技能分布分析。此外，可能有消融实验验证技能维护分数机制、PPO Gate 的效果。但具体实验组数（如不同任务数量、种子次数）未提供。
- **充分性**：从摘要描述“Superior reuse rates and significant gains with extreme memory compression”可见，实验覆盖了泛化性和效率两个维度，且通过可视化分析了技能演化过程，比较全面。但对比方法数量和统计显著性检验细节未知，需要核查全文才能判断绝对公平性。

### 6. 论文的主要结论与发现

- **主要结论**：Skill-Pro 框架使 LLM 代理能够自主学习和重用可执行的程序化技能，在域内、跨任务、跨代理场景下均取得了更高的技能复用率，同时实现了极端的内存压缩，大幅提升决策效率和稳定性。
- **发现**：通过非参数 PPO 验证的技能质量较高，且技能库的分数维护机制能持续保持紧凑性；可视化的技能演化轨迹表明技能积累过程透明高效，有助于长期自主性。

### 7. 优点

- **非参数化**：无需更新 LLM 参数，避免了灾难性遗忘和昂贵的微调成本。
- **可解释性**：技能以自然语言三元组（激活/执行/终止）存储，易于理解和调试。
- **高效性**：语义梯度 + PPO Gate 在极少量候选情况下即可筛选出高质量技能，配合分数维护机制实现了极端记忆压缩。
- **泛化性**：在跨任务、跨代理场景中仍保持良好表现，证明技能具有一定抽象性。
- **自动化**：完全自主从被动经验中学习技能，无需人工标注。

### 8. 不足与局限

- **实验覆盖有限**：提供的文本中未列出具体任务数据集和对比方法的详细结果，难以判断是否涵盖了足够多的基线和任务类型（如是否包含多轮对话、具身任务等）。
- **偏差风险**：非参数 PPO 中的“语义梯度”定义和 PPO Gate 的具体实现可能存在主观设计偏差，不同 LLM 后端（如 GPT-4 vs LLAMA）效果可能差异较大。
- **应用限制**：技能的可迁移性依赖于 LLM 的表达能力和推理能力，对于低性能模型可能难以生成高质量技能。另外，技能激活条件完全由语言描述决定，可能导致歧义或匹配失败。
- **算力信息缺失**：未报告训练/推理时间和GPU消耗，无法评估实际部署成本。
- **稳定性验证**：虽然提到了“稳定”，但未给出多次运行的标准差或置信区间。

（完）
