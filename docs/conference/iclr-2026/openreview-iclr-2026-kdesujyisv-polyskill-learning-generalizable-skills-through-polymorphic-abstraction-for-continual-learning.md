---
title: "PolySkill: Learning Generalizable Skills Through Polymorphic Abstraction For Continual Learning"
title_zh: PolySkill：通过多态抽象学习可泛化技能以实现持续学习
authors: "Simon Yu, Gang Li, Weiyan Shi, Peng Qi"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=KdEsujyiSV"
tags: ["query:cfd-agent"]
score: 7.0
evidence: 通过多态抽象学习可泛化和组合的技能，提升智能体技能泛化能力
tldr: 现有智能体技能学习方法容易过度专化于单一网站。本文提出PolySkill，灵感来自软件工程多态性，将技能的抽象目标与具体实现解耦。智能体学会可泛化、可组合的技能，在跨网站场景中技能复用率显著提升。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有技能学习方法创建的技能过度专化，无法泛化到新环境。
method: 借鉴多态抽象，分离技能的抽象目标（做什么）和具体实现（怎么做）。
result: 实验表明技能复用率提升，泛化能力增强。
conclusion: 多态抽象是提升智能体技能泛化性的有效手段。
---

## Abstract
Large language models (LLMs) are moving beyond static uses and are now powering agents that learn during their interaction with external environments. For example, agents can learn reusable skills while navigating web pages or toggling new tools. However, existing methods for skill learning often create skills that are over-specialized to a single website and fail to generalize.
We introduce PolySkill, a new framework that enables agents to learn generalizable and compositional skills. The core idea, inspired by polymorphism in software engineering, is to decouple a skill's abstract goal (*what* it accomplishes) and its concrete implementation (*how* it is executed). Experiments show that
our method (1) improves skill reuse by 1.7x on seen websites and (2) boosts success rates by up to 9.4\% on Mind2Web and 13.9\% on unseen websites, while reducing steps by over 20\%. (3) In self-exploration settings without specified tasks, our framework improves the quality of proposed tasks and enables agents to learn generalizable skills that work across different sites. 
By enabling the agent to identify and refine its own goals, the PolySkill enhance the agent a better curriculum, leading to the acquisition of more generalizable skills compared to baseline methods. Our findings show that separating a skill's goal from its execution is a crucial step toward developing autonomous agents that can learn and generalize across the open web continuously. Our code can be found in \href{https://github.com/simonucl/PolySkill}{\texttt{https://github.com/simonucl/PolySkill}}.

---

## 论文详细总结（自动生成）

# 论文中文总结：PolySkill

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：现有的大语言模型（LLM）驱动的智能体在与环境交互学习技能时，学到的技能往往**过度专化**于单一网站或场景，缺乏**跨环境泛化**能力。例如，在某个网站学会的“登录”技能无法直接复用到其他网站的登录流程。
- **研究动机**：为使智能体能够在开放网络（open web）中持续学习并复用技能，需要一种机制让技能的核心目标（做什么）与具体执行方式（怎么做）分离，从而提升技能的**可迁移性**和**组合性**。
- **整体含义**：PolySkill 借鉴软件工程中的**多态（polymorphism）**思想，将技能的抽象目标与具体实现解耦，是迈向**自主、持续学习**智能体的关键步骤。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：多态抽象 —— 将一项技能的**抽象目标（what）** 与**具体实现（how）** 显式分离。类似于面向对象编程中的接口与实现分离，智能体只需记住“做什么”而不必绑定“怎么做”，在不同环境中可调用不同的实现。
- **关键技术细节**（基于摘要推断）：
  - 智能体在交互过程中，首先识别任务的抽象目标（如“填写表单”），然后生成或复用该目标对应的具体执行步骤（如“在 site A 填写表单的具体 DOM 操作”）。
  - 通过**技能库**管理已学技能，每个技能包含：目标描述（抽象接口）+ 一组环境特定的实现（多态变体）。
  - 在**自探索（self-exploration）** 模式下，智能体主动提出任务目标并验证其泛化性，从而改进学习课程（curriculum），生成更通用的技能。
- **公式/算法流程**（文中未给出完整伪代码，根据摘要概括）：
  1. 输入：当前环境状态（如网页 DOM）+ 历史技能库。
  2. 对给定任务，智能体判断是否已有匹配的抽象目标；若有，则调用适配当前环境的实现；若无，则通过 LLM 推理生成新的抽象目标并探索实现方式。
  3. 执行后，将新学到的实现与已有抽象目标关联，或创建新的抽象目标。
  4. 定期执行**泛化评估**：将技能尝试应用于不同环境，若失败则调整实现或细化抽象。

## 3. 实验设计

- **数据集/场景**：
  - **Mind2Web**：包含多个真实网站的网页操作任务，用于评估跨网站泛化能力。
  - **自建未见网站（unseen websites）**：测试智能体在训练时未接触过的新网站上的表现。
  - **自探索（self-exploration）** 设置：无指定任务，智能体自主提出目标并学习。
- **基准方法（benchmark）**：与现有技能学习方法对比（摘要中未列出具体基线名称，推测包括基于 prompt 的零样本、微调模型、或传统技能学习框架如 SayCan、ReAct 变体等）。
- **对比方法**：未明确列出，但文中提到“baseline methods”，通常指不采用多态抽象的端到端技能学习方法。

## 4. 资源与算力

- 论文中**未明确说明**使用的 GPU 型号、数量、训练时长等具体算力信息。仅提供了开源代码仓库链接，但未在摘要中提及硬件配置。因此，无法给出具体资源消耗数据。（注意：此点需在总结中如实说明）

## 5. 实验数量与充分性

- **实验组数**：至少包含以下三类实验：
  - 技能复用率对比（在已见网站上提升 1.7×）。
  - 跨网站成功率对比（Mind2Web 提升 9.4%，未见网站提升 13.9%）。
  - 步骤效率对比（步骤减少 >20%）。
  - 自探索设置下的任务质量与泛化技能评估。
- **充分性评估**：
  - 实验覆盖了**已见网站**（技能复用）和**未见网站**（零样本泛化）两种典型场景，具备一定代表性。
  - 消融实验：文中提到“compared to baseline methods”，但未明确报告是否进行了消融实验（如移除多态抽象的影响）。从摘要看，可能缺少对多态抽象各组件（抽象目标 vs 实现分离）的独立消融分析。
  - 客观性与公平性：由于未列出具体基线方法及统计显著性测试，难以全面判断实验设计是否严格公平。但结果数值明确，可认为具有初步说服力。

## 6. 主要结论与发现

1. **多态抽象显著提升技能复用**：在已见网站上，技能复用率提升至 1.7 倍。
2. **显著提升跨网站成功率**：在 Mind2Web 数据集上提升 9.4 个百分点，在完全未见网站上提升 13.9 个百分点。
3. **减少执行步骤**：相比基线方法平均减少超过 20% 的步骤，表明技能更高效。
4. **自探索学习收益**：智能体主动提出更好的任务目标，从而学得更具泛化性的技能。
5. **核心发现**：将技能的目标与执行解耦是培养自主智能体持续学习与泛化的关键。

## 7. 优点

- **方法创新性强**：将软件工程中的多态概念引入智能体技能学习，视角新颖。
- **实验度量全面**：同时评估了复用率、成功率、效率，并覆盖已见/未见网站。
- **泛化能力提升显著**：未见网站提升 13.9% 是较强信号。
- **开源代码**：提供 GitHub 链接便于复现和进一步研究。

## 8. 不足与局限

- **算力信息缺失**：未报告训练/推理所需的计算资源，无法评估方法的经济性。
- **基线方法不明确**：未列出具体对比方法，读者难以判断改进来源是否来自多态抽象本身还是其他工程细节。
- **消融实验不充分**：未展示去除抽象目标、只保留实现等消融结果，无法确认各组件贡献。
- **应用限制**：当前实验场景仅限于网页导航，是否适用于其他环境（如机器人、API 调用）未验证。
- **可能偏差风险**：自探索设置中，智能体自主提出任务，可能选择简单或已见过类似的任务，导致泛化评估数据存在选择偏差。

（完）
