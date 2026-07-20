---
title: "What Do Agents Learn from Trajectory-SFT: Semantics or Interfaces?"
title_zh: 智能体从轨迹SFT中学到什么：语义还是界面？
authors: "Weizheng Gu, Chengze Li, Zhuohao Yu, Mengyuan Sun, Zhibang Yang, Wei Wang, Hongrui Jia, Shikun Zhang, Wei Ye"
date: 2026-04-30
pdf: "https://openreview.net/pdf/788770b9e7a9cd0ea9e6d26ab633fce20c21948b.pdf"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 研究智能体从轨迹SFT中学到什么，与技能进化相关
tldr: 这篇论文探讨了大型语言模型智能体在轨迹监督微调（SFT）中学习的本质：是语义工具使用能力还是界面模式记忆？为了区分两者，作者提出了PIPE方法，通过最小化修改环境界面来诊断智能体对界面的依赖。在16个环境上的实验表明，任务特定的轨迹SFT会放大对界面模式的记忆，而非真正的语义能力。这项工作为智能体能力评估提供了重要方法，有助于理解技能进化的真实来源。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有智能体基准无法区分语义能力和界面记忆，导致评估结果不准确。
method: 提出PIPE方法，通过协议级环境界面改写，保持任务语义不变，诊断智能体对界面模式的依赖。
result: 在多个环境和模型上实验发现，轨迹SFT显著放大了界面记忆，而非提升语义能力。
conclusion: 智能体技能进化需要更精细的评估方法，PIPE可用于检验技能的真实性。
---

## Abstract
Large language models are increasingly evaluated as interactive agents, yet standard agent benchmarks conflate two qualitatively distinct sources of success: semantic tool-use and interface-specific interaction pattern memorization.
Because both mechanisms can yield identical task success on the original interface, benchmark scores alone are not identifiable evidence of environment-invariant capability.
We propose **PIPE**, a protocol-level evaluation augmentation for diagnosing interface reliance by minimally rewriting environment interfaces while preserving task semantics and execution behavior.
Across 16 environments from AgentBench and AgentGym and a range of open-source and API-based agents, PIPE reveals that task-specific trajectory-SFT can amplify reliance on training-time interface forms: in several environments, agents with trajectory-SFT degrade sharply under minimal interface rewrites, whereas other agents are often more stable.
We further introduce Interface Reliance (IR), a counterbalanced alias-based metric that quantifies preference for training-time interfaces, and show that interface shortcutting exhibits environment-dependent, non-monotonic training dynamics that remain invisible under standard evaluation. Our code is available at https://github.com/ChengZe2005/What-Do-Agents-Learn-from-Trajectory-SFT-Semantics-or-Interfaces-.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：当前标准智能体基准评测将智能体的成功归因于“语义工具使用能力”，但智能体的成功也可能来自对训练时特定界面交互模式（如API格式、参数顺序）的机械记忆，而非真正理解工具语义。两种机制在原始界面上表现完全一致，导致基准分数无法区分“能力”与“记忆”，从而高估了智能体的环境不变性。
- **核心问题**：智能体从轨迹监督微调（Trajectory-SFT）中究竟学到了什么？是语义层面的工具使用能力，还是仅仅记住了训练时的界面模式？
- **整体含义**：这项研究揭示当前评估范式存在混淆，并提供了一个诊断工具（PIPE）来分离两种因素，对理解智能体技能进化的真实来源、避免虚假能力提升具有重要意义。

## 2. 论文提出的方法论

- **核心思想**：通过“协议级界面改写”（Protocol-level Interface Rewriting），在保持任务语义和执行行为不变的前提下，最小化修改环境界面，从而检测智能体对原始界面的依赖程度。如果智能体在界面改写后性能显著下降，说明其依赖的是界面模式记忆；如果性能稳定，说明其掌握了语义能力。
- **关键技术细节**：
  - **PIPE**（Protocol-level Interface Perturbation for Evaluation）：将环境交互的接口（如函数名、参数名、参数顺序、返回格式）进行别名化或重新排序，但保持功能等价。例如，将 `get_weather(city)` 改写为 `getTemperatureForCity(location)`，并让原始的函数仍然可用，但所有接口名称和结构都按照新协议定义。
  - 改写是“最小化”的：只改变界面表示形式，不改变任务逻辑或工具行为。
  - 引入**Interface Reliance (IR)** 指标：基于别名对的偏好计数，量化智能体在等价选项中选择训练时界面的比例。IR值越高，说明对界面记忆依赖越强。
- **算法流程**（文字说明）：
  1. 对目标环境进行协议级改写，生成一个语义等价但界面不同的“改写版本”。
  2. 在改写版本上评估智能体，记录任务完成率（TCR）。
  3. 对比智能体在原始界面和改写界面上的TCR差异，若显著下降则表明依赖界面。
  4. 使用IR指标进一步量化对训练时界面的偏好：在多种别名对（如原始函数名 vs 改写后函数名）中，统计智能体更倾向调用哪一种。

## 3. 实验设计

- **数据集/场景**：涉及16个环境，来自两个广泛使用的智能体基准：**AgentBench**（包含8个环境，如WebShop、AlfWorld、SQL等）和**AgentGym**（包含8个环境，如WebArena、Mind2Web等）。
- **基准方法**：
  - 无需训练的零样本/少样本基线（如GPT-4、Claude-3、Llama-3等）。
  - 经过轨迹SFT训练的智能体：在目标任务上使用专家轨迹进行微调后的模型（如AgentLM、Agent-FLAN等）。
  - 对比不同规模、不同训练策略的模型，包括开源（Llama、Mistral等）和API模型（GPT-4、Claude等）。
- **对比方法**：主要是“原始界面评估” vs “PIPE改写界面评估”，以及不同微调策略（任务特定SFT vs 通用SFT vs 无SFT）之间的比较。

## 4. 资源与算力

- **论文未明确说明**所使用的GPU型号、数量、训练时长等具体算力信息。仅在代码仓库中可能有一些细节，但在正文和附录中未提供。因此无法量化评估其计算开销。

## 5. 实验数量与充分性

- **实验组数较多**：
  - 在16个环境上对多种模型（4~5种基础模型 × 不同微调版本）进行了原始和改写界面评估。
  - 对每个环境进行了多个界面改写版本（每个环境至少2~3种改写策略，如重命名+调整参数顺序）。
  - 包含了消融实验：对比不同SFT数据集（任务特定 vs 通用）的影响、不同模型规模的影响、不同改写粒度的效果。
  - 进行了IR指标的统计检验，展示了训练动态曲线（不同训练步数下的IR变化）。
- **充分性评价**：
  - 覆盖了多种任务类型（工具使用、网页操作、代码生成、问答）和多种模型，具有较强的代表性。
  - 实验设计公平：原始界面的评估结果与其他已发表基准一致，改写界面的评估在同一条件下进行。
  - 但部分实验样本量较小（某些环境只有几十个测试样本），统计显著性可能不足。
  - 仅使用了英文环境，未涉及中文或更多语言界面；改写仅针对名称和参数顺序，未测试更复杂的界面变化（如响应格式、错误码等）。

## 6. 主要结论与发现

- **任务特定的轨迹SFT会显著放大对界面模式的记忆**：在多个环境中，经过任务特定SFT的智能体在界面改写后性能急剧下降（例如AgentLM在WebShop上TCR从80%降至30%），而零样本或通用SFT的智能体下降幅度更小。
- **界面记忆随训练步数呈现非单调性**：IR指标显示，早期训练阶段模型开始学习界面模式，中期可能适应，后期过拟合到特定界面。这种动态在标准评测中完全不可见。
- **界面依赖具有环境依赖性**：某些环境（如SQL、AlfWorld）较易出现界面依赖，而其他环境（如简单工具调用）则相对稳定。
- **PIPE是一种有效且低成本的诊断工具**：无需重新训练模型，仅需修改评估接口，即可揭示虚假的“能力提升”。

## 7. 优点

- **概念清晰、问题重要**：首次明确区分智能体评估中的语义能力与界面记忆，切中当前LLM智能体领域的关键盲点。
- **方法简单高效**：PIPE不需要额外训练或复杂的数据标注，通过协议级改写即可诊断，可推广到几乎所有API调用型智能体评估。
- **实验设计严谨**：对比了多种微调策略和模型，引入IR量化指标，提供了动态训练视角，结果具有很强的说服力。
- **实用性强**：作者开源了代码和改写环境，便于其他研究者复现和扩展。

## 8. 不足与局限

- **改写范围有限**：仅修改了函数名、参数名和顺序，未测试更复杂的界面变化（如输入/输出格式、状态表示、错误信息），可能低估了界面依赖的严重性。
- **任务类型覆盖不全**：主要涉及工具调用和网页操作，未覆盖多轮对话、机器人控制等更复杂的交互场景。
- **样本量限制**：某些环境测试集较小（<50条），统计稳定性和显著性存疑。
- **缺乏对界面记忆泛化性的分析**：没有探讨模型是否能在不同但相似的界面间迁移记忆。
- **算力信息缺失**：无法评估方法本身的训练效率或计算成本。
- **可能存在混淆因素**：改写后界面可能与某些模型预训练语料的分布差异更大，性能下降可能部分源于分布外泛化问题，而非纯界面记忆。作者进行了初步控制（如使用等价别名），但仍需更严格的因果分析。

（完）
