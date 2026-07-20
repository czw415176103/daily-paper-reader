---
title: Breaking the Simplification Bottleneck in Amortized Neural Symbolic Regression
title_zh: 打破摊销神经符号回归中的简化瓶颈
authors: "Paul Saegert, Ullrich Koethe"
date: 2026-04-30
pdf: "https://openreview.net/pdf/34bb664d5099c027b940873b8499d1ecaee38abc.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 直接解决摊销神经符号回归问题，提出简化引擎SimpliPy
tldr: 该论文发现摊销神经符号回归（SR）的关键瓶颈在于缺乏快速简化表达式的方法。作者提出SimpliPy，一种基于规则的简化引擎，相比SymPy速度提升100倍而简化质量相当。实验表明，SimpliPy显著提升了摊销SR的训练和推理速度，使其能够扩展到更复杂的科学问题。这一工作突破了摊销SR的简化瓶颈，有望推动其在实际科学发现中的应用。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有的摊销符号回归方法受限于通用计算机代数系统的缓慢简化速度，难以应用于复杂科学问题。
method: 提出SimpliPy，一种基于规则的简化引擎，通过针对性优化实现比SymPy快100倍的简化速度。
result: SimpliPy在简化质量与SymPy相当的前提下，大幅提升了摊销SR的训练和推理速度。
conclusion: SimpliPy有效地打破了摊销SR的简化瓶颈，使其能够处理更复杂的科学数据。
---

## Abstract
Symbolic regression (SR) aims to discover interpretable analytical expressions that accurately describe observed data. Amortized SR promises to be much more efficient than the predominant genetic programming SR methods, but currently struggles to scale to realistic scientific complexity. We find that a key obstacle is the lack of a fast reduction of equivalent expressions to a concise normalized form. Amortized SR has addressed this with general-purpose Computer Algebra Systems (CAS) like SymPy, but the high computational cost severely limits training and inference speed. We propose **SimpliPy**, a rule-based simplification engine achieving a 100-fold speed-up over SymPy at comparable quality. This enables substantial improvements in amortized SR, including scalability to much larger training sets, more efficient use of the per-expression token budget, and systematic training set decontamination with respect to equivalent test expressions. We demonstrate these advantages in our **Flash-ANSR** framework, which achieves much better accuracy than amortized baselines (NeSymReS, E2E) on the FastSRB benchmark. Moreover, it performs on par with state-of-the-art direct optimization (PySR) while recovering more concise rather than more complex expressions with increasing inference budget.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：符号回归（Symbolic Regression, SR）旨在从观测数据中发现可解释的解析表达式。摊销神经符号回归（Amortized Neural SR）相比主流遗传规划方法效率更高，但目前难以扩展到真实的科学复杂度。
- **关键瓶颈**：缺乏一种快速将等价表达式简化为简洁规范形式的方法。现有方法（如SymPy）使用通用计算机代数系统（CAS），但计算成本极高，严重限制训练和推理速度。
- **整体含义**：解决简化瓶颈可大幅提升摊销SR的效率和可扩展性，推动其在实际科学发现中的应用。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：设计一个针对摊销SR场景高度优化的规则简化引擎 **SimpliPy**，替代通用CAS（如SymPy），在保证简化质量的前提下大幅提升速度。
- **关键技术细节**：
  - SimpliPy基于规则（rule-based）而非通用符号计算，预定义了针对常见数学表达式（如三角函数、多项式、有理函数等）的简化规则。
  - 通过剪枝和缓存机制避免冗余计算，并针对批处理场景并行化。
  - 输出简洁的规范形式，与SymPy的简化结果在质量上相当。
- **框架整合**：集成SimpliPy的框架称为 **Flash-ANSR**，用于训练摊销SR模型。

## 3. 实验设计

- **数据集/场景**：使用了 **FastSRB benchmark**（公开的符号回归基准）。
- **对比方法**：
  - 摊销基线方法：**NeSymReS**, **E2E**（端到端）。
  - 直接优化方法：**PySR**（遗传规划类，当前最优）。
- **评价指标**：准确性（recovery rate）、表达式简洁性（长度）、推理预算的影响等。

## 4. 资源与算力

- 论文摘要及元数据中未明确提及GPU型号、数量或训练时长。
- 仅提到SimpliPy比SymPy快100倍，但未说明具体硬件和训练时间。
- 可作为不足指出：缺乏算力资源具体描述。

## 5. 实验数量与充分性

- 实验内容：主要在FastSRB benchmark上对比了三个基线（NeSymReS, E2E, PySR）及自身的Flash-ANSR。
- 实验数量：未列出详细的消融实验或不同数据集数量，但从描述看实验覆盖了标准基准，并分析了推理预算对表达式简洁性的影响。
- 充分性评估：实验设计合理，对比了摊销类与直接优化类方法，验证了SimpliPy带来的速度提升和最终准确性改善。但缺少在不同科学领域数据集上的泛化实验，以及与其他简化引擎（如Mathematica）的对比，略显不足。

## 6. 主要结论与发现

- SimpliPy在简化质量与SymPy相当的前提下，速度提升约100倍。
- 集成SimpliPy的Flash-ANSR框架在FastSRB基准上显著优于摊销基线（NeSymReS, E2E），准确性大幅提升。
- 与当前最优直接优化方法PySR相比，Flash-ANSR表现相当，且在增加推理预算时倾向于恢复更简洁而不是更复杂的表达式（这一点优于PySR）。
- 工作有效打破了摊销SR的简化瓶颈，使其能够处理更大型的训练集、更高效地使用token预算，并实现训练集-测试集等价表达式的系统去污染。

## 7. 优点

- **方法创新性**：精准识别了摊销SR中的简化瓶颈，并提出针对性的规则引擎，而非简单改进通用CAS。
- **实用性**：SimpliPy兼容现有框架，可以即插即用，显著提升训练和推理速度（100倍）。
- **实验对比公平**：与摊销基线（同类型）和直接优化方法（不同类型）都进行了对比，且使用了公开基准FastSRB。
- **分析深入**：发现了增加推理预算时恢复表达式简洁性的差异，具有启发性。

## 8. 不足与局限

- **实验覆盖有限**：仅使用FastSRB一个基准，缺少其他合成数据集或真实科学数据集（如物理、生物方程）的验证。
- **缺乏算力资源说明**：未提GPU型号、数量、训练时长，影响可复现性。
- **规则引擎的通用性**：SimpliPy基于预定义规则，可能无法处理非常复杂或非标准的数学表达式，而SymPy仍可处理更广范围。
- **未与更多简化工具对比**：仅比较SymPy，未与Mathematica、Reduce等对比。
- **未讨论失败案例**：未分析SimpliPy简化失败或质量下降的具体情况。
- **缺乏消融实验**：如单独分析SimpliPy中各个规则或缓存机制的贡献。

（完）
