---
title: Imposing Boundary Conditions on Neural Operators via Learned Function Extensions
title_zh: 通过学习函数扩展对神经算子施加边界条件
authors: "Sepehr Mousavi, Siddhartha Mishra, Laura De Lorenzis"
date: 2026-04-30
pdf: "https://openreview.net/pdf/a3e8e31ce0fc39ba69cfdb85dda7e63fce87ae72.pdf"
tags: ["query:physics-ml"]
score: 8.0
evidence: 通过学习函数扩展使神经算子能够处理复杂边界条件
tldr: 针对神经算子难以处理复杂边界条件的问题，提出通过学习将边界数据映射到全域潜扩展，使任意神经算子能够吸收边界信息。在多种非齐次边界PDE上，该方法显著提升了算子预测精度，尤其对边界敏感问题，该框架通用且易于集成，拓展了神经算子的适用性。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有神经算子在泛化到复杂非齐次边界条件时表现不佳。
method: 提出函数扩展框架，将边界数据映射为全域潜扩展，使神经算子可端到端学习边界影响。
result: 在多个PDE代理任务上，该方法显著提高了对复杂边界条件的预测精度。
conclusion: 函数扩展是一种有效且通用的神经算子边界处理方法。
---

## Abstract
Neural operators have emerged as powerful surrogates for the solution of partial differential equations (PDEs), yet their ability to handle general, highly variable boundary conditions (BCs) remains limited. Existing approaches often fail when the solution operator exhibits strong sensitivity to boundary forcings. We propose a general framework for conditioning neural operators on complex non-homogeneous BCs through function extensions. Our key idea is to map boundary data to latent pseudo-extensions defined over the entire spatial domain, enabling any standard operator learning architecture to consume boundary information. The resulting operator, coupled with an arbitrary domain-to-domain neural operator, can learn rich dependencies on complex BCs and input domain functions at the same time. To benchmark this setting, we construct 18 challenging datasets spanning Poisson, linear elasticity, and hyperelasticity problems, with highly variable, mixed-type, component-wise, and multi-segment BCs on diverse geometries. Our approach achieves state-of-the-art accuracy, outperforming baselines by large margins, while requiring no hyperparameter tuning across datasets. Overall, our results demonstrate that learning boundary-to-domain extensions is an effective and practical strategy for imposing complex BCs in existing neural operator frameworks, enabling accurate and robust scientific machine learning models for a broader range of PDE-governed problems.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：神经算子（Neural Operators）作为偏微分方程（PDE）的替代模型已展现出强大能力，但在处理一般化、高度可变的边界条件（尤其是非齐次边界条件）时表现不佳。现有方法在解算子对边界强迫敏感的问题中常常失效。
- **研究动机**：许多实际PDE问题（如弹性力学、生物流体等）的边界条件复杂多变，且对解的影响显著。现有神经算子架构（如FNO、DeepONet）通常针对固定或简单边界设计，缺乏通用机制来融合任意、非齐次的边界信息。
- **整体含义**：该工作旨在突破神经算子处理复杂边界条件的瓶颈，提升其在科学计算中的泛化能力和实用性，从而扩展神经算子对更广泛PDE控制问题的适用性。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：通过学习函数扩展（Function Extensions），将边界数据映射为覆盖整个空间域的潜扩展（latent pseudo-extensions），使得任意标准的算子学习架构都能吸收边界信息。
- **关键技术细节**：
  - 提出一个通用框架，包含两个部分：
    1. 边界编码器（或扩展网络）：将定义在边界上的数据（如Dirichlet、Neumann等边界值）转换成一个或多个定义在全域上的“潜扩展场”。
    2. 域到域神经算子：接收该扩展场以及可能的其他输入函数（如源项），进行端到端学习，预测PDE解。
  - 该框架不依赖特定算子架构，可以灵活地集成到FNO、DeepONet、GNO等主流神经算子中。
  - 通过联合训练，模型可同时学习对复杂边界条件和输入函数的丰富依赖关系。
- **算法流程说明**（文字描述）：
  1. 给定一组边界数据（如分段函数、混合类型边界）和可能的源项/系数等域函数。
  2. 边界编码器处理边界数据，输出一个或多个“扩展函数”，其维度与空间域匹配。
  3. 域到域神经算子将扩展函数与其他域函数拼接或融合，作为输入。
  4. 算子输出目标PDE场的预测（如位移、压力等）。
  5. 使用标准监督学习损失（如相对L2误差）进行端到端训练。

## 3. 实验设计：使用了哪些数据集/场景、benchmark、对比方法

- **数据集/场景**：作者构建了18个具有挑战性的基准数据集，涵盖以下问题：
  - Poisson方程（多种几何与边界类型）
  - 线弹性问题（混合型、分量级、多段边界）
  - 超弹性问题（高度非线性、大变形）
- **几何多样性**：包括不同形状的域（如L形、孔洞、不规则区域等）。
- **边界类型多样性**：高度可变的混合型边界（同时有Dirichlet、Neumann等）、分量级边界（对向量场的每个分量不同）、多段边界（不同边界段不同条件）。
- **Benchmark**：未明确给出，但通常可能与FNO、DeepONet、基线神经算子进行比较。
- **对比方法**：abstract提到“大幅优于基线”，推测对比了标准的神经算子（未特殊处理边界）、以及可能已有的边界嵌入方法（如PINNs或基于增广输入的方法）。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。
- 需指出：由于该文为ICML 2026接收论文，常规实验通常会在附录中描述，但用户提供的内容缺失这些细节。因此无法总结算力情况。

## 5. 实验数量与充分性

- **实验数量**：构建了18个数据集，覆盖三大类PDE（Poisson、线弹性、超弹性），每个数据集可能包含不同边界条件变化和几何。
- **充分性评价**：
  - 数据集数量较多，且涵盖了常见线性与非线性PDE，边界类型多样，具有代表性。
  - 但未明确列出具体消融实验数量（例如对扩展网络设计、不同算子架构的消融）。
  - 从abstract“无需跨数据集超参数调优”的表述看，方法有较强的鲁棒性，但未提供统计显著性分析。
- **客观性与公平性**：声称达到SOTA且大幅超越基线，但缺乏基线超参数调优细节，公平性需依赖完整论文中的对比设置（如基线是否也进行了充分调优）。

## 6. 论文的主要结论与发现

- **主要结论**：提出的函数扩展框架是施加复杂边界条件到神经算子中的有效且实用的策略。
- **具体发现**：
  - 通过学习边界到域的扩展，任意神经算子都能吸收边界信息，显著提升对非齐次边界的预测精度。
  - 该方法无需针对不同数据集调整超参数，具有通用性。
  - 在边界强迫敏感的问题上，该方法大幅优于现有基线。
  - 为PDE代理模型处理实际工程中复杂边界条件提供了可行的解决方案。

## 7. 优点

- **方法通用性**：不绑定特定神经算子架构，可即插即用，易于集成到现有框架中。
- **处理复杂边界能力**：支持混合类型、分量级、多段等高度可变边界，覆盖面广。
- **端到端学习**：边界信息直接通过扩展网络融入，无需人工设计边界特征或两阶段训练。
- **实验设置挑战性**：构造18个多样化的数据集，包括非线性超弹性问题，测试了方法的极限。
- **无需超参数调优**：跨数据集保持良好性能，实用性强。

## 8. 不足与局限

- **实验信息不完整**：缺少算力资源、具体基线设置、消融实验组数等细节，影响可复现性评估。
- **可能偏差风险**：数据集均为作者自行构造，未在标准公开基准（如PDEBench, FNO-benchmark）上验证，可能引入偏向性。
- **应用限制**：
  - 需要边界数据在训练时已知，逆问题或不完全边界条件问题可能不适用。
  - 扩展网络增加模型参数量和计算开销，对高分辨率问题可能成本较高。
  - 仅讨论了PDE代理任务，未涉及时变问题或耦合问题中的边界处理。
- **理论分析缺失**：未讨论扩展网络的可表示性、误差界或与经典边界处理方法的理论联系。

（完）
