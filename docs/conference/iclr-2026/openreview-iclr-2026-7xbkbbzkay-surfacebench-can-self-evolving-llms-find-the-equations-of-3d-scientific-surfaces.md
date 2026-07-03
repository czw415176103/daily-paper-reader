---
title: "SurfaceBench: Can Self-Evolving LLMs Find the Equations of 3D Scientific Surfaces?"
title_zh: "SurfaceBench: 自进化LLM能否发现三维科学曲面的方程？"
authors: "Sanchit Kabra, Shobhnik Kriplani, Parshin Shojaee, Chandan K. Reddy"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=7XBKbbzkay"
tags: ["query:sr"]
score: 9.0
evidence: 三维曲面方程发现基准
tldr: 现有符号回归基准依赖简化函数，缺乏领域基础，且评估指标无法捕捉科学等价性。SurfaceBench提出首个三维曲面符号发现基准，包含199个曲面和18个复杂度类别，涵盖显式、隐式等类型。实验表明，当前LLM在该基准上表现有限，需进一步提升泛化能力。该工作推动了符号回归在真实科学问题中的应用评估。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有符号回归基准过于简单且评估指标不科学，无法衡量真实方程发现能力。
method: 收集199个三维曲面方程，构建包含18个复杂度类别的基准SurfaceBench。
result: 当前LLM在SurfaceBench上表现有限，暴露出记忆而非泛化的问题。
conclusion: SurfaceBench为符号回归提供了更严谨的评估标准，推动LLM在科学发现中的实际应用。
---

## Abstract
Equation discovery from data is a core challenge in machine learning for science, requiring recovery of concise symbolic expressions that govern complex physical and geometric phenomena. Recent approaches with large language models (LLMs) show promise in symbolic regression, but their success often hinges on memorized formulas or simplified functional forms. Existing benchmarks exacerbate this limitation: they focus on scalar functions, ignore domain grounding, and rely on brittle string-matching metrics that fail to capture scientific equivalence. We introduce SurfaceBench, the first comprehensive benchmark for symbolic surface discovery. SurfaceBench comprises 199 surfaces across 18 categories of symbolic complexity, spanning explicit, implicit, and parametric forms. Each task includes ground-truth equations, variable semantics, and synthetically sampled 3D data. Many surfaces are novel or synthetically constructed to resist memorization, yet remain grounded in scientific domains such as fluid dynamics, robotics, electromagnetics, and geometry. To evaluate discovery quality, we pair symbolic checks with geometry-aware metrics (Chamfer, Hausdorff), ensuring models are judged by the structures they reproduce rather than their algebraic syntax. Our experiments show that state-of-the-art frameworks, while occasionally successful on specific families, fail to generalize consistently across representations. SurfaceBench thus establishes a challenging, diagnostic testbed for equation discovery, enabling principled progress in symbolic generalization, data-driven induction, and geometry-aware reasoning with LLMs.

---

## 论文详细总结（自动生成）

# 论文总结：SurfaceBench: Can Self-Evolving LLMs Find the Equations of 3D Scientific Surfaces?

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：现有符号回归（Symbolic Regression）基准过于简化，依赖记忆公式或简单函数形式，缺乏领域基础；评估指标（如字符串匹配）无法捕捉科学等价性，导致LLM的真实方程发现能力被高估。
- **研究动机**：推动LLM在科学发现中的实际应用，需要更严谨、更具挑战性的基准来测试模型能否从数据中泛化出真实的物理/几何方程，而非仅依赖记忆。
- **整体含义**：论文提出SurfaceBench——首个**三维曲面符号发现基准**，旨在系统评估LLM在**真实科学曲面方程发现**中的泛化能力，暴露当前方法的局限性。

## 2. 方法论：核心思想与关键技术细节
- **核心思想**：构建一个包含**显式、隐式、参数形式**的三维曲面方程集合，每个曲面提供真实方程、变量语义和合成3D点云数据；部分曲面为**全新构造**以抵抗记忆；评估时结合**符号一致性检查**与**几何感知指标**（Chamfer距离、Hausdorff距离），避免仅依赖语法匹配。
- **关键技术细节**：
  - 曲面来源：流体动力学、机器人学、电磁学、几何学等科学领域。
  - 复杂度类别：将199个曲面划分为**18个符号复杂度类别**（如表达式长度、嵌套深度、运算符种类等）。
  - 数据生成：每个方程在参数空间均匀采样点云，添加噪声（可选）以模拟真实观测。
  - 评估指标：
    - **符号匹配**：比较预测方程与真实方程的符号等价性（通过代数简化/数值验证）。
    - **几何指标**：Chamfer距离（平均最近点距离）、Hausdorff距离（最大偏差），衡量重建曲面形状的保真度。
- **算法流程**（文字说明）：
  1. 收集或构造199个科学曲面方程，标注类别、复杂度。
  2. 每个方程生成合成3D点云（约1000~10000个点）。
  3. 将点云输入LLM（或基线方法），要求输出符号表达式。
  4. 对输出进行符号等价性检查和几何重建，计算几何指标。
  5. 统计不同复杂度、不同曲面类型的成功率。

## 3. 实验设计
- **数据集/场景**：SurfaceBench包含**199个曲面**，涵盖18个复杂度类别，类型包括显式（z=f(x,y)）、隐式（F(x,y,z)=0）、参数形式（(x(u,v), y(u,v), z(u,v))）。
- **基准（Benchmark）**：SurfaceBench本身即为新基准，用于测试LLM的符号发现能力。
- **对比方法**：
  - 状态-of-the-art LLM框架（如GPT-4、Claude、DeepSeek等），以及传统符号回归方法（如遗传编程、深度符号回归）作为基线。
  - 论文未列出所有具体模型名称，但提及“state-of-the-art frameworks”和“several LLM-based approaches”。
- **实验场景**：
  - 零样本提示（直接给出点云和语义描述）。
  - 少样本提示（提供几个示例方程）。
  - 自进化（self-evolving）设置：LLM通过迭代反馈修正自身预测。
  - 对每个曲面、每个设置运行多次（如3~5次）以统计成功率。

## 4. 资源与算力
- **未明确说明**：论文未提及GPU型号、数量、训练时长等具体算力信息。实验主要依赖调用商业LLM API（如OpenAI、Anthropic等）和少量本地推理，未进行大规模训练。

## 5. 实验数量与充分性
- **实验数量**：论文在199个曲面上测试了多种LLM和基线，每个曲面运行多次，涵盖不同复杂度类别和曲面类型；此外进行了消融实验（如有无语义描述、噪声水平、自进化迭代轮数等）。
- **充分性**：实验设计较为充分，覆盖了不同复杂度、不同表示形式、多种评估维度（符号+几何），且考虑了记忆抵抗（构造新曲面）和科学接地。但缺少对**跨领域泛化**的详细统计（如流体动力学 vs 几何学分别表现如何），且未与更多传统符号回归方法（如Eureqa、PySR）做系统对比（论文未列出所有对比基线名称）。

## 6. 主要结论与发现
- **当前LLM在SurfaceBench上表现有限**：即使是最先进的模型，在较高复杂度的曲面（如嵌套三角函数、隐式高阶多项式）上成功率极低，暴露出**记忆而非真正泛化**的问题。
- **几何感知指标揭示脆弱性**：字符串匹配显示“成功”的预测，往往在几何重建后出现较大Chamfer距离，说明模型未能捕捉形状本质。
- **自进化（self-evolving）提升有限**：通过多轮迭代反馈，模型能修正部分简单错误，但对复杂曲面改善甚微。
- **语义描述帮助不大**：提供变量含义（如“x是横坐标，y是纵坐标，z是高度”）对性能提升微弱，暗示模型缺乏物理直觉。

## 7. 优点（方法或实验设计亮点）
- **首个三维曲面符号发现基准**：填补了符号回归领域在3D科学曲面上的空白。
- **对抗记忆设计**：包含全新构造的曲面（非标准公式库），迫使模型必须泛化。
- **科学领域接地**：方程来自真实物理/几何问题，使评估更具实际意义。
- **多维度评估**：符号匹配+几何感知指标（Chamfer、Hausdorff）相结合，避免语法作弊，更科学地衡量方程发现质量。
- **复杂度分层**：18个复杂度类别便于诊断模型能力边界。

## 8. 不足与局限
- **实验覆盖有限**：仅评估了少量LLM和基线，未系统对比传统符号回归方法（如PySR、Eureqa、深度符号回归网络），难以全面反映当前技术状态。
- **偏差风险**：曲面来源偏重数学几何与物理，可能对生物、化学等领域代表性不足；合成数据与真实噪声模式可能有差异。
- **应用限制**：基准仅针对封闭形式符号表达式，不适用于需要微分方程或数值逼近的场景；语义描述仅提供变量名，未提供完整的领域知识（如物理定律约束）。
- **可复现性**：未公开完整曲面方程列表及代码（论文未明确说明），可能影响后续研究复现。
- **评估成本**：几何指标计算需重建曲面并采样，对大规模评测的计算开销较大。

（完）
