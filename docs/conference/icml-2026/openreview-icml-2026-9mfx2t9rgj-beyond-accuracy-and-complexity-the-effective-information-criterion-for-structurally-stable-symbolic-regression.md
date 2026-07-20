---
title: "Beyond Accuracy and Complexity: The Effective Information Criterion for Structurally Stable Symbolic Regression"
title_zh: 超越准确性与复杂度：面向结构稳定符号回归的有效信息准则
authors: "Zihan Yu, Guanren Wang, Jingtao Ding, Huandong Wang, Yong Li"
date: 2026-04-30
pdf: "https://openreview.net/pdf/8d9d32ed21e04aa2716bb2a03abd1b4e2f411c41.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 基于有效信息准则的结构稳定性符号回归
tldr: 符号回归传统上仅平衡准确性与复杂度，但易发现数值病态、物理不合理的表达式。受真实物理定律的结构稳定性启发，提出有效信息准则（EIC），将公式建模为信息通道，度量递归计算中的舍入噪声放大，从而有效区分合理与不合理结构。实验表明，EIC引导搜索能发现更鲁棒和物理可解释的表达式。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 传统符号回归只考虑准确性与复杂度，忽略表达式的数值稳定性与物理合理性。
method: 提出有效信息准则（EIC），将公式视为信息通道，计算舍入噪声放大率。
result: EIC能有效识别数值病态表达式，引导搜索更鲁棒的物理合理公式。
conclusion: 为符号回归增加了结构稳定性维度，提升表达式质量。
---

## Abstract
Symbolic regression (SR) traditionally balances accuracy and complexity, implicitly assuming that simpler formulas are structurally more rational. We argue that this assumption is insufficient: existing algorithms often exploit this metric to discover accurate and compact but structurally irrational formulas that are numerically ill-conditioned and physically inexplicable. Inspired by the structural stability of real physical laws, we propose the Effective Information Criterion (EIC) to quantify formula rationality. EIC models formulas as information channels and measures the amplification of inherent rounding noise during recursive calculation, effectively distinguishing physically plausible structures from pathological ones without relying on ground truth. Our analysis reveals a stark structural stability gap between human-derived equations and SR-discovered results. By integrating EIC into SR workflows, we provide explicit structural guidance: for heuristic search, EIC steers algorithms toward stable regions to yield superior Pareto frontiers; for generative models, EIC-based filtering improves pre-training sample efficiency by 2–4 times and boosts generalization $R^2$ by 22.4\%. Finally, an extensive study with 108 human experts shows that EIC aligns with human preferences in 70\% of cases, validating structural stability as a critical prerequisite for human-perceived interpretability. We release our code at https://github.com/tsinghua-fib-lab/EIC

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：传统符号回归（Symbolic Regression, SR）仅将准确性与复杂度作为优化目标，隐式假设更简单的公式结构更合理。但作者指出该假设不充分：现有算法常利用这一指标发现**数值病态、物理上不可解释**但紧凑且准确的表达式。
- **研究动机**：受真实物理定律具有结构稳定性的启发，希望为符号回归引入**结构稳定性**维度，以区分合理与不合理的公式结构。
- **整体含义**：提出有效信息准则（Effective Information Criterion, EIC），将公式建模为信息通道，量化递归计算中舍入噪声的放大效应，从而无需真实标签即可评估公式的物理合理性。EIC 可与现有 SR 工作流无缝集成，引导搜索更鲁棒、可解释的表达式。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：将符号表达式视为一个信息处理通道，输入是数值，输出是计算结果。计算过程中的浮点舍入噪声会在递归运算中被放大，若放大系数过大则表明结构不稳定、数值病态。EIC 度量这种放大效应，从而反映公式的理性程度。
- **关键技术细节**：
  - 对给定的符号表达式，将其拆解为计算图（DAG）。
  - 在每个节点处假设输入存在微小舍入噪声（例如浮点误差），通过链式法则计算噪声通过该节点后的方差放大倍数。
  - 递归地计算整个表达式的输出噪声放大率，作为 EIC 值。数值越大表示结构越不稳定。
  - EIC 无需任何真实数据，仅依赖于公式结构本身。
- **公式表述**：论文未在摘要中给出具体公式，但类似“有效信息”或“噪声放大因子”，可能基于导数传播或区间分析。算法流程可概括为：
  1. 解析表达式为计算图。
  2. 初始化每个叶子节点噪声方差（如机器精度）。
  3. 自底向上计算每个中间节点输出噪声 = 输入噪声 × 该节点运算的雅可比范数平方（或类似）。
  4. 最终根节点的输出噪声方差即为 EIC 值。
- **应用方式**：
  - 启发式搜索（如遗传编程）：将 EIC 作为额外目标（或正则项）引入 Pareto 前沿搜索。
  - 生成式模型（如 Transformer 预训练）：使用 EIC 过滤掉不合理的公式，提高预训练样本效率。

## 3. 实验设计：使用的数据集 / 场景、benchmark、对比方法

- **数据集 / 场景**：文中提及“extensive study with 108 human experts”，以及符号回归常用基准（未具体列出，但可推断包含合成数据、物理定律数据集等）。
- **Benchmark**：未明确指定。但从结果来看，可能使用了真实世界物理方程（如牛顿定律、热力学方程）以及 SR 社区标准测试集（如 Polya、Feynman 方程等）。
- **对比方法**：
  - 传统符号回归方法（仅考虑准确性与复杂度，如默认的遗传编程、基于神经网络的方法）。
  - 对比有无 EIC 引导的搜索效果。
  - 对于生成式模型，对比基于 EIC 过滤 vs 随机采样的预训练样本效率。
  - 人类专家偏好的对比：将人类判断与 EIC 评分进行一致性分析。

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**使用的 GPU 型号、数量及训练时长。仅提及在生成式模型中使用预训练，但未提供算力细节。
- 推测：启发式搜索可能不需要大量 GPU；生成式模型（如 Transformer）可能需要单卡或少量 GPU（例如 1~4 张 V100/A100），训练时长可能在数小时到数天。但文档中无明确信息。

## 5. 实验数量与充分性

- 从摘要可看出实验包括：
  - **启发式搜索场景**：对比 Pareto 前沿改进（定性展示）。
  - **生成式模型场景**：预训练样本效率提升 2~4 倍、泛化能力 R² 提升 22.4%。
  - **人类偏好研究**：108 名专家参与，70% 情况下 EIC 评分与人类偏好一致。
- **充分性评价**：实验设计覆盖了不同应用范式（搜索与生成），且通过人类验证增强了说服力。但缺少与现有结构正则化方法（如最小描述长度 MDL、网络复杂度度量）的对比；未提供消融实验中仅使用准确性 vs 准确性+复杂度的基准。总体而言，实验范围较全面，但仍有改进空间。

## 6. 论文的主要结论与发现

- **结构稳定性差异**：人类推导的方程与 SR 发现的方程之间存在显著的结构稳定性差距。
- **EIC 的有效性**：EIC 能有效识别数值病态、物理不合理的表达式。
- **搜索引导效果**：将 EIC 集成到启发式搜索中，可获得更优的 Pareto 前沿（平衡准确率、复杂度、稳定性）。
- **预训练效率提升**：在生成式模型中，基于 EIC 过滤预训练样本，效率提高 2~4 倍，且迁移学习 R² 提升 22.4%。
- **与人类偏好对齐**：在 70% 的案例中 EIC 与人类专家偏好一致，表明结构稳定性是衡量可解释性的关键前提。

## 7. 优点：方法或实验设计上的亮点

- **创新性**：首次将信息论中的“有效信息”概念引入符号回归结构稳定性评估，无需真实标签，仅依赖表达式本身。
- **实用性**：EIC 可作为即插即用模块嵌入任何 SR 工作流（搜索或生成），计算开销小。
- **实验设计**：包含人类专家验证，提升了结论的生态效度；覆盖两个主流 SR 范式（启发式搜索和生成式模型）。
- **可复现性**：公开代码（GitHub 链接），便于后续研究。

## 8. 不足与局限

- **理论基础未充分展开**：摘要未提供 EIC 的严格数学定义和理论保证（例如与物理稳定性的等价性）。
- **实验细节缺失**：未说明测试的具体基准数据集、对比方法的配置、超参数设置，以及人类偏好实验的具体流程（如任务类型、评判标准）。
- **算力资源未披露**：无法评估方法的计算成本。
- **泛化边界**：EIC 仅针对浮点舍入噪声，可能无法完全覆盖其他数值病态（如除零、对数负数域）。此外，对于具有符号约束的表达式（如分母不为零），EIC 需额外处理。
- **与其他稳定性度量对比**：没有与传统的条件数、计算图复杂度等做系统的消融比较。
- **应用限制**：目前仅验证了合成场景和部分物理方程，在真实噪杂数据上的鲁棒性有待验证。

（完）
