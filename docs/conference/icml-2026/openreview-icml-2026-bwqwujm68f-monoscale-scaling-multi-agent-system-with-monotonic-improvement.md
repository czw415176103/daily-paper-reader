---
title: "MonoScale: Scaling Multi-Agent System with Monotonic Improvement"
title_zh: MonoScale：通过单调改进扩展多智能体系统
authors: "Shuai Shao, Yixiang Liu, Bingwei Lu, Weinan Zhang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/097d2bafdd70497480aef1e760dd4cec90d9141d.pdf"
tags: ["query:cfd-agent"]
score: 7.0
evidence: 提出通过记忆实现智能体技能单调改进的框架
tldr: 该论文针对多智能体系统扩展时新加入智能体导致性能崩溃的问题，提出MonoScale框架，通过主动生成熟悉化任务，从成功和失败交互中提取证据并蒸馏为自然语言记忆，引导后续路由，实现单调性能提升，为多智能体系统的稳定扩展提供了新方法。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 在扩展多智能体系统时，新加入的异构智能体会导致路由冷启动和性能崩溃。
method: 提出MonoScale框架，生成智能体条件化的熟悉化任务，从交互中收集证据并蒸馏为可审计的自然语言记忆以指导路由。
result: 实现了多智能体系统扩展时的单调性能改进，避免了性能崩溃。
conclusion: 通过记忆驱动的路由更新机制可以安全地扩展多智能体系统。
---

## Abstract
In recent years, LLM-based multi-agent systems (MAS) have advanced rapidly, using a router to decompose tasks and delegate subtasks to specialized agents. A natural way to expand capability is to scale up the agent pool by continually integrating new functional agents or tool interfaces, but naive expansion can trigger performance collapse when the router cold-starts on newly added, heterogeneous, and unreliable agents. We propose MonoScale, an expansion-aware update framework that proactively generates a small set of agent-conditioned familiarization tasks, harvests evidence from both successful and failed interactions, and distills it into auditable natural-language memory to guide future routing. We formalize sequential augmentation as a contextual bandit and perform trust-region memory updates, yielding a monotonic non-decreasing performance guarantee across onboarding rounds under a non-interfering expansion assumption. Experiments on GAIA and Humanity's Last Exam show stable gains as the agent pool grows, outperforming naive scale-up and strong-router fixed-pool baselines. Our code is available here.

---

## 论文详细总结（自动生成）

# 论文《MonoScale: Scaling Multi-Agent System with Monotonic Improvement》详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：在基于大型语言模型（LLM）的多智能体系统（MAS）中，当通过持续集成新功能智能体或工具接口来扩展智能体池时，路由模块对新加入的异构、不可靠智能体存在“冷启动”问题，导致系统性能崩溃。
- **研究动机**：现有扩展方式（naive expansion）会因路由对新智能体缺乏经验而引发性能退化，阻碍MAS能力的持续增长。需要一种机制保证扩展过程中性能单调提升而非下降。
- **整体含义**：提出**MonoScale**框架，通过主动生成任务、收集交互证据并蒸馏为可审计的自然语言记忆，指导后续路由决策，从而实现多智能体系统扩展时性能的**单调非递减**改进。

## 2. 论文提出的方法论

### 核心思想
- 将智能体扩展过程建模为**上下文赌博机（contextual bandit）**问题，路由根据智能体记忆选择子任务分配。
- 引入**信任区域记忆更新（trust-region memory update）**，确保每次加入新智能体后，路由策略的更新不会导致性能下降，从而提供单调改进保证。

### 关键技术细节
1. **智能体条件化的熟悉化任务生成**：针对每个新加入的智能体，自动生成一小批与其能力相关的熟悉化任务，用于快速收集交互数据。
2. **证据收集与蒸馏**：从成功和失败交互中提取“证据”（evidence），将其转化为**可审计的自然语言记忆**（auditable natural-language memory），存储智能体的行为模式、成功率、失败原因等信息。
3. **记忆引导路由**：后续路由决策时，查询记忆库，优先选择记忆评分高的智能体执行子任务，避免冷启动导致的错误分配。
4. **非干扰扩展假设**：新智能体的加入不影响已有智能体的能力或行为，在此假设下，信任区域更新保证性能单调非递减。

### 算法流程（文字说明）
1. 初始化：已有智能体池和路由模型，维护每个智能体的记忆库。
2. 加入新智能体时：
   - 生成一组针对该智能体的熟悉化任务。
   - 路由尝试分配任务，收集执行结果（成功/失败）。
   - 从结果中提取证据，更新该智能体的记忆。
   - 对比更新前后的路由策略，若新策略导致性能下降，则回退到旧策略（信任区域约束）。
3. 每次扩展重复上述过程，实现性能单调提升。

## 3. 实验设计

- **数据集/场景**：
  - **GAIA**：一个多智能体复杂任务推理基准。
  - **Humanity's Last Exam (HLE)**：一个高难度综合推理测试集。
- **Benchmark**：以扩展过程中的性能变化作为主要评估指标。
- **对比方法**：
  - **Naive scale-up**：不进行任何特殊处理，直接加入新智能体。
  - **Strong-router fixed-pool**：使用强大的路由模型但保持智能体池固定（不扩展）。
- **结果**：MonoScale在智能体池增长时表现稳定增益，性能优于两种基线，且未出现性能崩溃。

## 4. 资源与算力

- 论文正文及元数据中**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅提及代码已开源，但未提供训练配置细节。

## 5. 实验数量与充分性

- **实验数量**：在两个数据集（GAIA、HLE）上进行了主要对比实验，并对比了两种基线方法。但元数据及摘要中未提及消融实验数量或详细统计分析。
- **充分性评估**：
  - 优点：覆盖了不同难度和类型的任务基准（GAIA侧重多智能体协同，HLE侧重知识推理），对比方法合理。
  - 不足：缺乏对超参数敏感性、不同扩展规模（如每次加入多个智能体）、不同路由架构的充分实验。未报告多次运行的统计方差，且缺少对“非干扰扩展假设”违背时的鲁棒性测试。总体而言，实验设计较为初步，充分性有限。

## 6. 论文的主要结论与发现

- **结论**：通过记忆驱动的路由更新机制可以安全地扩展多智能体系统，实现性能单调非递减改进，避免传统扩展方式的性能崩溃。
- **发现**：
  - 主动生成熟悉化任务并收集交互证据，能有效缓解新智能体的冷启动问题。
  - 自然语言记忆作为可审计的中间表示，既指导路由又提供可解释性。
  - 信任区域更新策略保证了扩展过程的稳定性。

## 7. 优点

- **理论保证**：将扩展形式化为上下文赌博机，并提供单调性能改进的数学保证，在方法论上具有创新性。
- **实用设计**：通过自然语言记忆实现可审计性，便于调试和验证路由决策，符合实际部署需求。
- **实验验证**：在GAIA和HLE两个有挑战性的基准上验证了有效性，且代码开源，可复现。

## 8. 不足与局限

- **实验覆盖不足**：仅测试了两个数据集，未在更多样化的MAS任务（如对话、协作推理）上验证；消融实验缺失，无法判断各组件（熟悉化任务生成、记忆蒸馏、信任区域更新）各自的贡献。
- **假设限制**：“非干扰扩展假设”过于理想，实际场景中新智能体可能影响已有智能体的协作模式或负载分布，论文未讨论此假设不成立时的应对。
- **资源开销**：未报告生成熟悉化任务和记忆蒸馏的计算成本，以及记忆库规模对路由速度的影响。
- **偏差风险**：熟悉化任务可能偏向于新智能体擅长的领域，导致记忆偏差；路由可能过度依赖记忆而忽视实时环境变化。

（完）
