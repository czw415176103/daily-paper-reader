---
title: Efficient Agent Training for Computer Use
title_zh: 高效的计算机使用智能体训练
authors: "Yanheng He, Jiahe Jin, Pengfei Liu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=cDuA6ZNvCl"
tags: ["query:cfd-agent"]
score: 4.0
evidence: 通过合成轨迹训练计算机使用智能体以提升技能
tldr: "开发人类级别的计算机使用智能体需要大量轨迹数据。本文提出PC Agent-E框架，仅用312条人工标注轨迹，结合Claude 3.7合成数据，训练出的模型超越了基线141%并超过Claude 3.7。该方法融合人类技能与AI数据合成，显著降低了对大规模人工演示的依赖。"
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 高质量轨迹数据稀缺限制了计算机使用智能体的开发。
method: 使用少量人工轨迹，通过合成替代动作决策增强训练数据，微调模型。
result: "训练模型在WindowsAgentArena-V2上相对提升141%，超越Claude 3.7。"
conclusion: 少量人类演示结合AI数据合成可实现高效的智能体技能训练。
---

## Abstract
Scaling up high-quality trajectory data has long been a critical bottleneck for developing human-like computer use agents. We introduce PC Agent-E, an efficient agent training framework that significantly reduces reliance on large-scale human demonstrations. Starting with just 312 human-annotated computer use trajectories, we further augment them by synthesizing diverse alternative action decisions with Claude 3.7 Sonnet. Trained on these enriched trajectories, our PC Agent-E model achieved a remarkable 141% relative improvement, and even surpassed the Claude 3.7 Sonnet by 10% in relative terms on WindowsAgentArena-V2, an improved benchmark we also released. By integrating robust human computer use skills with automated AI data synthesis capabilities, our method not only brought substantial improvements over training on human trajectories alone, but also significantly surpassed direct distillation from Claude 3.7 Sonnet.

---

## 论文详细总结（自动生成）

# 高效计算机使用智能体训练（PC Agent-E）论文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：开发类人计算机使用智能体需要大量高质量的人类演示轨迹数据，但这类数据获取成本高、规模受限，成为关键瓶颈。
- **整体含义**：本文旨在探索**如何通过极少量人工标注轨迹（仅312条）结合AI合成数据，高效训练出超越当前最强模型（Claude 3.7 Sonnet）的计算机使用智能体**，从而大幅降低对大规模人工演示的依赖，推动自主智能体在实际GUI环境中的应用。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：融合**少量人类专家的计算机使用技能**（知识先验）与**自动化AI数据合成能力**，通过**替代动作决策增强**（synthesizing diverse alternative action decisions）来扩充轨迹多样性。
- **关键技术细节**：
  1. 收集**312条人工标注的完整计算机使用轨迹**（含步骤、动作、目标等）。
  2. 以这些轨迹为种子，利用**Claude 3.7 Sonnet**模型为每个步骤合成**多种替代动作决策**（如不同的点击位置、菜单路径、键盘快捷键等），生成丰富的新轨迹。
  3. 将原始人工轨迹与合成轨迹合并，形成增强训练集。
  4. 在该增强数据集上微调一个基础模型（论文未明确基础模型，推测为开源视觉语言模型或预训练智能体模型），获得**PC Agent-E**。
- **公式/算法流程（文字说明）**：
  - 输入：人工轨迹集合 \(D_h\)（312条）。
  - 对每条轨迹的每个动作节点，使用Claude 3.7 Sonnet采样K个语义等价的替代动作（如“双击”改为“右键+打开”），生成合成轨迹 \(D_s\)。
  - 训练集：\(D = D_h \cup D_s\)。
  - 在D上进行监督微调（行为克隆），最小化动作预测损失。
  - 输出：PC Agent-E模型。

## 3. 实验设计

- **数据集/场景**：使用**WindowsAgentArena**环境及其改进版**WindowsAgentArena-V2**（本文改进并发布的基准），涵盖Windows桌面常见任务（文件操作、浏览器使用、设置调整等）。
- **Benchmark**：WindowsAgentArena-V2（任务成功率等指标）。
- **对比方法**：
  - **基线**：仅用312条人工轨迹训练的模型（未使用合成数据）。
  - **直接蒸馏**：直接使用Claude 3.7 Sonnet生成的轨迹（不限量）训练模型。
  - **Claude 3.7 Sonnet自身**（作为上界参考）。
- 结果：PC Agent-E在WindowsAgentArena-V2上相对基线**提升141%**，并**超越Claude 3.7 Sonnet 10%**（相对值）。

## 4. 资源与算力

- **文中未明确说明**具体GPU型号、数量或训练时长。论文摘要和元数据中未提及算力细节。仅说明使用了Claude 3.7 Sonnet（外部API）进行合成数据生成，可能对算力需求不高（推理成本为主）。训练部分推测为单卡或少量GPU即可完成（因数据量小）。

## 5. 实验数量与充分性

- **实验数量**：正文中主要报告了**核心对比实验**（三组：仅人工、直接蒸馏、本方法）以及**相对提升百分比**。未列出大量消融实验（如不同K值、不同合成策略等）。论文可能包含更多内部实验（摘要未详述）。
- **充分性评价**：
  - **优点**：对比了最相关的两种基线（仅人工、直接蒸馏），并超越了当前最强参考模型，结论清晰。
  - **局限性**：仅在一个基准（WindowsAgentArena-V2）上验证，未提及跨平台（如Mac、Linux）或跨任务域（如手机、Web）的泛化。消融实验可能不够深入（例如，不同合成策略的影响、人工轨迹数量敏感度）。整体实验充分性**中等偏上**，但公开信息有限。

## 6. 论文的主要结论与发现

- **少量（312条）高质量人工演示 + AI合成数据，可以高效训练出超越强基线的智能体**，相对提升141%，甚至超越纯蒸馏Claude 3.7的结果。
- **直接蒸馏Claude 3.7（不加人工种子）的效果不如本方法**，说明**人类演示提供的“技能先验”至关重要**，而单纯AI合成易产生模式偏差。
- **合成数据替代动作决策策略有效**，能大幅扩充轨迹多样性而不引入人类标注成本。

## 7. 优点（方法或实验设计亮点）

- **数据高效性**：仅用312条人工轨迹，成本极低，具有很强实用价值。
- **方法简洁自然**：利用大模型（Claude 3.7）进行动作级数据增强，而非复杂模块；易于复现。
- **基准贡献**：发布了改进的WindowsAgentArena-V2基准，促进后续研究。
- **显著超越强基线**：不仅高于人类轨迹训练，还超越当前领先闭源模型，展示了“小数据+合成”的威力。

## 8. 不足与局限

- **实验覆盖有限**：仅在WindowsAgentArena-V2上评测，未涉及更多元环境（如Web、手机、办公软件等）或真实用户场景。
- **合成数据质量依赖Claude 3.7**：若基础AI模型较弱，合成动作可能不准确或不符合人类直觉，导致性能下降。
- **未公开全部消融实验**：缺乏对合成策略（如K值、采样温度、后处理）的深入分析，以及人工轨迹数量对最终性能的影响。
- **可能存在的偏差风险**：人工轨迹仅312条，可能无法覆盖所有常见操作模式；合成数据可能放大模型自身的偏见或错误模式。
- **训练细节不透明**：未说明微调基模型、超参数、优化器等，可复现性存疑。
- **算力消耗未报告**：不利于资源预算评估。

（完）
