---
title: "AI Agents with Human-Like Collaborative Tools: Adaptive Strategies for Enhanced Problem-Solving"
title_zh: 具有类人协作工具的AI代理：增强问题解决的适应性策略
authors: "Harper Reed, Michael Sugimura, Angelo Zangari"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=a6mORbW095"
tags: ["query:cfd-agent"]
score: 6.0
evidence: 为LLM代理配备协作工具提升问题解决效率
tldr: "提升AI代理问题解决能力的一种思路是赋予其类人协作工具。本文为Claude Code代理提供基于MCP的社交和日志工具，在34个编程挑战中，协作工具使困难问题成本降低15-40%，完成速度提升12-38%。这表明自动工具使用是增强代理技能的有效手段。"
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: LLM代理缺乏人类自然使用的协作工具，性能受限。
method: 为Claude Code代理集成MCP社交和日志工具，允许自主使用。
result: 在困难编程挑战中，成本、轮次和时间均显著降低。
conclusion: 协作工具作为性能增强器，可有效提升代理问题解决能力。
---

## Abstract
We investigate whether giving LLM agents the collaborative tools and autonomy that humans naturally use for problem-solving can improve their performance, providing Claude Code agents with MCP-based social media and journaling tools and the flexibility to use them as they see fit. Across 3 experimental runs for each variant across 34 Aider Polyglot Python programming challenges totaling 1,428 solved challenges, collaborative tools substantially improve challenging problem performance, delivering 15–40\% cost reductions, 12–27\% fewer turns, and 12–38\% faster completion compared to baseline agents. Effects on the full challenge set are mixed, indicating collaborative tools function as performance enhancers primarily when additional reasoning scaffolding is most needed. Surprisingly, different models naturally adopted distinct collaborative strategies without explicit instruction. Sonnet 3.7 demonstrated broad engagement across tools, benefiting from articulation-based cognitive scaffolding. Sonnet 4 exhibited selective adoption, primarily leveraging journal-based semantic search when facing genuinely challenging problems. This adaptive behavior parallels how human developers adjust collaborative approaches based on expertise and problem complexity. Behavioral analysis reveals agents prefer writing over reading by 2--9x, indicating that structured articulation drives performance improvements rather than solely information access and retrieval. Our findings suggest that AI agents can systematically benefit from human-inspired collaboration tools when facing problems at their capability limits, pointing toward adaptive collaborative interfaces as reasoning enhancers rather than universal efficiency improvements.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：当前大型语言模型（LLM）代理缺乏人类在解决问题时自然使用的协作工具（如社交媒体、日志记录、信息检索等），导致其推理能力和问题解决效率受限。
- **研究动机**：人类通过书写、交流、查阅资料等协作方式辅助思考；作者假设赋予LLM代理类似的人类协作工具，并允许其自主决定何时使用，可以显著提升代理在复杂问题上的表现。
- **整体含义**：探索将“人机协作”的认知策略迁移到AI代理中，使代理能像人类一样通过外部工具进行认知支架（cognitive scaffolding），从而在能力极限附近获得性能增益。

## 2. 方法论

- **核心思想**：在Claude Code代理中集成基于MCP（Model Context Protocol）的社交工具（如社交媒体API）和日志工具（如日记/笔记），让代理在编程挑战过程中自主决定是否使用这些工具，无需显式指令。
- **关键技术细节**：
  - 使用Claude Code（基于Anthropic的Claude系列模型）作为基础代理。
  - 集成两类工具：
    - **社交工具**：允许代理模拟发帖、搜索社交动态，用于获取外部信息或记录思考。
    - **日志工具**：允许代理记录中间思路、回顾历史，支持基于语义的搜索（journal-based semantic search）。
  - 代理在运行时可以自由决定调用工具的时机和频率，系统不强制干预。
- **算法流程**（文字描述）：代理接收编程挑战 → 自然语言推理 → 需要时调用MCP工具（写/读社交、写/读日志）→ 基于工具反馈调整策略 → 生成最终代码。

## 3. 实验设计

- **数据集/场景**：使用Aider Polyglot Python编程挑战，共34个挑战任务，涵盖不同难度级别。
- **Benchmark**：以无工具的Claude Code代理作为基线（baseline）。每个变体（不同模型配置）进行3次独立运行，共计1,428个已解决的挑战（34挑战 × 3运行 × 多种变体）。
- **对比方法**：
  - 基线（无协作工具）
  - 带协作工具的Claude Code代理（基于Sonnet 3.7和Sonnet 4两种模型）
  - 未对其他公开方法（如ReAct、Reflexion等）进行直接对比，主要进行自对比。

## 4. 资源与算力

- 文中**未明确说明**使用了何种GPU型号、数量或训练时长。仅提及使用Anthropic的Claude API进行推理调用，未报告具体硬件配置或训练成本。实验主要依赖云API，因此算力消耗取决于API调用次数和代理响应长度，但具体数值未披露。

## 5. 实验数量与充分性

- **实验数量**：3个实验运行（每个变体） × 34个挑战 × 多个模型变体（至少包含Sonnet 3.7和Sonnet 4两种模型），总计1,428个已解决挑战。此外还进行了行为分析（如读写比例）。
- **充分性**：实验覆盖了不同难度级别，并多次运行以减少偶然性。但仅测试了Python编程任务，任务类型单一；未与其他主流代理策略（如ReAct、CoT、工具增强等）进行横向比较，因此对比的充分性有限。
- **客观公平性**：基线对比是合理的，但可能存在API调用延迟、工具响应时间等外部变量未完全控制。实验设计较为规范，但缺乏消融研究（如仅社交工具、仅日志工具分别的效果）。

## 6. 主要结论与发现

- **协作工具显著提升困难问题性能**：在困难编程挑战上，成本降低15–40%，轮次数减少12–27%，完成时间加快12–38%。
- **效果因问题难度而异**：在全部34个挑战上效果混合（部分简单任务无提升甚至下降），说明协作工具主要作为“性能增强器”在最需要额外推理支架时发挥作用。
- **不同模型自适应采用不同协作策略**：
  - Sonnet 3.7：广泛使用所有工具，受益于“表达型认知支架”（articulation-based cognitive scaffolding）。
  - Sonnet 4：选择性使用，主要在面对真正困难问题时利用日志进行语义搜索。
- **行为模式**：代理书写（写日志、发帖）与阅读的比例为2:1至9:1，表明“结构化表达”（structured articulation）是性能提升的主要驱动因素，而不仅仅是信息获取。
- **总体结论**：类人协作工具可作为AI代理的推理增强器，尤其在其能力极限附近有效；未来可发展自适应协作接口。

## 7. 优点

- **方法创新性**：首次将人类协作工具（社交、日志）引入LLM代理，并允许自主使用，而非预设工具调用规则。
- **行为分析深入**：不仅报告性能指标，还分析了读写比例、不同模型的自适应策略差异，揭示“表达性认知支架”的重要性。
- **实验规模合理**：1,428次运行提供统计可靠性，多次重复减少偏倚。
- **结果具有可解释性**：发现工具效果与问题难度相关，解释了为何简单任务上效果不突出。

## 8. 不足与局限

- **实验覆盖单一**：仅使用Python编程挑战（Aider Polyglot），未在更广泛的任务（如对话、推理、决策、多模态）上验证。
- **缺乏与其他方法对比**：未与ReAct、Reflexion、AutoGPT等流行代理框架进行同等条件下的比较，难以判断该方法相对现有技术的优劣。
- **未披露算力细节**：无法评估资源消耗，可能影响实际部署可行性。
- **消融实验不足**：未单独评估社交工具和日志工具的贡献，也未测试移除某一工具的影响。
- **模拟而非真实环境**：社交工具为模拟API，并非真实社交媒体，生态效度有限。
- **模型依赖**：仅使用Claude系列模型（Sonnet 3.7和Sonnet 4），结果可能无法泛化到其他LLM（如GPT-4、Llama等）。
- **应用限制**：协作工具可能增加延迟和成本（尤其在简单任务上），且需要用户配置MCP环境。

（完）
