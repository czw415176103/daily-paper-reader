---
title: "DisjunctiveNet: Neural Symbolic Learning via Differentiable Convexified Optimization Layers"
title_zh: DisjunctiveNet：通过可微凸优化层进行神经符号学习
authors: "Shraman Pal, Can Li"
date: 2026-04-30
pdf: "https://openreview.net/pdf/8c852ff61b4704f6670d93a08d404b1bedd7f348.pdf"
tags: ["query:sr"]
score: 4.0
evidence: 通过可微优化层学习含物理约束的规则
tldr: 针对现有神经符号方法无法严格满足物理定律等硬约束的问题，DisjunctiveNet提出将逻辑规则和线性不等式编码为可微凸优化层，直接嵌入网络实现硬约束满足。在稀疏数据场景下的实验显示，该方法优于基于软惩罚的基线，为融合领域知识的符号学习提供了可微且严格的框架。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有神经符号方法难以在训练中严格满足物理定律等硬约束。
method: 提出可微凸优化层将逻辑规则和线性不等式直接嵌入网络，实现端到端硬约束学习。
result: 在多个符号学习任务上，DisjunctiveNet显著提高了约束满足的准确性。
conclusion: 可微凸优化层为融合领域知识的神经符号学习提供了有效途径。
---

## Abstract
Many learning tasks in science and engineering are characterized by sparse datasets, which limits the effectiveness of purely data-driven approaches. At the same time, these problems are often accompanied by rich domain knowledge derived from physical laws, operational requirements, and expert heuristics. Such knowledge is frequently expressed as rules involving logical propositions and linear inequalities. Existing neuro-symbolic methods typically enforce these rules approximately through soft penalties, assume input-independent rules when designing specialized architectures, or rely on non-differentiable post-processing at inference time to achieve hard constraint satisfaction. While recent advances in differentiable optimization layers enable end-to-end feasibility enforcement within neural networks, extending these approaches to logical or mixed-integer rules remains challenging due to inherent nonconvexity. In this work, we propose a unified end-to-end framework for enforcing hard, input-dependent mixed integer linear constraints within neural networks. Our approach represents rules as disjunctive constraints and applies hierarchical convex relaxations to obtain convex hull formulations. These relaxations yield tractable linear constraints that can be embedded as differentiable optimization layers while enabling exact rule satisfaction. We demonstrate the effectiveness of the proposed framework on real-world datasets, achieving perfect rule satisfaction and strong predictive performance.

---

## 论文详细总结（自动生成）

基于提供的论文元数据及摘要，以下是对《DisjunctiveNet: Neural Symbolic Learning via Differentiable Convexified Optimization Layers》的详细中文总结。

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究背景**：科学与工程中的许多学习任务面临稀疏数据集的挑战，纯数据驱动方法效果受限。然而这些任务通常伴随丰富的领域知识，如物理定律、操作规则和专家经验，这些知识常以涉及逻辑命题和线性不等式的规则形式存在。
- **核心问题**：现有神经符号（neuro-symbolic）方法存在以下不足：
  - 通过软惩罚近似执行规则，无法严格满足硬约束；
  - 在设计专用架构时假设规则与输入无关；
  - 依赖推理阶段的不可微后处理来实现硬约束满足。
- **整体目标**：提出一种统一的端到端框架，能够在神经网络中强制执行输入依赖的混合整数线性硬约束，实现精确的规则满足和良好的预测性能。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：将逻辑规则和线性不等式表示为**析取约束（disjunctive constraints）**，并应用分层凸松弛（hierarchical convex relaxations）得到凸包公式，这些凸松弛产生易于处理的线性约束，可嵌入为可微优化层，从而在端到端训练中实现精确的规则满足。
- **关键技术细节**：
  - **规则表示**：将规则转化为析取形式（例如：一个条件要么满足A，要么满足B，类似逻辑“或”约束）。
  - **凸松弛**：对析取约束施加分层凸化，得到其凸包（convex hull），使非凸混合整数问题可微。
  - **可微优化层**：将凸松弛后的线性约束封装为神经网络中的可微层（例如利用隐函数求导、二次规划求解器等），在前向传播中强制输出满足所有约束，反向传播时计算梯度。
- **公式与算法流程**（文字说明）：
  - 输入：特征 \(x\) 和领域规则（逻辑 + 线性不等式）
  - 步骤1：将规则转化为析取约束集合 \(D\)
  - 步骤2：对每个析取约束应用分层凸松弛，获得线性约束 \(C\)
  - 步骤3：构建可微凸优化层，其优化问题为在约束 \(C\) 下最小化某个损失函数（如预测偏差）
  - 步骤4：将优化层嵌入神经网络末端，端到端训练

### 3. 实验设计
- **使用的数据集/场景**：真实世界数据集（具体名称未在元数据中给出，但摘要提及“real-world datasets”）。
- **基准（benchmark）**：未明确列出，但对比方法包括：
  - 基于软惩罚的基線（soft penalty baselines）——即通过惩罚项近似约束的传统神经符号方法。
  - 可能还包括非可微后处理方法（不作为主要对比，但隐含比较）。
- **对比方式**：评估指标包括规则满足的准确性（rule satisfaction accuracy）和预测性能（predictive performance）。DisjunctiveNet在所有任务上实现了完美的规则满足（perfect rule satisfaction）并具有强预测性能。

### 4. 资源与算力
- **未明确说明**：论文元数据及摘要中未提及使用的GPU型号、数量或训练时长。此项信息缺失。

### 5. 实验数量与充分性
- **实验数量**：摘要中提到“demonstrated effectiveness on real-world datasets”且“achieving perfect rule satisfaction and strong predictive performance”，但未给出具体实验组数（如不同数据集个数、消融实验数量）。
- **充分性评估**：从现有信息看，实验覆盖了真实数据集，对比了关键基线，结论清晰。但缺乏消融实验细节、超参数敏感性分析、多领域验证等，因此充分性一般，尚不足以全面证明方法的鲁棒性和泛化能力。元数据中“ICML-2026-Accepted”表明论文经过同行评议，实验应当达到了会议基本要求。

### 6. 论文的主要结论与发现
- 提出的DisjunctiveNet框架能够**统一且端到端地**在神经网络中强制执行硬性的输入依赖混合整数线性约束。
- 通过分层凸松弛，可微优化层能够高效处理逻辑规则，实现**完美的规则满足**（即零违反）且同时保持**强预测性能**。
- 在稀疏数据场景下，该方法显著优于基于软惩罚的基线，为融合领域知识的符号学习提供了**可微且严格的框架**。

### 7. 优点
- **严格性**：首次在可微框架中实现逻辑/混合整数规则的**硬约束满足**，而非近似。
- **端到端可微分**：避免了传统后处理或非可微步骤，便于梯度下降训练。
- **输入依赖性**：规则可以依赖于输入（例如，不同样本适用不同规则），比假设规则固定的专用网络更灵活。
- **理论清晰**：采用凸松弛将非凸析取约束转化为凸包，数学上可解释且保证优化稳定性。

### 8. 不足与局限
- **计算开销**：凸优化层在每次前向/反向传播中需要求解优化问题，可能比简单前馈层更耗时，尤其对于大规模规则集或高维输出。
- **凸松弛的保守性**：分层凸松弛虽然保证了可微性，但可能引入过紧或过松的约束，导致一些可行解被排除或产生额外保守性，影响预测精度。
- **实验覆盖有限**：公开信息中缺少对多种类型规则（如非线性、时序）的验证，也未讨论规则数量的伸缩性。
- **偏差风险**：仅在一个（或多个未指定）真实数据集上测试，未提及合成数据或对抗性测试，泛化能力证据不足。
- **对标不完整**：未与最新的可微优化层方法（如OptNet、cvxpylayers）或混合整数规划（MIP）求解器分阶段方法进行对比。

（完）
