---
title: Towards Adaptive GUI Agents with Memory-Driven Knowledge Evolution
title_zh: 面向自适应GUI智能体的记忆驱动知识进化
authors: "Libo Sun, Jiwen Zhang, zhongyu wei"
date: 2025-09-10
pdf: "https://openreview.net/pdf?id=ZbPjx41RYz"
tags: ["query:cfd-agent"]
score: 6.0
evidence: 记忆驱动的知识进化用于自适应GUI智能体，提升智能体技能
tldr: 移动App智能体面临UI变化和任务逻辑漂移的挑战。本文提出记忆驱动的自适应智能体框架，包含静态记忆和程序记忆双层结构。当UI元素失效或流程改变时，智能体利用记忆进行知识演化，持续适应。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 移动应用更新导致智能体UI识别失败和任务逻辑漂移。
method: 提出双层记忆结构（静态记忆和程序记忆），使智能体能自适应演化。
result: 在动态移动环境中保持智能体可靠性和长期可用性。
conclusion: 记忆驱动的知识进化是提升智能体适应性的有效方法。
---

## Abstract
Mobile App Agents powered by large foundation models represent a transformative approach to human-computer interaction, enabling autonomous task execution within dynamic mobile applications. However, the volatile nature of mobile ecosystems characterized by frequent application updates poses challenges to agent reliability and long-term viability. We identify two critical problems: UI element identification failure when visual or structural changes occur, and task logic drift when fundamental workflows are altered. To address these challenges, we propose \textbf{\modelname}, a \textbf{M}emory-driven \textbf{A}daptive a\textbf{GENT} framework, equipped with a novel dual-level memory consisting of stationary memory and procedural memory. The stationary memory maintains rich multimodal representations of UI elements, enabling robust action grounding despite interface modifications, while the procedural memory captures and adapts structured task workflows to handle logical changes in operations. This framework effectively bridges the update gap that has limited the practical deployment of mobile agents. 
Comprehensive experiments demonstrate that \modelname achieves robust generalization across various in-domain scenarios and strong adaptability to novel task domains.

---

## 论文详细总结（自动生成）

# 论文核心总结：面向自适应GUI智能体的记忆驱动知识进化

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：基于大模型的移动App智能体在动态移动环境中面临两大挑战：① UI元素识别失败（因视觉或结构变化）；② 任务逻辑漂移（因基本工作流被改变）。应用频繁更新导致智能体的可靠性和长期可用性受限。
- **整体含义**：提出一种**记忆驱动的自适应智能体框架**，通过双层记忆结构使智能体能够持续演化知识，从而桥接应用更新带来的“更新间隙”，提升智能体在动态环境中的鲁棒性和适应性。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：利用记忆驱动知识进化，让智能体在面对UI变化和逻辑变化时自适应调整行为。
- **关键技术细节**：
  - **双层记忆结构**：
    - **静态记忆（Stationary Memory）**：维护UI元素的多模态表示（视觉+结构），即使界面修改也能实现稳健的动作定位。
    - **程序记忆（Procedural Memory）**：捕获并适应结构化的任务工作流，处理操作逻辑的变化。
  - **工作机制**（根据摘要推断）：当检测到UI元素失效或流程改变时，智能体检索静态记忆重新定位元素，或从程序记忆调整任务步骤，实现知识演化。
- **算法流程**（文字说明）：① 初始化双层记忆（从原始任务数据构建）；② 运行时，智能体接收当前UI状态，若识别失败则从静态记忆匹配相似元素；③ 若任务执行出错，则调用程序记忆重新规划工作流；④ 通过经验反馈更新记忆，持续进化。

## 3. 实验设计：使用了哪些数据集/场景、benchmark、对比方法

- **数据集/场景**：论文未明确说明具体数据集名称，仅提及“多种域内场景”和“新型任务域”。推测可能基于常见移动App（如手机设置、办公应用等）的模拟环境或真实应用。
- **Benchmark**：未给出标准基准名称，可能为自定义的移动智能体评测集（如基于App动作轨迹的评估）。
- **对比方法**：未提及具体基线模型名称。从元数据“query:cfd-agent”推测可能对比了无记忆的基线或纯大模型驱动方法。
- **总结**：实验细节不足，难以评估对比公平性。

## 4. 资源与算力

- **资源与算力**：论文中**未明确说明**使用的GPU型号、数量或训练时长。仅提到“基于大模型”，未给出训练/推理的具体算力要求。

## 5. 实验数量与充分性

- **实验数量**：仅用“comprehensive experiments（综合实验）”概括，未列具体组数。通常此类工作会包含域内泛化实验、域外适应实验以及消融研究，但本文未给出定量结果。
- **充分性判断**：由于缺乏具体数字和对比实验细节，无法判断实验的充分性和公平性。仅从摘要所述“robust generalization”和“strong adaptability”看，不足以支撑严谨结论。

## 6. 论文的主要结论与发现

- **主要结论**：记忆驱动的知识进化（CFD-Agent）能够有效提升GUI智能体在动态移动环境中的适应性和鲁棒性。
- **关键发现**：
  - 静态记忆有助于抵抗UI更新导致的元素识别失败。
  - 程序记忆能处理任务逻辑漂移，保持长期可用性。
  - 框架在域内场景和新任务域均表现出强泛化能力。

## 7. 优点：方法或实验设计上的亮点

- **方法亮点**：
  - 提出**双层记忆结构**，将UI元素表示与任务工作流解耦，分别应对两种不同类型的漂移。
  - 实现了“知识进化”机制，使智能体具备持续适应能力，而非一次性训练。
- **动机清晰**：精准识别了移动智能体实际部署中的两大核心障碍（UI变化和逻辑变化），具有强实用价值。

## 8. 不足与局限

- **实验覆盖不足**：未提供具体数据集、指标、对比方法、消融实验细节，无法验证结果的可复现性和统计显著性。
- **偏差风险**：仅给出定性结论，缺少定量结果（如成功率、鲁棒性提升百分比），存在选择性报告的风险。
- **应用限制**：
  - 未讨论记忆存储的规模与更新效率，大规模应用时可能有性能瓶颈。
  - 未说明对全新未见UI的风格迁移能力，仅提到“novel task domains”但未明确是否跨App。
- **算力信息缺失**：无法评估部署成本。

（完）
