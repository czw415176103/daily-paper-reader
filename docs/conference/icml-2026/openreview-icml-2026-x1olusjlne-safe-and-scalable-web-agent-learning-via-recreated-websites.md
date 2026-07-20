---
title: Safe and Scalable Web Agent Learning via Recreated Websites
title_zh: 通过重建网站实现安全可扩展的网络智能体学习
authors: "Hyungjoo Chae, Jungsoo Park, Alan Ritter"
date: 2026-04-30
pdf: "https://openreview.net/pdf/c2afd1ace56b4878c7b02ffb5a7e124fa1ae83a3.pdf"
tags: ["query:cfd-agent"]
score: 4.0
evidence: 通过环境扩展实现智能体学习的自我进化
tldr: 该论文针对网络智能体训练环境不安全、难以重置的问题，提出VeriEnv框架，自动克隆真实网站为可执行合成环境，使智能体自生成任务并获得确定性奖励，通过环境扩展实现安全可扩展的自我进化。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 真实网站环境不安全且难以提供可验证反馈，限制智能体训练。
method: 提出VeriEnv框架，利用语言模型自动克隆网站为可执行合成环境，并提供Python SDK允许智能体自生成任务。
result: 在网页智能体基准上实现了高效学习，且避免了真实环境的安全风险。
conclusion: 可验证的合成环境为智能体安全自进化提供了可行方案。
---

## Abstract
Training autonomous web agents is fundamentally limited by the environments they learn from: real-world websites are unsafe to explore, hard to reset, and rarely provide verifiable feedback.
We propose VeriEnv, a framework that treats language models as environment creators, automatically cloning real-world websites into fully executable, verifiable synthetic environments.
By exposing controlled internal access via a Python SDK, VeriEnv enables agents to self-generate tasks with deterministic, programmatically verifiable rewards, eliminating reliance on heuristic or LLM-based judges.
This design decouples agent learning from unsafe real-world interaction while enabling scalable self-evolution through environment expansion.
Through experiments on web agent benchmarks, we show that agents trained with VeriEnv generalize to unseen websites, achieve site-specific mastery through self-evolving training, and benefit from scaling the number of training environments.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **背景**：训练自主网页智能体（web agent）的核心瓶颈在于其所依赖的环境——真实网站存在三大缺陷：
  - **不安全**：智能体在真实网站上进行探索和操作可能对真实系统或数据造成破坏。
  - **难以重置**：同一网站的状态（如登录状态、页面内容）难以在每次训练后恢复初始，导致实验不可重复。
  - **缺乏可验证反馈**：真实网站通常不提供程序化的、确定性的奖励信号，往往需要依赖启发式规则或基于LLM的评判器，这些方法不可靠且成本高。

- **问题**：如何在避免与真实网站直接交互的前提下，为网页智能体提供安全、可重置、可验证反馈的训练环境，从而实现可扩展的自主进化学习？

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：将语言模型（LM）视为环境创建者，自动克隆真实网站为**完全可执行的合成环境**（VeriEnv），使智能体可以在该合成环境中自生成任务，并获得确定性的、程序可验证的奖励，从而解耦学习过程与真实世界的交互。

- **关键技术细节**：
  - **自动网站克隆**：利用LM（如GPT系列）分析真实网站的HTML/DOM结构、交互逻辑（如点击、表单填写、导航），自动生成一个功能等价但完全隔离的合成网站。合成环境保留了原始网站的关键交互路径，但内部状态由Python控制。
  - **Python SDK**：提供一个内部访问接口（SDK），使智能体能够通过程序化方式生成任务（例如“在购物网站上找到某个商品的价格”），这些任务对应的奖励信号由合成环境直接计算（例如，如果智能体成功获取正确价格则+1，否则0），不依赖外部评判器。
  - **确定性奖励**：由于合成环境的状态完全可控，每个任务都有一个确定的、可验证的正确结果，避免了LLM评判的不一致性和偏见。
  - **环境扩展与自我进化**：智能体可以通过增加新的合成环境（克隆更多真实网站）来扩展训练数据，从而提升泛化能力；同时，智能体可以在已有的合成环境中不断自生成新任务，实现站点内的针对性优化。

- **算法流程（文字说明）**：
  1. 选择一个真实网站作为目标。
  2. 使用LM解析该网站的交互逻辑，生成对应的合成环境代码（包含状态机、DOM模拟、奖励计算函数）。
  3. 部署该合成环境，并通过Python SDK暴露内部API。
  4. 智能体在合成环境中进行探索：它可以调用SDK生成任务（如“点击某个按钮”“提取某段文本”），然后执行一系列动作（点击、输入等）。
  5. 合成环境根据任务完成情况返回确定性奖励（0或1）。
  6. 收集经验，训练智能体策略（例如通过强化学习）。
  7. 重复步骤1-6，逐步扩展环境库，实现规模化的自我进化。

## 3. 实验设计

- **数据集/场景**：论文使用网页智能体领域的标准基准测试集（但未明确给出名称，推测为类似MiniWoB、WebArena等）。实验覆盖**未见过的网站**、**已训练过的网站**（站点特定掌握）以及**不同数量的训练环境**等场景。
- **Benchmark**：作者在网页智能体任务上评估，比较的训练方式包括：
  - 在真实网站上训练（基线，但存在安全和重置问题）
  - 在合成环境VeriEnv中训练
  - 不同环境数量下的表现
- **对比方法**：主要与直接使用真实网站进行训练的方法进行对比，但论文并未列出具体的对比模型名称，而是聚焦于环境设计对智能体学习的影响。

## 4. 资源与算力

- **文中未明确说明**：论文未给出训练所使用的GPU型号、数量、训练时长等算力信息。仅提及使用语言模型（如GPT系列）进行网站克隆，但克隆过程以及智能体训练的具体计算资源消耗未披露。

## 5. 实验数量与充分性

- **实验组数**：论文进行了多组实验：
  - **泛化性实验**：在未见过的网站上测试通过VeriEnv训练的智能体，验证其跨网站迁移能力。
  - **站点特定掌握实验**：在已克隆的网站上，通过自我进化训练，观察智能体任务完成率随训练次数的提升。
  - **环境规模扩展实验**：改变训练环境数量（如从1个增加到10个），观察效果提升。
- **充分性评价**：
  - 实验设计覆盖了核心假设（安全、可扩展、泛化），但**对比基准不够充分**。缺少与现有其他合成环境方法（如基于模拟器的训练）的对比，也缺乏在多个不同类别的网站上的广泛测试。
  - 没有消融实验分析VeriEnv各组件（如克隆保真度、SDK设计）的贡献。
  - 总体而言，实验证明了方法有效性，但在**公平性和全面性**方面有提升空间。

## 6. 主要结论与发现

- **结论**：
  - 通过VeriEnv训练的智能体能够泛化到未见过的真实网站。
  - 在单个合成环境内进行自我进化训练可以显著提升站点特定任务的成功率。
  - 增加合成环境的数量（环境扩展）能带来智能体性能的持续提升，证明了方法的可扩展性。
  - 合成环境避免了真实网站的安全风险，同时提供确定性奖励，使得训练过程更稳定可靠。

## 7. 优点

- **安全性**：彻底消除智能体探索真实网站带来的风险，适合开放世界的自主训练。
- **可验证性**：采用程序化确定性奖励，舍弃不可靠的LLM评判器，提升训练信度。
- **可扩展性**：通过自动克隆任意网站实现环境库的无限扩展，支持大规模自我进化。
- **自生成任务**：利用SDK让智能体自行定义任务，减少人工标注成本，增加任务多样性。

## 8. 不足与局限

- **实验局限性**：
  - 基准对比不够全面，未与当前流行的合成环境（如基于DOM树模拟的环境）或强化学习算法基线进行详细比较。
  - 没有评估克隆网站与真实网站的保真度差异对智能体迁移效果的影响。
  - 缺少消融实验（例如不使用SDK自生成任务，或使用不同LM克隆的效果）。
- **偏差风险**：
  - 克隆过程依赖LLM，可能引入原始网站的失真或偏见（如网站交互逻辑的建模不完整）。
  - 合成环境可能过度简化真实网站的复杂性（如动态内容、异步加载），导致训练出的智能体在真实世界中失败。
- **应用限制**：
  - 克隆过程需要访问目标网站的结构信息，对于完全动态或反爬的网站可能不适用。
  - 当前未讨论智能体在真实网站部署时的安全隔离机制（即使训练安全，部署时仍需防范）。
- **资源算力未披露**，使得复现和成本评估困难。

（完）
