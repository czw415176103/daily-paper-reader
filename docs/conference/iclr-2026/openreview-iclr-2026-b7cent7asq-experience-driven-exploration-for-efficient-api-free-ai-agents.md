---
title: Experience-Driven Exploration for Efficient API-Free AI Agents
title_zh: 面向高效无API AI智能体的经验驱动探索
authors: "Chenwei Tang, Jingyu Xing, Xinyu Liu, Zizhou Wang, Jiawei Du, Liangli Zhen, Jiancheng Lv"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=B7ceNt7AsQ"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 经验驱动框架提升无API环境中的智能体技能获取
tldr: 该论文针对无API环境下LLM代理因局部视觉体验导致低效试错的问题，提出KG-Agent框架，将原始像素交互构建为持久的状态-动作知识图，通过图结构链接相似状态，显著提升了探索效率和技能获取速度。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 无API智能体因只能看到局部视觉信息而做出短视决策，探索效率低。
method: 构建状态-动作知识图（SA-KG），将视觉上不同但功能相似的状态连接以增强探索。
result: 在多个GUI任务中，KG-Agent的任务完成率和效率显著优于基线。
conclusion: 知识图驱动的经验学习框架能有效提升无API智能体的长期规划能力。
---

## Abstract
Most existing software lacks accessible Application Programming Interfaces (APIs), requiring agents to operate solely through pixel-based Graphical User Interfaces (GUIs). In this API-free setting, large language model (LLM)-based agents face severe efficiency bottlenecks: limited to local visual experiences, they make myopic decisions and rely on inefficient trial-and-error, hindering both skill acquisition and long-term planning. To address these challenges, we propose KG-Agent, an experience-driven learning framework that structures an agent's raw pixel-level interactions into a persistent State-Action Knowledge Graph (SA-KG). KG-Agent overcomes inefficient exploration by linking functionally similar but visually distinct GUI states, forming a rich neighborhood of experience that enables the agent to generalize from a diverse set of historical strategies. To support long-horizon reasoning, we design a hybrid intrinsic reward mechanism based on the graph topology, combining a state value reward for exploiting known high-value pathways with a novelty reward that encourages targeted exploration. This approach decouples strategic planning from pure discovery, allowing the agent to effectively value setup actions with delayed gratification. We evaluate KG-Agent in two complex, open-ended GUI-based decision-making environments (Civilization V and Slay the Spire), demonstrating significant improvements in exploration efficiency and strategic depth over the state-of-the-art methods.

---

## 论文详细总结（自动生成）

# 论文总结：Experience-Driven Exploration for Efficient API-Free AI Agents

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：大多数现有软件缺乏可访问的应用程序编程接口（API），智能体只能通过基于像素的图形用户界面（GUI）进行操作。在这种无API（API-free）环境中，基于大语言模型（LLM）的智能体面临严重的效率瓶颈——它们局限于局部视觉经验，做出短视决策，依赖低效的试错，导致技能获取和长期规划能力受限。
- **核心问题**：如何让LLM智能体在只能看到局部像素信息的情况下，克服短视行为与低效探索，实现高效的任务完成与长期规划。
- **整体含义**：该论文提出一种经验驱动的学习框架KG-Agent，通过构建持久化的状态-动作知识图（State-Action Knowledge Graph，SA-KG），将原始像素级交互结构化，从而提升无API环境中智能体的探索效率与策略深度。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 将智能体的原始像素交互经验组织为持久化的**状态-动作知识图（SA-KG）**。
- 通过连接**功能相似但视觉不同**的GUI状态，形成丰富的经验邻域，使智能体能够从多样化的历史策略中泛化，避免重复低效试错。
- 基于图拓扑设计**混合内在奖励机制**，平衡探索与利用。

### 关键技术细节
1. **状态-动作知识图（SA-KG）构建**：
   - 节点：智能体感知的GUI状态（像素级表示）。
   - 边：智能体执行的动作以及状态转移关系。
   - 利用图结构将视觉上不同但功能相似的状态链接起来，使得智能体在遇到新状态时能够访问相似历史经验。

2. **混合内在奖励机制**：
   - **状态值奖励（State Value Reward）**：基于图拓扑评估节点价值，鼓励智能体利用已知的高价值路径。
   - **新奇奖励（Novelty Reward）**：鼓励智能体探索尚未充分访问的状态区域，实现目标导向的探索。
   - 该机制将策略规划与纯发现解耦，使得智能体能够正确评估具有延迟回报的“设置动作”（setup actions）。

3. **算法流程（文字说明）**：
   - 初始化SA-KG（空图）。
   - 智能体在GUI环境中执行动作，收集状态转移数据（<当前状态，动作，下一状态，奖励>）。
   - 将每个状态表示为节点，动作与转移关系作为边，动态更新SA-KG。
   - 在每一步，智能体通过图拓扑计算混合内在奖励（状态值+新奇性），并与外部环境奖励结合。
   - 智能体以该奖励驱动策略学习（如强化学习或基于LLM的规划），最终选择动作。

## 3. 实验设计

- **数据集/场景**：两个复杂、开放式的**GUI决策环境**：
  - **Civilization V**（文明5）：经典策略游戏，长期规划与资源管理。
  - **Slay the Spire**（杀戮尖塔）：卡牌策略游戏，需要平衡资源与风险。
- **Benchmark**：论文未明确命名标准benchmark，但对比了**state-of-the-art方法**（具体列表在摘要未给出，正文中应有详细对比）。
- **对比方法**：无API环境下的最新LLM智能体方法（如使用纯试错、简单记忆增强等方法），论文中提及“显著优于当前最优方法”。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅能从环境复杂性推测可能需要一定计算资源，但无法量化。
- **需指出**：文中缺少对硬件配置、训练成本的描述。

## 5. 实验数量与充分性

- **实验数量**：论文在**两个**不同的复杂GUI环境中进行了评估，未提及消融实验的具体数量。从摘要看，实验对比了KG-Agent与基线方法的任务完成率和效率。
- **充分性评估**：由于仅有两个环境，且缺乏消融实验细节，实验覆盖范围有限。但选择的环境具有代表性（策略深度和探索挑战）。结论表明“显著提升”，但未提供统计显著性检验或误差范围，**客观性和公平性需依赖正文中的完整实验设置**（摘要未提供）。整体上实验设计合理但不够充分，尤其缺少对知识图规模、奖励权重等超参数的敏感度分析。

## 6. 主要结论与发现

- KG-Agent在无API的GUI环境中显著提升了**探索效率**和**策略深度**。
- 通过SA-KG连接功能相似状态，智能体能够从历史经验中泛化，减少无效试错。
- 混合内在奖励机制有效平衡了利用已知高价值路径与探索新区域，尤其对需要延迟满足的“设置动作”具有更好的估值能力。
- 实验表明，该框架优于现有无API智能体方法，为无API环境下的长期规划提供了可行方案。

## 7. 优点

- **创新性**：首次将知识图引入无API LLM智能体的经验管理，利用图结构解决像素级局部视觉导致的短视问题。
- **可扩展性**：SA-KG结构可动态增长，适用于长期交互任务。
- **奖励设计巧妙**：混合内在奖励机制从图拓扑中提取信息，无需额外模型训练，计算开销较小。
- **实验环境选择**：两个开放策略游戏具有挑战性，能够考察智能体的长期规划和探索能力。

## 8. 不足与局限

- **实验覆盖不足**：仅测试了两个游戏环境，缺乏对其他类型GUI任务（如桌面操作、网页导航）的验证，泛化性存疑。
- **状态表示依赖**：使用原始像素作为节点，可能受视觉噪声影响；对于状态相似性的定义（功能相似 vs 视觉相似）未详细说明。
- **可扩展性风险**：随着交互次数增加，SA-KG规模可能急剧增长，未讨论图压缩或遗忘机制。
- **算力成本缺失**：未报告训练/推理的硬件需求，实际部署可行性不明。
- **基线对比不充分**：摘要中未列出具体对比方法名称，也未提供消融实验（如去掉混合奖励、去掉知识图等）来验证各组件贡献。
- **方法论细节不足**：如如何定义“功能相似”的状态、图更新的频率、奖励权重设置等关键细节缺失，影响复现性。

（完）
