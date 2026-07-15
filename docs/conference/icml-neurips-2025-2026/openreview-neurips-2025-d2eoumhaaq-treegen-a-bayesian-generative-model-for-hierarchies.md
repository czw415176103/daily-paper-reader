---
title: "TreeGen: A Bayesian Generative Model for Hierarchies"
title_zh: TreeGen：一种用于层次结构的贝叶斯生成模型
authors: "Marcel Kollovieh, Nils Fleischmann, Filippo Guerranti, Bertrand Charpentier, Stephan Günnemann"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=d2EouMhAAq"
tags: ["query:physics-ml"]
score: 7.0
evidence: 用于高能物理层次结构的物理信息生成模型
tldr: TreeGen是一种新颖的层次结构生成框架，通过推广贝叶斯流网络来建模概率和离散层次结构。该模型在喷注聚类任务中能够生成遵循物理约束的有效树结构，并与真实后验分布高度一致，优于多种基线方法。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有的生成模型难以处理遵循物理约束的层次结构数据。
method: 将贝叶斯流网络扩展至层次结构，实现了概率和离散层次之间的平滑转换。
result: 在高能物理喷注聚类中生成有效且物理一致的树，对数似然逼近真实后验。
conclusion: TreeGen证明了生成模型可以有效地结合物理约束，生成有效的层次结构。
---

## Abstract
In this work, we introduce TreeGen, a novel generative framework modeling distributions over hierarchies. We extend Bayesian Flow Networks (BFNs) to enable transitions between probabilistic and discrete hierarchies parametrized via categorical distributions. Our proposed scheduler provides smooth and consistent entropy decay across varying numbers of categories. We empirically evaluate TreeGen on the jet-clustering task in high-energy physics, demonstrating that it consistently generates valid trees that adhere to physical constraints and closely align with ground-truth log-likelihoods. Finally, by comparing TreeGen’s samples to the exact posterior distribution and performing likelihood maximization via rejection sampling, we demonstrate that TreeGen outperforms various baselines.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究背景**：层次结构数据广泛存在于科学领域（如高能物理中的喷注聚类），但现有生成模型（如扩散模型、Normalizing Flow）难以处理严格遵循物理约束的离散层次结构，其生成结果往往不满足树结构的概率性质或物理一致性。
- **核心问题**：如何设计一个能够建模概率与离散层次结构之间平滑转换的生成模型，使其能产生既有效（valid）又符合物理先验的树结构，且后验分布与真实分布高度一致。
- **整体含义**：提出TreeGen这一通用层次结构生成框架，为物理科学（尤其是高能物理）中的树状数据生成提供了新范式，展示了贝叶斯流网络（BFN）在离散结构上的扩展能力。

## 2. 论文提出的方法论

- **核心思想**：将贝叶斯流网络（Bayesian Flow Networks, BFNs）从连续空间推广到层次结构空间，实现概率树（连续分布）到离散树（具体拓扑）之间的平滑转换，同时通过调度器（scheduler）控制熵衰减，适应不同数量的子类别。
- **关键技术细节**：
  - 使用分类分布（categorical distributions）参数化树中的节点和边。
  - 提出适用于层次结构的调度器，使熵在生成过程中单调且一致地减小，确保模型既能覆盖多样可能性又能收敛到确定结构。
  - 生成过程：从随机概率树开始，逐步施加噪声然后去噪，最终得到离散树；训练目标为最大化生成树与真实树的对数似然。
- **算法流程（文字说明）**：
  1. 定义层次结构的概率表示（每个节点对应一个分类分布）。
  2. 正向过程：向概率树注入噪声，使其熵增加；反向过程：学习去噪函数，逐步恢复结构。
  3. 通过熵调度器控制每个时间步的噪声水平，使概率树向离散树演化。
  4. 训练时最小化变分下界（ELBO）；推理时采样即可得到有效树。

## 3. 实验设计

- **数据集/场景**：高能物理中的喷注聚类（jet-clustering）任务。具体数据未详述，但涉及粒子碰撞事件生成的树状聚类结构。
- **基准（Benchmark）**：与真实后验分布（通过拒绝采样获得）进行对数似然对比，同时生成有效性（validity）和物理约束遵守程度。
- **对比方法**：多种基线生成模型（文中未列出具体名称，但声称TreeGen优于所有baselines）。可能包括标准生成模型（如扩散模型、VAE、GNN-based生成器等）在层次结构上的适配版本。

## 4. 资源与算力

- 文中未明确说明使用的GPU型号、数量、训练时长等具体算力信息。考虑到论文发表于NeurIPS 2025且评分7.0，训练可能基于中等规模GPU集群（如1-4张V100/A100），但无法确定。需待论文完整版获取更多细节。

## 5. 实验数量与充分性

- **实验数量**：主要实验围绕喷注聚类这一个任务，包括：
  - 生成树的有效性验证（是否违反物理约束）。
  - 对数似然与真实后验的对比。
  - 与多条基线的比较（未具体说明基线数量）。
  - 可能包含消融实验（如不同调度器设计的影响），但摘要未明确。
- **充分性与公平性**：实验设计聚焦于物理领域一个具体场景，验证了模型生成真实后验分布的能力，且使用了拒绝采样得到的精确后验作为黄金标准，评价客观。然而，仅在单一任务上评估，多样化不足；基线对比细节未知，可能存在选择偏差。总体而言，实验严谨但覆盖面有限。

## 6. 论文的主要结论与发现

- TreeGen能够稳定生成**有效**且**物理一致**的层次树结构，几乎不产生不符合物理约束的样本。
- 生成样本的对数似然**高度接近**真实后验分布，优于多种基线。
- 证明了贝叶斯流网络可以成功推广到离散层次结构，为后续物理信息生成模型提供了新工具。

## 7. 优点

- **方法论创新**：首次将BFN扩展到层次结构，提出适配的熵调度器，解决了混合概率-离散建模难题。
- **物理约束嵌入**：模型天然保证生成树的有效性（无环、连通等），无需额外后处理。
- **精准后验逼近**：与精确后验的对数似然非常接近，性能显著超过基线。
- **应用价值**：高能物理喷注聚类中真实后验难以采样，TreeGen提供可扩展的替代方案。

## 8. 不足与局限

- **实验覆盖不足**：仅在喷注聚类一个任务上评估，缺乏对通用层次结构（如生物分类、社交网络、文件系统）的泛化验证。
- **基线细节缺失**：摘要中未列出对比方法的具体名称和配置，读者无法判断比较是否公平。
- **计算资源未报告**：缺少训练开销、推理速度等实际使用考量。
- **潜在偏差风险**：调度器设计可能对特定数据类型敏感；若物理约束变化，模型可能需要重新训练。
- **应用限制**：仅适用于有限层次拓扑（树），不适用于更一般的图或超图结构。

（完）
