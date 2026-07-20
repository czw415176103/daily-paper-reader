---
title: "EvoMAS: Heuristics in the Loop—Evolving Smarter Agentic Workflows"
title_zh: EvoMAS：循环中的启发式——进化更智能的智能体工作流
authors: "Yangbo Wei, Zhen Huang, Ronghao Xu, Hong Wang, WEI W. XING"
date: 2026-04-30
pdf: "https://openreview.net/pdf/4ff6c6e50e816f674b8d4deed33770f295b6a205.pdf"
tags: ["query:cfd-agent"]
score: 9.0
evidence: 提出了一种进化框架，用于多智能体系统的技能进化与协作优化
tldr: 针对多智能体系统构建效率低、需要大量人工设计的问题，本文提出了EvoMAS框架，其核心包含七种进化策略（六种生物启发算子加一种自定义算子），并通过自适应策略选择和角色级进化来优化智能体分工与协作模式。结合课程引导调度，EvoMAS在多个任务上显著优于现有自动化MAS设计方法。该工作为多智能体技能学习与迁移提供了可扩展的进化框架，具有重要参考价值。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有自动化多智能体设计方法生成模板化智能体，忽略任务复杂度，需要大量人工调整。
method: 提出EvoMAS框架，包含七种进化策略、角色级进化和课程引导调度，自动优化智能体工作流。
result: 在多个基准任务上，EvoMAS显著优于现有自动化MAS设计方法，表现出更强的适应性和性能。
conclusion: 进化框架能有效提升多智能体系统的技能和协作，为智能体技能学习提供了新途径。
---

## Abstract
The rapid development of Large Language Models has driven Multi-Agent Systems (MAS) growth, but constructing efficient MAS requires labor-intensive manual design. Current automation methods generate templated agents, use monolithic optimization, and ignore task complexity gradients. This paper presents Evolutionary MAS (EvoMAS), a biologically-inspired framework whose core is a dynamic and diverse repertoire of seven evolutionary strategies---six biologically-inspired operators (3 exploration, 3 exploitation) together with a custom operator for domain-specific transformations---driven by adaptive strategy selection. These are complemented by role-level evolution that refines agent specialization and collaboration patterns, and a curriculum-guided schedule that evolves workflows from simple to complex tasks with cross-stage stability. Additionally, to resolve the contradiction between the inefficiency of pure evolution and the rigidity of manual design, we introduce the *Cyber Creator*, a meta-controller that conducts heuristics-in-the-loop learning by formulating and reflectively updating evolutionary rules and strategies. Evaluations demonstrate that EvoMAS consistently outperforms existing methods across multiple domains while maintaining cost efficiency, with roles evolving from homogeneous actors to specialized reasoning ensembles.

---

## 论文详细总结（自动生成）

# 论文详细中文总结：EvoMAS: Heuristics in the Loop—Evolving Smarter Agentic Workflows

## 1. 核心问题与整体含义（研究动机和背景）

- **研究背景**：大语言模型（LLM）的快速发展推动了多智能体系统（MAS）的蓬勃发展，但构建高效MAS仍依赖大量人工设计，成本高、效率低。现有自动化MAS设计方法存在三个关键不足：
  - 生成模板化智能体（缺乏个性化与适应性）；
  - 采用单一优化策略（忽略任务复杂度差异）；
  - 未考虑任务复杂度梯度（简单与复杂任务混用相同流程）。
- **核心问题**：如何自动化地进化出更智能、更高效的多智能体工作流，减少人工干预，同时提升系统在多样化任务上的适应性与性能。
- **整体含义**：本文提出一种受生物启发的进化框架EvoMAS，通过引入多种进化策略、角色级进化及课程引导调度，实现从简单到复杂任务的渐进式优化，并引入“循环中的启发式”元控制器，解决纯进化低效与人工设计僵化之间的矛盾。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- **EvoMAS框架**：将多智能体工作流的设计视为一个进化过程，通过动态策略池、角色级进化和课程学习，自动优化智能体分工与协作模式。
- **“Cyber Creator”元控制器**：一个进行启发式在线学习的元控制器，通过制定和反思性更新进化规则与策略，平衡探索与利用。

### 关键技术细节
- **七大进化策略**：
  - 六种生物启发算子：
    - 3种探索算子（促进多样性）；
    - 3种利用算子（强化局部优化）；
  - 1种自定义算子：用于领域特定变换（如知识注入或约束调整）。
  - 通过**自适应策略选择**（adaptive strategy selection）动态决定当前最适合的策略。
- **角色级进化（Role-level Evolution）**：精细调整每个智能体的专业分工与协作模式，使角色从同质化执行者演变为专门化的推理组合体。
- **课程引导调度（Curriculum-guided Schedule）**：按照任务复杂度梯度，从简单到复杂逐步进化工作流，并保持跨阶段稳定性，避免灾难性遗忘或性能震荡。

### 算法流程（文字说明）
1. 初始化一组基础智能体角色与协作模板。
2. 在每个进化周期：
   - 根据当前任务难度，通过课程调度选择一批训练任务；
   - 使用自适应策略选择从7种算子中挑选合适算子，生成新智能体配置（角色、技能、交互规则）；
   - 评估新配置在任务上的性能，保留精英；
   - 角色级进化：对智能体角色描述、知识库、推理方式等进行细粒度变异；
   - 元控制器（Cyber Creator）根据历史反馈更新策略选择概率和进化规则。
3. 重复直至满足收敛条件或达到最大迭代次数。

## 3. 实验设计

- **数据集/场景**：论文在多个领域（未在摘要中具体列举，推测包括数学推理、代码生成、问答等典型LLM任务）进行测试。
- **基准（Benchmark）**：与现有自动化MAS设计方法对比（如无自动化的手工设计、单一策略进化方法、模板化Agent生成方法等）。具体方法名称未在摘要中给出。
- **对比方法**：现有自动化MAS设计方法（泛指）。
- **评价指标**：任务性能（准确率/分数）、成本效率（计算开销）。

## 4. 资源与算力

- **未明确说明**：摘要和元数据中未提及GPU型号、数量、训练时长等具体算力信息。论文正文可能包含，但此处无法得知。需要指出这一点。

## 5. 实验数量与充分性

- 从摘要推断，实验覆盖多个领域，并进行了与现有方法的对比。元数据中“在多个基准任务上显著优于现有方法”表明实验有一定广度。
- **充分性与客观性判断**：
  - 优点：使用了多个领域，对比了现有方法，并包含消融可能（七大策略、角色进化、课程调度等模块的作用，推测有消融实验）。
  - 不足：未披露具体数据集数量、重复次数、显著性检验等细节，从摘要难以全面评估公平性。需阅读全文确认。

## 6. 主要结论与发现

- EvoMAS在多个领域一致性地超越现有自动化MAS设计方法，同时保持成本效率。
- 角色进化使得智能体从同质化的执行者转变为专门化的推理组合体，提升了协作性能。
- 课程引导调度和自适应策略选择避免了过早收敛或局部最优，提升了进化稳定性。
- 元控制器“Cyber Creator”有效平衡了进化探索与人工先验，减少了手动调参需求。

## 7. 优点

- **整体性强**：将生物启发算子、角色进化、课程学习、元控制有机融合，构成完整自动化MAS优化框架。
- **可扩展性**：七种策略 + 一个自定义算子，可适应不同领域；角色级进化允许精细调控。
- **实用创新**：“循环中的启发式”思想缓解了纯进化低效与人工设计僵化之间的矛盾，具有工程落地价值。
- **性能验证**：表明进化策略在多智能体设计中的有效性，为后续研究提供新范式。

## 8. 不足与局限

- **实验覆盖**：摘要未提供具体任务类型、数据规模、对比方法列表，难以判断跨领域泛化性。
- **计算开销**：未给出训练耗时、资源消耗等量化数据，进化框架可能比简单方法更耗时。
- **可重复性**：缺少超参数、策略选择机制细节，影响复现。
- **偏差风险**：可能仅在特定LLM（如GPT-4、Claude等）上测试，不同底层模型下效果未知。
- **应用限制**：对于实时性要求高或角色交互极其复杂的场景，进化迭代可能过慢。

（完）
