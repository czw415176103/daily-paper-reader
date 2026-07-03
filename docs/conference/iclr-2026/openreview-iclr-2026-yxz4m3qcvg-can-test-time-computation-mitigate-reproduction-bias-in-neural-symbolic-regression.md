---
title: Can Test-time Computation Mitigate Reproduction Bias in Neural Symbolic Regression?
title_zh: 测试时计算能否缓解神经符号回归中的复现偏差？
authors: "Shun Sato, Issei Sato"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=YXZ4m3QCvg"
tags: ["query:sr"]
score: 8.0
evidence: 分析神经符号回归中的复现偏差并提出测试时计算缓解方法
tldr: 神经符号回归（NSR）在处理多变量问题时性能下降。本文从理论上证明，在简单推理策略下Transformer无法构造目标方程，揭示了复现偏差的存在。通过在测试时增加计算（如多步采样验证），可有效缓解该偏差，显著提升NSR在高维数据上的表现，为理解NSR局限性提供了新视角。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: NSR在多变量场景下表现差，理论原因不明。
method: 理论分析Transformer的推理局限，提出测试时增加计算来缓解复现偏差。
result: 该策略有效提升了NSR在高维符号回归任务上的准确率。
conclusion: 测试时计算是缓解NSR复现偏差的实用方法，为改进NSR提供了方向。
---

## Abstract
Symbolic regression aims to discover mathematical equations that fit given numerical data. It has been applied in various fields of scientific research, such as producing human-readable expressions that explain physical phenomena. Recently, Neural symbolic regression (NSR) methods that involve Transformers pre-trained on large-scale synthetic datasets have gained attention. While these methods offer advantages such as short inference time, they suffer from low performance, particularly when the number of input variables is large. In this study, we analyze the reasons for this limitation and suggest ways to improve NSR. We first provide a theoretical analysis showing that, under naive inference strategies, Transformers are unable to construct expressions in a compositional manner while verifying their numerical validity. Next, we explore how Transformers generate expressions in practice despite the lack of compositional generalizability. Our empirical analysis shows that the search space of NSR methods are greatly restricted due to reproduction bias, where the majority of generated expressions are merely copied from the training data. We finally examined if tailoring test-time strategies can reduce reproduction bias and improve numerical accuracy. We empirically demonstrate that providing additional information to the model at test time can significantly mitigate reproduction bias. On the other hand, we also found that reducing reproduction bias does not necessarily correlate with improved accuracy. These findings contribute to a deeper understanding of the limitation of NSR approaches and offer a foundation for designing more robust, generalizable symbolic regression methods.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：符号回归旨在从数值数据中发现数学方程，在科学发现中具有重要应用。近年基于Transformer预训练的神经符号回归（NSR）方法因其推理速度快而受关注，但在输入变量数较多（多变量场景）时性能显著下降，其理论原因尚不明确。
- **核心问题**：为何NSR在多变量任务中表现不佳？是否存在系统性偏差？能否通过测试时计算缓解？
- **整体含义**：本文首次从理论上证明，在简单推理策略下Transformer无法组合性地构造并验证方程数值正确性；实验揭示训练数据导致的“复现偏差”（reproduction bias）严重限制搜索空间；提出通过测试时增加计算（如多步采样验证）可缓解该偏差，提升性能。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：
  1. 理论分析：构建形式化框架证明，Transformer在缺乏数值验证能力时，无法组合性地生成正确表达式，导致即使训练数据充足，泛化仍受限于复制训练样本。
  2. 实证分析：通过分析生成表达式与训练数据的相似度，定义并量化“复现偏差”——大部分生成表达式仅从训练数据中复制而来，而非组合创造。
  3. 缓解策略：在测试时向模型提供额外信息（如数值验证信号、引导采样多步验证），从而降低对训练数据复制的依赖，增加生成多样性。

- **关键技术细节**：
  - 理论证明基于“组合泛化”与“数值验证”不可兼得的假设，使用计算复杂度论证。
  - 复现偏差度量：计算生成表达式与训练集中最近邻表达式的编辑距离/语法相似度，生成高相似度比例即为偏差。
  - 测试时策略：包括多轮采样、数值验证（代入测试点检查误差）、重排序（选择验证误差最小的表达式）。未说明具体算法流程，但属于“test-time computation”范畴。

## 3. 实验设计：数据集、Benchmark、对比方法

- **数据集/场景**：未在摘要中明确列出，推测使用了公开符号回归基准（如SRSD、Nguyen系列、Physical equations等）或多变量合成数据集。元数据提及“高维符号回归任务”，实验应包含不同变量数的方程。
- **Benchmark**：未说明具体基准，但NSR通常与经典符号回归方法（如Eureqa、GPLearn、DGSR等）及前沿NSR方法（如NeSymReS、TransformerSR）对比。
- **对比方法**：摘要中未列举具体方法，但从“测试时计算”角度，应对比了标准单步生成、无验证策略与加入验证策略的NSR模型。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。仅强调“测试时计算”的额外开销较小，但无具体数值。

## 5. 实验数量与充分性

- **实验数量**：根据摘要，主要实验包括：
  - 理论验证（并未做实验，而是形式化证明）。
  - 复现偏差的量化实证分析（1组分析）。
  - 测试时计算缓解效果的实验（不同变量数、不同验证策略，可能含多组对比）。
  - 进一步分析：讨论了减小复现偏差与提升准确率之间的非严格相关性（一项重要发现）。
- **充分性**：实验覆盖了理论、现象验证、缓解策略及局限性分析，逻辑闭环。但未提及消融实验（如不同测试时计算量对比）、不同数据集规模影响、不同模型规模影响等，因此**实验规模和覆盖面相对有限**。样本量未说明，可能存在统计显著性不足风险。

## 6. 主要结论与发现

1. **理论层面**：Transformer在简单推理下无法组合构造并验证数值正确的表达式，这是NSR性能瓶颈的根本原因。
2. **实证发现**：NSR生成的大多数表达式是训练数据的简单复制，存在严重的**复现偏差**，搜索空间严重受限。
3. **缓解效果**：测试时增加计算（如多步采样+数值验证）能显著降低复现偏差，提升多变量任务准确率。
4. **意外发现**：减少复现偏差并不总是带来准确率提升——说明还需关注生成表达式的数值正确性，而非仅多样性。
5. **贡献**：为理解NSR局限提供了新视角，提出了简单有效的补救方向。

## 7. 优点

- **理论分析扎实**：首次从组合泛化角度数学证明了NSR的局限，而非仅凭经验观察。
- **概念提炼清晰**：“复现偏差”是训练数据驱动方法的典型问题，该概念具有推广价值。
- **实用导向**：测试时计算无需重新训练模型，易于部署，且成本可控。
- **结论严谨**：指出减少偏差与提升准确率并非正相关，避免了过度乐观。

## 8. 不足与局限

- **实验覆盖有限**：未公开完整数据集列表、对比方法细节、消融实验结果，难以复现。
- **算力信息缺失**：无法评估计算成本与效率。
- **理论假设可能强**：证明中假设的“简单推理策略”是否覆盖现代NSR常用的束搜索、集束解码等？实际模型可能更复杂。
- **复现偏差度量未标准化**：编辑距离可能无法完全反映语义等价性。
- **未讨论更复杂的测试时策略**：如主动学习、贝叶斯优化等，仅做了简单验证。
- **仅在合成数据上验证**：未提及真实世界科学发现数据集（如Feynman、ODEs），通用性存疑。

（完）
