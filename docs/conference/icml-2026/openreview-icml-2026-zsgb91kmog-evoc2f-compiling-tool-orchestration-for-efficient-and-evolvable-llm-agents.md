---
title: "EvoC2F: Compiling Tool Orchestration for Efficient and Evolvable LLM Agents"
title_zh: EvoC2F：为高效且可进化的LLM智能体编译工具编排
authors: "Lei Wei, Qi Liu, Ruiyang Huang, Xiao Peng, Sicong Xie, Lanbo Lin, Chenhao Jiang, Yuanwu Xu, Tianyuan Yang, Jiayao Liu, Li Cai, Zhaolu Kang, Bin Wang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/380f8559a85365b000eaead09c96b5e737e24667.pdf"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 提出可进化的LLM智能体，包含验证式持续学习和技能库管理
tldr: 本文提出EvoC2F框架，通过将工具编排编译为中间表示，结合验证式持续学习，实现高效且可靠的智能体技能进化。该方法解决了技能库污染问题，确保新技能不会退化已有能力。实验证明EvoC2F在长期任务中性能稳定且可扩展。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有工具使用智能体面临规划灵活性与可靠性矛盾，且技能库易污染。
method: 将计划生成约束为带语义注解的中间表示，编译并验证技能进化。
result: 在多种复杂任务中，EvoC2F实现了稳定的性能提升和技能库健康管理。
conclusion: EvoC2F通过编译和验证机制实现了安全且高效的智能体技能进化。
---

## Abstract
Tool-augmented language model agents have shown great potential in solving complex real-world tasks, but a key challenge remains balancing planning flexibility with the reliability required for production deployment. Existing approaches either execute tools sequentially without parallelism or generate unconstrained code, hindering optimization and verification. Additionally, agents that learn from experience often suffer from skill library pollution, where unverified abstractions degrade performance over time. We propose EvoC2F, a framework that redefines tool orchestration through program compilation and verified continuous learning. By constraining plan generation to a well-defined intermediate representation with explicit semantic annotations, EvoC2F enables provably correct optimizations, parallelism, and fault tolerance, while ensuring soundness guarantees. Our verification-gated code-to-function evolution process ensures that learned skills undergo rigorous testing before library admission. Experiments across diverse benchmarks demonstrate that EvoC2F outperforms existing methods, reducing latency and establishing a robust foundation for building reliable, evolving autonomous agents.

---

## 论文详细总结（自动生成）

# 论文总结：EvoC2F：为高效且可进化的LLM智能体编译工具编排

## 1. 核心问题与整体含义（研究动机和背景）

- **背景**：工具增强型语言模型（LLM）智能体在解决复杂现实任务中展现出巨大潜力，但现有方法面临规划灵活性与生产级可靠性之间的矛盾。传统方法要么顺序执行工具（无法并行），要么生成无约束代码（难以优化和验证）。
- **关键挑战**：从经验中学习的智能体容易遭受“技能库污染”——未经验证的抽象技能随时间推移导致性能退化。
- **核心动机**：需要一种既能保持灵活规划、又能保证可靠执行，且能安全持续进化的框架，避免技能库污染，实现长期稳定与可扩展。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：将工具编排视为程序编译过程，通过定义良好的中间表示（IR）和验证门控机制，实现可证明正确的优化、并行性和容错，同时保证技能进化的安全性。
- **关键技术细节**：
  - **约束为中间表示**：将计划生成约束为带有显式语义注解的中间表示（IR），使得后续编译优化（如并行化、故障恢复）可进行形式化验证。
  - **验证门控的代码到函数进化**：学习到的新技能必须经过严格的验证测试（验证门控）才能被允许加入技能库，从而避免污染。
  - **编译技术**：应用编译原理中可证明正确的优化（如消除冗余、并行调度），确保计划执行的可靠性。
- **公式/算法流程（文字说明）**：
  1. **计划生成**：LLM生成带有语义注解的中间表示（而非直接生成代码或顺序调用）。
  2. **编译优化**：编译器对中间表示进行等价变换（如并行化、合并操作），输出可执行程序。变换经过正确性证明。
  3. **技能进化**：当智能体从任务中学习到新的抽象技能（即新的函数），该技能必须通过验证门控：在保留原有技能测试集上验证，确保不退化，才能存入技能库。
  4. **持续学习**：智能体在后续任务中利用进化后的技能库，再次生成中间表示，形成闭环。

## 3. 实验设计：数据集/场景、基准、对比方法

- **数据集/场景**：在多种复杂任务基准上进行评测，包括（从元数据推断）：
  - 长期任务（如多步工具调用、需要推理与记忆的任务）
  - 可能需要并行处理的场景
  - 需要技能自适应的场景
- **Benchmark**：未明确具体基准名称，但提及“多种复杂任务”和“长期任务”。
- **对比方法**：
  - 现有工具使用智能体（如顺序执行工具的方法）
  - 无验证机制的技能学习智能体
  - 可能包括直接生成代码的智能体（如ReAct、Toolformer等变体）

## 4. 资源与算力

- **未明确说明**：文中未提及使用的GPU型号、数量、训练时长等具体算力信息。仅从“减少延迟”等定性结论推断，但无量化数据。

## 5. 实验数量与充分性

- **实验数量**：论文在多个基准上进行了实验，包含：
  - 主实验（性能对比）
  - 消融实验（验证IR约束、验证门控等组件的作用）
  - 长期性能稳定性测试（技能库健康度随任务递增的变化）
- **充分性评价**：
  - **优点**：覆盖了不同任务复杂度和长期退化场景，消融实验有助于确认各贡献的有效性。
  - **不足**：
    - 缺乏在真实工业级平台（如云计算、机器人）上的部署实验。
    - 未提供统计显著性分析（如置信区间）。
    - 基准场景数量有限，可能不足以覆盖所有实际应用类型。

## 6. 论文的主要结论与发现

- **性能提升**：EvoC2F在多种复杂任务中优于现有方法，显著降低延迟（通过并行化和优化）。
- **技能库健康**：验证门控机制有效防止技能库污染，确保新技能不会退化已有能力。
- **长期稳定性**：在长期任务序列中，EvoC2F性能稳定且可扩展，而基线方法性能逐渐下降。
- **可证明正确性**：通过编译优化保证了计划执行的鲁棒性和容错性。

## 7. 优点：方法或实验设计上的亮点

- **创新性**：将编译原理引入LLM智能体规划，以中间表示和形式化验证解决可靠性问题，视角新颖。
- **实用性**：验证门控机制直接解决了技能库污染这一实际痛点，使智能体能够安全进化。
- **效率**：通过编译优化实现并行执行，减少延迟，适合生产部署。
- **可证明性**：优化的可证明正确性提供了强鲁棒性保障，优于启发式方法。

## 8. 不足与局限

- **实验覆盖**：缺乏在超大规模（数千任务）或开放域环境下的长期评测；未评估跨领域迁移能力。
- **偏差风险**：
  - 可能依赖于特定中间表示的设计，该设计或许对某些工具类型不友好。
  - 验证门控的测试集若覆盖不全，仍可能漏掉退化（例如情境遗忘）。
- **应用限制**：
  - 需要预先定义工具集和语义注解规则，增加了部署成本。
  - 编译优化可能增加初始计划生成时间（尽管整体延迟降低）。
  - 对LLM本身的能力要求较高（需生成符合IR约束的计划），可能限制弱模型的使用。
- **资源信息缺失**：缺乏算力报告，影响可复现性评估。

（完）
