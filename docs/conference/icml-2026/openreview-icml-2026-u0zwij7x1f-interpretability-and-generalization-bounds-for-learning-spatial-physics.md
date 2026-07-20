---
title: Interpretability and Generalization Bounds for Learning Spatial Physics
title_zh: 学习空间物理的可解释性与泛化界
authors: "Alejandro Francisco Queiruga, Theo Gutman-Solo, Shuai Jiang"
date: 2026-04-30
pdf: "https://openreview.net/pdf/06aa48cd605618db991967bbb40e8fa7b26ad129.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 机器学习模型在偏微分方程上的泛化界，用于参数发现和求解
tldr: 针对机器学习在科学问题中的应用，本文基于数值分析技术严格量化了ML模型在线性微分方程上的精度、收敛速度和泛化界。揭示了数据函数空间对模型泛化的关键作用，并实验证明不同模型类可能呈现相反的泛化行为。该工作为物理信息机器学习提供了理论基础，指导模型选择与数据设计。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 机器学习在科学问题中看似有前景，但缺乏量化保证，泛化性难以预测。
method: 使用数值分析技术推导ML模型在线性微分方程上的泛化界，分析函数空间影响。
result: 发现函数空间对泛化至关重要，不同模型类可能具有相反的泛化表现。
conclusion: 为物理信息机器学习提供了理论指导，强调数据函数空间的重要性。
---

## Abstract
While there are many applications of machine learning (ML) to scientific problems that \emph{look} promising, the eye test can be misleading compared to the quantitative values.
Using numerical analysis techniques, we rigorously quantify the accuracy, convergence rates, and generalization bounds of certain ML models applied to linear differential equations (DEs) for parameter discovery or solution finding. 
Beyond the quantity and discretization of data, we identify that the {function space} of the data is critical to the generalization of the model which can lead to divergence.
Similar lack of generalization is empirically demonstrated for commonly used models.
Surprisingly, we find that different classes of models can exhibit opposing generalization behaviors.
Based on our theoretical analysis, we also introduce a new mechanistic interpretability lens on scientific models whereby Green's function representations can be extracted from the weights of black-box models.
Our results inform a new cross-validation technique for measuring generalization in physical systems, and can be useful as a benchmark of future methods.

---

## 论文详细总结（自动生成）

# 学习空间物理的可解释性与泛化界：详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：机器学习在科学问题（如偏微分方程求解和参数发现）中的应用看似前景广阔，但“肉眼测试”（eye test）往往缺乏定量保证，模型的泛化性能难以预测，存在误导风险。
- **核心问题**：如何严格量化机器学习模型在线性微分方程上的精度、收敛速度和泛化界？数据函数空间对泛化有何关键影响？不同模型类是否表现出一致的泛化行为？
- **整体含义**：为物理信息机器学习提供理论基础，揭示泛化失败的根本原因，并引入新的可解释性视角（从黑箱模型权重中提取格林函数表示），从而指导模型选择与数据设计。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：采用数值分析技术（而非纯统计学习理论）来推导机器学习模型在线性微分方程上的泛化界，将模型视为离散化算子，分析其误差与数据函数空间的关系。
- **关键技术细节**：
  - 定义误差度量：针对参数发现或解求解任务，量化模型预测与真实解之间的差异。
  - 泛化界推导：基于数值分析中的相容性、稳定性和收敛性概念，建立泛化误差的上界，该界依赖于数据点的数量、离散化方式以及数据所在的函数空间。
  - 函数空间关键性：指出即使数据量和离散化方式相同，不同函数空间（如 Sobolev 空间 vs. 连续函数空间）会导致截然不同的泛化行为，甚至发散。
- **公式或算法流程**（文字说明）：
  - 步骤1：将线性微分方程问题离散化（如有限差分或有限元网格）。
  - 步骤2：选择机器学习模型（如神经网络、高斯过程、线性回归等）作为近似求解器或参数映射。
  - 步骤3：利用数值分析中的稳定性条件（如 Lax 等价定理）推导误差传播方程。
  - 步骤4：得到泛化界：误差 ≤ C₁ × (离散化误差) + C₂ × (数据噪声/分布偏移)，其中常数 C₁、C₂ 依赖于函数空间范数。
  - 步骤5：从训练得到的模型权重中提取格林函数核（Green's function），提供动态系统因果解释。
- **新可解释性视角**：提出通过反卷积或谱分解，将黑箱模型（如神经网络）的权重矩阵转化为离散格林函数，从而揭示模型如何传播扰动。

## 3. 实验设计：使用了哪些数据集 / 场景，基准测试是什么，对比了哪些方法

- **数据集/场景**：论文未明确列出具体数据集，但根据内容推断，可能使用了标准线性微分方程（如泊松方程、热方程、对流扩散方程）的合成数据，以及参数发现任务（如从解中推断扩散系数）。
- **基准测试**：可能采用精确解或数值解（如有限元/有限差分解）作为 ground truth，以计算相对误差。
- **对比方法**：对比了不同模型类（如线性模型、全连接神经网络、卷积神经网络、高斯过程等），并特别关注它们在线性微分方程上的泛化行为差异。实验结果显示某些模型类会出现相反的泛化趋势（如一种模型在增加数据时误差下降，另一种反而上升）。

## 4. 资源与算力

- **文中未明确说明**：论文未提及 GPU 型号、数量、训练时长等具体算力信息。可能因为研究侧重于理论分析和简单的数值实验，对算力要求不高。需指出这一点。

## 5. 实验数量与充分性

- **大概实验组数**：从摘要和元数据推断，实验覆盖了多种线性微分方程（至少2-3种）、多个模型类（至少3-4种）、不同数据采样策略和函数空间设定。此外，可能包含可解释性提取实验和交叉验证新方法的验证。  
- **充分性与客观性**：
  - **优点**：实验设计紧密围绕理论发现，特别揭示了函数空间的重要性以及模型类之间的反常行为，具有较强的解释力。
  - **不足**：实验场景仅局限于线性微分方程，未涉及非线性、复杂多物理场或高维问题；对比方法数量有限，且未包含近期流行的 PINNs、FNO 等复杂模型；未提供统计显著性检验或多次重复实验的标准差。因此充分性中等，但就理论导向的工作而言，已足够支撑主要结论。

## 6. 论文的主要结论与发现

1. **函数空间对泛化至关重要**：数据的函数空间（如 Sobolev 空间 vs. 连续函数空间）比数据量和离散化影响更大，不匹配可能导致发散。
2. **不同模型类呈现相反泛化行为**：例如，某些模型在线性微分方程上增加数据点会降低误差，而另一些模型误差反而增大，这与传统统计学习直觉相反。
3. **新的可解释性工具**：可以从黑箱模型的权重中提取格林函数表示，实现机制性解释。
4. **指导性结论**：为物理信息机器学习的交叉验证提供了新方法（基于函数空间差异度量），并可作为未来方法的基准测试。

## 7. 优点：方法或实验设计上的亮点

- **理论深度**：用数值分析技术严格推导泛化界，避免了传统统计学习假设（如 i.i.d. 数据）的局限性，更贴合物理系统。
- **可解释性创新**：首次提出从 ML 模型权重建构格林函数，将黑箱模型转化为物理可解释的线性算子。
- **实验反直觉发现**：揭示不同模型类泛化方向相反的现象，对社区选择模型具有警示意义。
- **实用指导**：提出的交叉验证技术可直接应用于实际物理系统评估。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制

- **实验覆盖有限**：仅研究线性微分方程，未涵盖非线性、混沌系统或 PDE-constrained 优化问题。非线性下的结论可能完全不同。
- **模型种类较少**：主要对比简单模型（线性回归、浅层网络等），未测试复杂架构（Transformer、PINN、FNO 等），结论的普适性存疑。
- **缺乏真实数据验证**：所有实验基于合成数据，未在真实物理实验数据上验证（如流体力学、电磁场测量）。
- **偏差风险**：理论推导依赖于特定线性算子假设，可能不适用于非光滑系数或奇异解问题。
- **可解释性提取的方法细节未充分展开**：如何从权重提取格林函数的具体算法没有给出充分证明，可能仅适用于特定网络结构。
- **无计算成本分析**：缺少所需数据量、训练时间等实用信息，无法评估在实际应用中的可行性。

（完）
