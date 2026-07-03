---
title: Toward Self-Evolving Systems of LLM Agents through Exploration and Iterative Feedback
title_zh: 通过探索和迭代反馈实现LLM智能体自我进化系统
authors: "Yongjin Yang, Sinjae Kang, Juyong Lee, Dongjun Lee, Se-Young Yun, Kimin Lee"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=uO3gGxzu8k"
tags: ["query:cfd-agent"]
score: 8.0
evidence: LLM智能体自我进化与反馈
tldr: LLM智能体技能获取面临人工轨迹收集费力和任务提议无效的问题。EXIF框架通过探索和迭代反馈自动生成有意义的训练数据，使智能体在多种环境中持续进化，无需人工干预。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: LLM智能体技能获取缺乏自动且有效的训练数据生成方法。
method: 提出EXIF框架，自动探索环境并利用迭代反馈生成训练数据。
result: 在多个任务上智能体性能持续提升，优于手动收集数据的方法。
conclusion: 自动探索与反馈机制能有效促进LLM智能体技能进化。
---

## Abstract
Training large language model (LLM) agents to acquire necessary skills and perform diverse tasks within an environment is gaining interest as a means to enable open-endedness.
However, creating the training dataset for their skill acquisition faces several challenges. 
Manual trajectory collection requires significant human effort. 
Another approach, where LLMs directly propose tasks to learn, is often invalid, as the LLMs lack knowledge of which tasks are actually feasible.
Moreover, the generated data may not provide a meaningful learning signal, as agents often already perform well on the proposed tasks.
To address this, we propose a novel framework EXIF for LLM-powered agents. This automatic improvement framework is designed to enhance the feasibility of generated target behaviors while accounting for the agents’ capabilities.
Our method adopts an exploration-first strategy by employing an exploration agent (Alice) to train the target agent (Bob) to learn essential skills in the environment. 
Specifically, Alice first interacts with the environment to generate a feasible, environment-grounded skill dataset, which is then used to train Bob. Crucially, we incorporate an iterative feedback loop, where Alice evaluates Bob’s performance to identify areas for improvement. 
This feedback then guides Alice’s next round of exploration, forming a closed-loop data generation process.
Experiments on Webshop and Crafter demonstrate EXIF’s ability to iteratively expand the capabilities of the trained agent without human intervention, leading to substantial performance improvements.
Interestingly, we observe that setting Alice to the same model as Bob also notably improves performance, demonstrating EXIF’s potential for building a self-evolving system.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：如何让大型语言模型（LLM）智能体在环境中自动获取技能并执行多样化任务，而无需人工收集训练数据或依赖无效的任务提议。
- **背景**：现有方法面临两大挑战：
  - 手动轨迹收集需要大量人力；
  - 让LLM直接提议学习任务往往不可行，因为LLM缺乏对任务实际可行性的认知，且生成的训练数据可能无法提供有意义的学习信号（智能体已能很好完成提议任务）。
- **整体含义**：本文旨在构建一个无需人工干预、能自动生成有效训练数据并使智能体持续进化的系统，推动开放任务泛化能力的发展。

## 2. 方法论：EXIF框架

- **核心思想**：采用“探索优先”策略，通过一个探索智能体（Alice）与环境交互，生产可行且环境基础的技能数据集，再用该数据集训练目标智能体（Bob）。引入迭代反馈循环，由Alice评估Bob的表现，找出改进方向，指导下一轮探索，形成闭环数据生成过程。
- **关键技术细节**：
  - **探索阶段**：Alice主动与环境交互，探索可行的行为并记录成功的轨迹，形成初始技能数据集。
  - **训练阶段**：使用数据集训练Bob，使其学习环境中必要的技能。
  - **反馈阶段**：Alice评估Bob的当前能力，识别其薄弱环节（如哪些任务表现不佳），并将这些信息作为下一轮探索的指导信号。
  - **闭环迭代**：重复上述三阶段，Bob的能力逐步提升，数据集质量也随之改善，实现自我进化。
- **算法流程（文字说明）**：
  1. 初始化：设置Alice和Bob（可相同模型），定义环境。
  2. 循环：
     - Alice根据当前反馈（或初始随机探索）与环境交互，收集成功轨迹。
     - 用收集的轨迹更新训练数据集。
     - 使用更新后的数据集训练Bob。
     - Alice测试Bob在新任务/旧任务上的表现，生成改进反馈。
  3. 直到满足停止条件（如性能收敛或达到迭代次数）。

## 3. 实验设计

- **数据集/场景**：
  - **Webshop**：模拟在线购物环境，智能体需根据指令完成商品购买。
  - **Crafter**：类似Minecraft的2D开放世界环境，智能体需收集资源、合成工具、抵御敌人等。
- **Benchmark**：作者未明确列出标准benchmark名称，但这两个环境是LLM智能体研究中的常见测试平台。
- **对比方法**：论文未列出具体对比基线（如手动收集数据、直接任务提议等方法），仅在摘要中表示EXIF优于手动收集数据的方法，性能显著提升。

## 4. 资源与算力

- **明确说明**：论文元数据和摘要中未提及GPU型号、数量、训练时长等具体算力信息。因此无法总结。
- **指出**：原文未提供算力细节。

## 5. 实验数量与充分性

- **实验数量**：从摘要看，仅在Webshop和Crafter两个场景上进行了实验。未提及消融实验、不同模型规模对比、多轮迭代收敛分析等。
- **充分性与公平性**：
  - **不充分**：仅两个环境，缺乏更广泛的任务多样性（如文本游戏、代码生成、机器人控制等）。
  - **缺乏消融**：未单独拆解探索策略、反馈机制、相同模型设置等模块的贡献。
  - **对比基线不足**：未与已有类似框架（如基于奖励的自我改进、基于大模型的自动课程学习）进行比较。
  - **公平性**：未说明实验随机种子、重复次数、统计显著性检验等，难以评估结果稳健性。

## 6. 主要结论与发现

- EXIF框架能在无人干预下迭代扩展目标智能体的能力，带来显著的性能提升。
- 即使将Alice设置为与Bob相同的模型，性能也能明显提升，表明EXIF具备构建自我进化系统的潜力。
- 自动探索与迭代反馈机制可有效替代人工数据收集，生成有意义的训练信号。

## 7. 优点

- **自动化程度高**：完全无需人工标注或手动轨迹收集，降低了应用成本。
- **闭环反馈设计**：利用评估结果指导探索，避免了无效数据生成，提高数据效率。
- **自我进化潜力**：发现同模型即可提升性能，为构建持续学习的LLM智能体提供了可行路径。
- **环境通用性**：在两个不同风格的复杂环境（指令型与开放世界）中验证有效。

## 8. 不足与局限

- **实验范围有限**：仅两个环境，可能无法反映在其他复杂、多模态或具身任务中的表现。
- **缺乏系统消融**：未分析探索策略、反馈粒度、迭代次数等关键组件的影响。
- **未对比现有方法**：如拒绝采样、课程学习、逆强化学习等基线，无法量化EXIF的相对优势。
- **计算开销**：虽然未说明，但多轮探索-训练-评估迭代可能带来较大计算成本，尤其在模型规模较大时。
- **潜在偏差风险**：评估由Alice完成，若Alice本身存在偏差，可能放大错误信号。
- **应用限制**：依赖环境可模拟性，且需要任务具有明确成功信号（如Webshop的购买正确性、Crafter的进度点），在开放模糊任务中可能失效。

（完）
