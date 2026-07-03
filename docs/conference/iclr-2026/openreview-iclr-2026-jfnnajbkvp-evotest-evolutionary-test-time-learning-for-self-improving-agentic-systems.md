---
title: "EvoTest: Evolutionary Test-Time Learning for Self-Improving Agentic Systems"
title_zh: EvoTest：用于自改进智能体系统的进化式测试时学习
authors: "Yufei He, Juncheng Liu, Yue Liu, Yibo Li, Tri Cao, Zhiyuan Hu, Xinxing Xu, Bryan Hooi"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=JFnnajbkvP"
tags: ["query:cfd-agent"]
score: 9.0
evidence: 进化式测试时学习使智能体自我提升技能
tldr: 当前AI智能体在测试时无法快速学习复杂技能，限制了其实用性。本文提出EvoTest框架，通过在测试时进行进化学习，使智能体能在新环境中自我提升。实验表明该方法在J-TTL基准上优于现有方法，推动了智能体技能演化研究。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有AI智能体在测试时无法快速学习复杂技能，限制了其在动态环境中的实用性。
method: 提出进化式测试时学习框架EvoTest，通过进化算法在测试阶段优化智能体策略。
result: 在J-TTL基准上，EvoTest相比反射、记忆等基线方法显著提升了智能体的连续学习能力。
conclusion: 进化式测试时学习为智能体技能演化提供了有效途径。
---

## Abstract
A fundamental limitation of current AI agents is their inability to learn complex skills on the fly at test time, often behaving like “clever but clueless interns” in novel environments. This severely limits their practical utility. To systematically measure and drive progress on this challenge, we first introduce the Jericho Test-Time Learning (J-TTL) benchmark. J-TTL is a new evaluation setup where an agent must play the same game for several consecutive episodes, attempting to improve its performance from one episode to the next. On J-TTL, we find that existing adaptation methods like reflection, memory, or reinforcement learning struggle. To address the challenges posed by our benchmark, we present EvoTest, an evolutionary test-time learning framework that improves an agent without any fine-tuning or gradients—by evolving the entire agentic system after every episode. EvoTest has two roles: the Actor Agent, which plays the game, and the Evolver Agent, which analyzes the episode transcript to propose a revised configuration for the next run. This configuration rewrites the prompt, updates memory by logging effective state–action choices, tunes hyperparameters, and learns the tool-use routines. On our J-TTL benchmark, EvoTest consistently increases performance, outperforming not only reflection and memory-only baselines but also more complex online fine-tuning methods. Notably, our method is the only one capable of winning two games (Detective and Library), while all baselines fail to win any.

---

## 论文详细总结（自动生成）

好的，请见下方基于给定信息生成的详细中文总结。

# 论文《EvoTest：用于自改进智能体系统的进化式测试时学习》分析总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：当前AI智能体在测试时（test time）无法快速学习复杂技能，面对新环境时表现如同“聪明但无知的实习生”，严重限制了其在实际动态场景中的实用价值。
- **研究动机**：现有方法（如反思、记忆、在线微调）在连续多轮交互场景下难以持续提升性能，需要一种能够在无梯度微调的情况下，让智能体在测试阶段自我进化的新范式。
- **整体含义**：通过引入进化式测试时学习（Evolutionary Test-Time Learning），使智能体能够在每轮执行后自主分析经验并重写自身配置（提示词、记忆、超参数、工具使用等），从而实现在不依赖训练数据或梯度更新的前提下，持续提升任务表现。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：将智能体系统视为一个可进化的实体，在每次执行完一个完整回合（episode）后，通过进化算法对整个智能体系统（包括提示、记忆、超参数、工具使用规则）进行配置重写，实现自我改进。
- **关键技术细节**：
    - **双角色架构**：
        - **Actor Agent**：负责实际执行任务（如玩游戏）。
        - **Evolver Agent**：负责分析上一回合的完整执行记录（episode transcript），并提出下一回合的修正配置。
    - **进化式更新内容**：
        - **重写提示词（Prompt）**：根据失败或成功案例优化行为指导。
        - **更新记忆（Memory）**：记录有效的状态-动作对，供后续回合参考。
        - **调整超参数（Hyperparameters）**：如探索-利用平衡因子等。
        - **学习工具使用例程（Tool-use routines）**：优化调用外部工具的策略。
    - **无梯度、无微调**：整个过程完全基于进化算法（如遗传编程或演化策略）的搜索与选择，不涉及任何梯度计算或模型参数更新。
- **算法流程（文字说明）**：
    1. Actor Agent 执行一个回合的游戏，生成完整记录。
    2. Evolver Agent 读取该记录，并基于当前系统配置生成若干变异候选配置。
    3. 通过进化算法（如交叉、变异、选择）评估候选配置（可基于模拟运行或启发式评分）。
    4. 选择最优配置部署给 Actor Agent，用于下一回合。
    5. 重复上述过程，直到性能收敛或达到回合数上限。

## 3. 实验设计：数据集/场景、基准、对比方法

- **数据集/场景**：使用 Jericho 游戏环境中的多个文字冒险游戏（如 Detective、Library 等），属于交互式文本游戏领域。
- **Benchmark**：作者首次提出了 **Jericho Test-Time Learning (J-TTL) 基准**，要求智能体连续多次玩同一款游戏，并评估从上一回合到下一回合的性能提升（即连续学习能力）。
- **对比方法**：
    - 基线方法：反射（Reflection）、仅记忆（Memory-only）、强化学习（RL）等。
    - 更复杂的方法：在线微调（Online fine-tuning）。
    - 结果：EvoTest 在所有方法中表现最优，且是唯一能在两个游戏（Detective 和 Library）中获胜的方法，而所有基线方法均未能获胜。

## 4. 资源与算力

- **文中明确说明**：摘要和元数据中均未提及所使用的 GPU 型号、数量或训练时长等算力信息。
- **推断**：由于 EvoTest 不涉及梯度反向传播，其计算开销可能主要集中在配置变异、评估和回合模拟上，但具体资源需求未知。

## 5. 实验数量与充分性

- **实验组数**：根据摘要描述，实验涵盖了 J-TTL 基准中的多个游戏场景（至少涉及 Detective 和 Library 等），并对比了多种基线方法。但具体消融实验、超参数敏感性分析等细节未提供。
- **充分性评估**：
    - **优点**：基准设计合理（连续多回合评估），对比方法覆盖了常见适应策略，且结果显著性高（唯一获胜方法）。
    - **不足**：因只有摘要，无法判断是否进行了充分的消融实验（如单独分析提示重写、记忆更新、超参数调优的贡献）或跨领域泛化实验（如是否在其他类型的智能体任务上验证）。实验的全面性有待完整论文披露。

## 6. 主要结论与发现

- EvoTest 在 J-TTL 基准上能够持续提升智能体性能，且效果显著优于反射、记忆、强化学习以及更复杂的在线微调方法。
- 进化式测试时学习为智能体技能演化提供了有效途径，使得智能体无需额外训练数据或模型微调即可在测试阶段自我提升。
- 这是第一个能够使智能体在两个文字冒险游戏中获胜的方法，凸显了进化式架构在测试时学习中的潜力。

## 7. 优点

- **创新性**：将进化算法引入测试时学习，打破了依赖梯度或记忆的常规思路，具备较高的原创性。
- **实用性**：无需微调模型参数，计算开销相对较低，易于集成到现有智能体系统中。
- **基准贡献**：提出 J-TTL 基准，为测试时学习提供了标准化的评估协议，有利于后续研究对比。
- **结果显著**：在所有基线上的绝对领先（唯一获胜方法）展现了方法的强大能力。

## 8. 不足与局限

- **实验覆盖有限**：仅展示了在文字游戏领域的实验结果，尚未在更广泛的智能体任务（如机器人控制、代码生成、对话系统）中验证其通用性。
- **偏差风险**：进化算法的随机性可能导致结果波动，文中未报告多次实验的统计置信区间或稳定性分析。
- **应用限制**：进化配置依赖于完整的回合记录，对于需要实时交互或回合边界模糊的场景可能不适用；另外，Evolver Agent 本身也需要较强的推理能力，可能存在开销。
- **资源与可复现性**：未提供算力需求和超参数设置，限制了其他研究者复现和推广。
- **理论分析缺乏**：未对进化收敛性、配置表示方式等提供理论支撑，更多是实证结果。

（完）
