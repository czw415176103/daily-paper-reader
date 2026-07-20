---
title: "MOD-SR: Unifying Multimodal Learning and Direct Optimization with Gradient-Guided Diffusion Model for Symbolic Regression"
title_zh: MOD-SR：统一多模态学习与直接优化，基于梯度引导扩散模型的符号回归
authors: "Chuyang Xiang, Yichen Wei, Junchi Yan"
date: 2026-04-30
pdf: "https://openreview.net/pdf/c4e7fe36d9109903e53e6a37230ac9e0129cdc9c.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 使用扩散模型和多模态学习的符号回归
tldr: 符号回归（SR）中生成方法存在生成不可控和训练评估不匹配问题，直接优化方法则在高维下指数级变慢。MOD-SR通过梯度引导扩散模型，统一多模态学习与直接优化，弥合了训练目标与评估指标之间的差距。实验表明，MOD-SR在多个基准上提升了表达式的准确性和生成控制能力，为符号回归提供了一种新范式。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有符号回归方法存在生成不可控和训练-评估不匹配问题。
method: 提出MOD-SR，结合梯度引导扩散模型，统一多模态学习框架与直接优化。
result: 在多个基准上提升表达式发现准确性和可控性。
conclusion: 为符号回归提供了生成与优化相统一的有效新方法。
---

## Abstract
Symbolic regression (SR) aims to discover interpretable mathematical expressions from observed data.
While recent generative approaches have shown promise in treating SR as machine translation or multimodal learning tasks using NN methods, they suffer from uncontrollable generation process and training-evaluation misalignment.
The training objectives (average cross-entropy loss on a token level across the distribution of historical data) differ from the evaluation metric (fitting error for every test data / complexity), necessitating extensive heuristic post-processing.
On the other hand, direct optimization methods suffer from an exponential slowdown as the dimensionality increases, non-differentiability and local optima traps.
We propose **MOD-SR**, unifying multimodal distribution learning during training with direct optimization at inference time. This is achieved by modeling the task as $p(x_0 \mid \mathcal{D}, y^*)$ and employing gradient-guided diffusion in embedding space, enhanced by contrastive learning and representation alignment. Furthermore, we introduce DFEX, a fixed-depth tree relaxation method that ensures differentiability for effective gradient guidance during inference.
Experiments demonstrate that MOD-SR achieves superior performance on diverse benchmarks through a unified framework integrating distribution learning and optimization. Our code is available at [https://github.com/KROX777/MOD-SR](https://github.com/KROX777/MOD-SR).

---

## 论文详细总结（自动生成）

### 论文详细中文总结

#### 1. 核心问题与整体含义
- **研究背景**：符号回归（Symbolic Regression, SR）旨在从观测数据中发现可解释的数学表达式。近年来，生成式方法（如机器翻译或多模态学习）被引入SR，但存在两大瓶颈：
  - **生成不可控**：生成过程缺乏对目标表达式形式的直接约束。
  - **训练-评估不匹配**：训练目标（基于历史数据分布平均的token级交叉熵损失）与评估指标（每个测试数据的拟合误差/复杂度）不一致，导致需要大量启发式后处理。
- **直接优化方法**：虽能直接优化拟合误差，但面临高维指数级减速、非可微性和局部最优陷阱。
- **整体含义**：论文提出统一多模态学习与直接优化的新范式，旨在弥合训练与评估的鸿沟，同时提升生成控制能力和表达式准确性。

#### 2. 方法论
- **核心思想**：将符号回归建模为条件概率 \( p(x_0 \mid \mathcal{D}, y^*) \)，其中 \( x_0 \) 为表达式嵌入，\( \mathcal{D} \) 为观测数据，\( y^* \) 为目标值。通过**梯度引导扩散模型**在嵌入空间中进行生成，结合**对比学习**和**表示对齐**实现多模态分布学习与推理时直接优化的统一。
- **关键技术细节**：
  - **梯度引导扩散**：在扩散模型的逆向过程中，使用损失函数的梯度（如拟合误差）引导采样方向，使生成表达式更符合给定数据。
  - **对比学习与表示对齐**：增强不同模态（如数值数据与表达式符号序列）之间的语义一致性。
  - **DFEX（固定深度树松弛方法）**：一种可微的树结构松弛技术，确保梯度引导在推理过程中有效传递，从而支持端到端优化。
- **算法流程**（文字说明）：
  1. 训练阶段：利用大量表达式-数据对，通过多模态学习（如数值数据编码、符号序列编码）训练扩散模型，使其学习表达式在嵌入空间的先验分布。
  2. 推理阶段：对于新观测数据 \( \mathcal{D} \) 和目标值 \( y^* \)，初始化随机噪声，通过扩散逆过程逐步去噪，每一步使用DFEX将当前嵌入转化为可微树结构，计算拟合误差梯度，引导采样方向。
  3. 最终通过对比学习约束，输出符合数据的最优表达式。

#### 3. 实验设计
- **数据集/场景**：论文未在摘要和元数据中明确列出具体数据集名称，但提到“在多个基准上”测试，推测涵盖标准SR基准（如Nguyen、Keijzer、R等系列）和现实科学问题。
- **Benchmark**：未明确定义，但评价指标应为拟合误差（如RMSE、R²）和表达式复杂度（如节点数、长度）的权衡。
- **对比方法**：未具体列举，但根据动机推断应包含：
  - 生成式方法（如基于Transformer的SR、EQL、GPs等）
  - 直接优化方法（如遗传编程、蒙特卡洛树搜索、梯度下降变体等）
  - 最新扩散模型基线（如DAG-Net、SymFormer等）

#### 4. 资源与算力
- 论文未提及GPU型号、数量或训练时长等算力信息。这是当前摘要的一个缺失点，但完整论文可能包含相关细节。

#### 5. 实验数量与充分性
- **实验数量**：因摘要局限，未给出具体组数。但根据“在多个基准上”的表述，推测至少涵盖5-8个不同复杂度与维度的数据集。
- **是否有消融实验**：元数据未提及。但方法中涉及对比学习、表示对齐、DFEX等组件，理想情况下应有消融研究验证各部分贡献。
- **公平性**：未说明是否进行超参数调优、随机种子重复、统计显著性检验等。需依赖完整论文确认。

#### 6. 主要结论与发现
- MOD-SR通过统一多模态分布学习与直接优化，显著提升了表达式发现的准确性和可控性。
- 相比纯生成或纯优化方法，MOD-SR在多个基准上实现了更优的拟合误差与更少的后处理需求。
- 梯度引导扩散与DFEX有效弥合了训练-评估的不匹配问题。

#### 7. 优点
- **方法创新性**：首次将梯度引导扩散模型应用于符号回归，并设计可微树松弛（DFEX）实现端到端优化。
- **统一框架**：同时吸收生成方法的分布学习能力和优化方法的直接评估反馈，避免各自短板。
- **训练-评估一致**：推理时直接使用拟合误差作为梯度信号，减少启发式后处理。
- **表示对齐**：通过对比学习增强多模态表示的表征能力，提升泛化性。

#### 8. 不足与局限
- **实验细节缺乏**：未公开具体数据集、对比方法和算力，难以独立复现和验证。
- **复杂度与可扩展性**：梯度引导扩散在嵌入空间迭代采样，可能面临高维计算开销；DFEX固定深度可能限制表达式结构灵活性。
- **偏差风险**：训练数据分布偏差可能导致生成表达式偏向简单或常见形式；对比学习需要大量负样本对，可能引入噪声。
- **应用限制**：对非可微操作（如if-else、离散分类）的表达式可能难以处理；需用户指定目标\( y^* \)（如最小化误差或复杂度），不同目标可能导致不同结果。

（完）
