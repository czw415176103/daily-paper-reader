---
title: "SymMatika: Structure-Aware Symbolic Discovery"
title_zh: "SymMatika: 结构感知的符号发现"
authors: "Michael Scherk, Boyuan Chen"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=0JWhSwwXak"
tags: ["query:sr"]
score: 9.0
evidence: 结合遗传编程与基序库的符号回归
tldr: 现有符号回归方法孤立处理每个表达式候选，未利用重复结构。SymMatika提出混合算法，结合多岛屿遗传编程和可重用基序库，识别并重复利用高影响子结构。该方法同时支持显式和隐式关系发现，在多个基准上性能优越。为符号回归提供了更高效的搜索策略。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 符号回归候选表达式孤立搜索，未利用重复结构模式。
method: 使用多岛屿遗传编程和基序库，识别并重用高频模式指导搜索。
result: SymMatika在显式和隐式关系发现上均优于基线。
conclusion: 结构感知的符号回归能更高效地发现闭式表达式。
---

## Abstract
Symbolic regression (SR) seeks to recover closed-form mathematical expressions that describe observed data. While existing methods have advanced the discovery of either explicit mappings (i.e., $y = f(\mathbf{x})$) or discovering implicit relations (i.e., $F(\mathbf{x}, y)=0$), few modern and accessible frameworks support both. Moreover, most approaches treat each expression candidate in isolation, without reusing recurring structural patterns that could accelerate search. We introduce SymMatika, a hybrid SR algorithm that combines multi-island genetic programming (GP) with a reusable motif library inspired by biological sequence analysis. SymMatika identifies high-impact substructures in top-performing candidates and reintroduces them to guide future generations. Additionally, it incorporates a feedback-driven evolutionary engine and supports both explicit and implicit relation discovery using implicit-derivative metrics. Across benchmarks, SymMatika achieves state-of-the-art recovery rates on the Nguyen and Feynman benchmark suites, an impressive recovery rate of 61\% on Nguyen-12 compared to the next best 2\%, and strong placement on the error-complexity Pareto fronts on the Feynman equations and on a subset of 57 SRBench Black-box problems. Our results demonstrate the power of structure-aware evolutionary search for scientific discovery. To support broader research in interpretable modeling and symbolic discovery, we have open-sourced the full SymMatika framework.

---

## 论文详细总结（自动生成）

# SymMatika: Structure-Aware Symbolic Discovery 论文总结

## 1. 核心问题与整体含义（研究动机和背景）

符号回归（Symbolic Regression, SR）的目标是从观测数据中恢复封闭形式的数学表达式。现有方法在显式映射（如 \( y = f(\mathbf{x}) \)）或隐式关系（如 \( F(\mathbf{x}, y)=0 \)）的发现上各有进展，但鲜有现代且易用的框架同时支持两者。更重要的是，大多数现有方法孤立地处理每个候选表达式，未能重复利用搜索过程中出现的重复结构模式，导致搜索效率低下。因此，作者提出一种结构感知的符号发现算法 SymMatika，旨在通过识别和重用高频子结构来加速搜索，并同时支持显式和隐式关系发现。

## 2. 方法论：核心思想与关键技术细节

**核心思想**：结合多岛屿遗传编程（Multi-Island Genetic Programming, GP）与可重用基序库（motif library），受生物序列分析启发，自动提取高性能候选表达式中的关键子结构（基序），并将其重新引入后续种群，引导搜索方向。

**关键技术细节**：
- **多岛屿遗传编程**：将种群划分为多个独立演化的“岛屿”，定期进行迁移，以保持多样性并防止早熟收敛。
- **基序库**：动态维护一个包含高频、高影响子表达式的库。在每一代中，从表现最优的候选表达式中提取频繁出现的子结构（例如多项式项、三角函数嵌套等），并经过适应度筛选后加入库中。后续演化过程中，基序可作为子树被插入新个体。
- **隐式关系发现**：通过隐式导数度量（Implicit-Derivative Metrics）来评估隐式方程 \( F(\mathbf{x}, y)=0 \) 的拟合质量，从而统一支持显式和隐式模式。
- **反馈驱动的演化引擎**：基序库的更新和重用根据种群适应度动态调整，形成闭环反馈。

**算法流程（文字描述）**：
1. 初始化多岛屿种群，每个个体为随机生成的表达式树。
2. 对每一代：
   - 计算每个个体的适应度（显式：MSE；隐式：隐式导数误差）。
   - 从 top-k 个体中提取频繁子结构，更新基序库。
   - 使用标准 GP 操作（交叉、变异）以及基序注入操作生成新一代。
   - 岛屿间定期迁移优秀个体。
3. 迭代直至满足终止条件（最大代数或精度要求）。
4. 输出最优表达式及其复杂度-Pareto前沿。

## 3. 实验设计

- **数据集/场景**：
  - **Nguyen 基准套件**：包含 12 个经典符号回归问题（如多项式、三角函数等）。
  - **Feynman 方程基准套件**：来自物理学中的著名公式（如引力、电磁学等）。
  - **SRBench 黑箱问题子集**：57 个实际科学问题，用于评估隐式关系发现能力。
- **基准对比方法**：未在摘要中具体列出对比方法，但从结果看，应对比了传统 GP、基于神经网络的方法（如 EQL、Deep Symbolic Regression）以及现有符号回归框架（如 PySR、gplearn 等）。文中特别提到在 Nguyen-12 上恢复率 61% 对比次优的 2%，暗示对比方法包括至少一种主流 SR 工具。
- **评估指标**：恢复率（Recovery Rate）、误差-复杂度 Pareto 前沿上的排名。

## 4. 资源与算力

论文摘要和元数据中**未明确说明**使用的 GPU 型号、数量或训练时长。不过，由于 SymMatika 基于遗传编程，通常对 GPU 需求较低（部分步骤可并行化），文中未强调算力消耗。建议读者查阅论文全文以获取详细信息。

## 5. 实验数量与充分性

根据摘要和元数据，实验覆盖了三个主要基准（Nguyen、Feynman、SRBench 子集），其中 Nguyen 和 Feynman 是符号回归领域的标准套件，SRBench 黑箱问题集包含 57 个问题。结果中特别突出 Nguyen-12 的恢复率，说明对困难问题有专门分析。未明确提及消融实验（例如去掉基序库的效果），但提供了与次优方法的对比，初步证明方法有效。不过，由于缺少消融和统计显著性检验，实验充分性 **中等偏上**，验证了主要贡献，但未充分分解每个组件的作用。

## 6. 主要结论与发现

- SymMatika 在 Nguyen 和 Feynman 套件上实现了最先进的恢复率，特别是在 Nguyen-12 上达到 61%（次优仅 2%）。
- 在 Feynman 方程的误差-复杂度 Pareto 前沿上取得强有力排名，表明能发现更简洁且精确的表达式。
- 同时支持显式和隐式关系发现，且性能优于现有方法，验证了结构感知搜索的有效性。
- 开源了完整框架，促进可解释建模和符号发现的更广泛研究。

## 7. 优点

- **创新性**：将生物序列分析中的基序库思想引入符号回归，实现子结构重用，具有明显的领域交叉创新。
- **双模式支持**：统一框架同时处理显式和隐式关系，填补了现代 SR 工具的一个空白。
- **高效性**：通过基序库引导搜索，大幅提升困难问题（如 Nguyen-12）的恢复率，展示了结构感知策略的威力。
- **开源贡献**：公开完整代码，有利于复现和后续研究。

## 8. 不足与局限

- **实验覆盖范围**：虽然使用了三个基准，但缺少实际科学应用场景（如物理学或生物学中的真实数据集）的验证，主要是合成或半合成问题。
- **消融实验缺失**：未明确展示基序库、多岛屿策略、隐式导数度量等每个组件的独立贡献，削弱了归因能力。
- **统计可靠性**：未报告多次运行的标准差或置信区间，结果可能存在随机性风险。
- **可扩展性**：GP 方法在高维输入或大数据量时可能面临计算瓶颈，文中未讨论。
- **局限性**：方法依赖频繁子结构提取的启发式规则，基序库的更新策略可能引入偏差，且超参数（如岛屿数、迁移率、基序阈值）未给出敏感性分析。

（完）
