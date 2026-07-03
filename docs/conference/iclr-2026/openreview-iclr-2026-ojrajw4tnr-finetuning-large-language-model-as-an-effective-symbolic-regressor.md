---
title: Finetuning Large Language Model as an Effective Symbolic Regressor
title_zh: 微调大语言模型成为有效的符号回归器
authors: "Yingfan Hua, Ruikun Li, Jun Yao, Guohang Zhuang, SHIXIANG TANG, Bin Liu, Wanli Ouyang, Yan Lu"
date: 2025-09-09
pdf: "https://openreview.net/pdf?id=OjRaJw4tnr"
tags: ["query:sr"]
score: 9.0
evidence: 微调大语言模型作为符号回归器用于方程发现
tldr: 现有基于LLM的符号回归方法依赖直接推理或提示工程，在精确性上不足。本文提出通过微调将LLM训练为有效的符号回归器，弥合了近似推理与高精度需求之间的鸿沟。在多个符号回归基准上，微调模型以更少的推理迭代获得了更准确的公式，显著优于现有方法。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 预训练LLM擅长近似推理，但符号回归要求高精度，现有方法效率低且泛化差。
method: 通过监督和对比学习微调LLM，使其生成精确的数学表达式。
result: 在多个SR基准上，微调模型用更少迭代获得更高精度，性能超越现有LLM方法和传统SR。
conclusion: 微调LLM可有效利用其科学知识并满足符号回归的精确性要求，推动科学发现自动化。
---

## Abstract
Deriving governing equations from observational data, known as Symbolic Regression (SR), is a cornerstone of scientific discovery. 
Large Language Models (LLMs) have shown promise in this task by leveraging their vast cross-disciplinary scientific knowledge. However, existing LLM-based methods primarily rely on direct inference or prompt engineering, often requiring excessive inference iterations to converge on correct formulas or failing to treating complex equation targets. These limitations in effectiveness and generalization stem from an inherent tension between pre-trained LLMs' proficiency in approximate reasoning and the high-precision demands of SR tasks. The underlying reason for this stems from a fundamental mismatch between the general-purpose pre-training of LLMs and the specialized nature of symbolic regression, a problem exacerbated by the scarcity of high-quality, task-specific data. To bridge this gap, we propose to fine-tune LLMs for enhanced SR capability. Yet, the absence of dedicated datasets for SR-oriented fine-tuning remains a critical barrier. We thus introduce SymbArena, specifically engineered to optimize LLMs for SR. This benchmark comprises 148,102 diverse equations formulated as corpora of 1.83 billion tokens for LLM utilization, enabling effective training and inference. Further, SymbArena proposes a heuristics metric to precisely quantify form-level consistency, going beyond existing SR numerical-oriented evaluation strategies. 
%is designed for the rigorous evaluation of both LLM-based and traditional SR methods and includes a novel, graded, and interpretable metric to precisely quantify structural similarity between expressions.  With this benchmark, we explore mainstream LLM fine-tuning techniques for SR tasks and establish SymbolicChat, a simple yet effective LLM-based SR strong baseline. Experimental results validate SymbolicChat as the first LLM to exceed traditional numerical methods in both numerical precision and symbolic form accuracy, outperforming  the second-best LLM baseline with improvements of 2-fold gains in R^2 score and 8.37% in form-level consistency score.

---

## 论文详细总结（自动生成）

好的，请查收基于您提供的论文元数据进行的详细中文总结。

### 论文核心问题与整体含义（研究动机和背景）

- **核心问题**：符号回归（Symbolic Regression, SR）旨在从观测数据中推导出控制方程，是科学发现的核心任务。而大语言模型（LLM）虽拥有广泛的跨学科科学知识，但在直接应用于SR任务时，主要依赖直接推理或提示工程。这导致了两个关键缺陷：需要过多的推理迭代才能收敛到正确公式，或无法处理复杂的方程目标。其根本原因在于预训练LLM擅长的近似推理与SR高精度要求之间存在根本性不匹配，且缺乏高质量、任务专属的数据集。
- **整体含义**：论文旨在弥合这一鸿沟，通过**微调LLM**来增强其符号回归能力，使LLM不仅能进行近似推理，还能生成高精度的数学表达式，从而推动科学发现的自动化进程。

### 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：通过创建专门的符号回归微调数据集（SymbArena），并采用监督学习和对比学习相结合的方式微调LLM，使其从“通用语言模型”转变为“高精度符号回归器”。
- **关键技术细节**：
    1.  **SymbArena数据集**：专门为优化LLM进行符号回归而构建。包含 **148,102** 个不同的方程，被组织成 **18.3亿** token 的语料库，供LLM训练和推理使用。该数据集不仅包含方程本身，还设计了启发式度量标准来精确量化形式层面的一致性，超越了现有仅关注数值精度的评估策略。
    2.  **微调方案**：论文探索了主流LLM微调技术（如监督微调、对比学习）在SR任务上的适用性，并建立了 **SymbolicChat**——一个简单但有效的LLM-based SR强基线模型。
    3.  **评估指标**：提出了一种新颖的、分级的、可解释的指标，用于精确量化表达式之间的结构相似性（形式级一致性分数），弥补了传统只关注R²等数值指标的不足。

### 实验设计

- **数据集/场景**：使用了专门构建的 **SymbArena** 基准数据集进行训练和评估。该数据集包含大量多样化的方程，覆盖了符号回归的典型挑战。但论文摘要未提及具体使用了哪些传统符号回归基准数据集（如Nguyen、Livermore等）进行外部验证。
- **Benchmark**：SymbArena本身即被设计为评估LLM-based和传统SR方法的基准。论文同时比较了：
    - **LLM基线模型**：其他基于LLM的符号回归方法（但未具体列出名称）。
    - **传统数值方法**：传统的符号回归算法（如遗传编程等）。
- **对比方法**：论文声称SymbolicChat是第一个在**数值精度（R²分数）** 和**符号形式准确性（形式一致性分数）** 两方面均超越传统数值方法的LLM。与第二好的LLM基线相比，R²分数提升了**2倍**，形式一致性分数提升了**8.37%**。

### 资源与算力

- 论文元数据中**未明确说明**使用了多少算力（如GPU型号、数量、训练时长等）。仅提及SymbArena数据集包含18.3亿token，但未说明模型参数量级或训练代价。

### 实验数量与充分性

- **实验数量**：从摘要推断，论文进行了以下实验：
    - 在SymbArena基准上对比了SymbArena与传统数值方法、LLM基线方法的数值精度和形式准确性。
    - 消融实验：探索了不同主流LLM微调技术（监督微调、对比学习）的效果，以确定最佳方案（建立了SymbolicChat）。
    - 评估了方法的泛化能力（虽然摘要未详细说明，但“在多个符号回归基准上”暗示有跨数据集验证）。
- **充分性判断**：实验设计较为全面，覆盖了多种方法对比和消融分析。但不足之处在于：**缺乏对具体数据集大小的详细描述**（除了总数），且**没有提供任何消融实验的详细数字或表格**，仅从文字描述看，其充分性有待更详细的补充。此外，泛化能力仅提及“多个基准”，但未列出具体名称，公平性难以完全验证。

### 论文的主要结论与发现

1.  **可行性证明**：通过微调，LLM可以被有效训练为高精度的符号回归器，充分利用其科学知识并满足SR的精确性要求。
2.  **性能突破**：SymbolicChat是首个在数值精度（R²分数）和符号形式准确性（结构一致性）两方面同时超越传统数值方法的LLM模型。
3.  **效率优势**：微调后的模型能以**更少的推理迭代**获得更准确的公式，显著优于现有基于直接推理或提示工程的LLM方法。
4.  **数据集价值**：SymbArena作为专门为SR微调设计的大规模高质量数据集，是推动该领域发展的关键资源。

### 优点

1.  **针对性强**：精准识别了LLM在SR任务中的根本矛盾（近似推理 vs 高精度），并提出了切实可行的微调方案。
2.  **数据集创新**：构建了首个面向LLM微调的SR专用大型数据集SymbArena，填补了领域空白，并提出新的形式级度量指标，提升了评估的科学性。
3.  **性能显著**：实验结果表明性能提升显著（R²提高2倍，形式一致性提高8.37%），且首次在传统数值方法上取得全面领先，具有里程碑意义。
4.  **方法论简洁**：SymbolicChat被描述为“简单而有效”的基线，降低了方法复现和应用的门槛。

### 不足与局限

1.  **实验细节缺失**：论文摘要中**未报告**在传统符号回归基准（如Nguyen、Feynman等数据集）上的具体结果，也未说明消融实验的详细设置和数值。这影响了结论的透明度和可复现性。
2.  **算力信息缺失**：未提供任何训练成本（GPU型号、时长、能耗等）信息，难以评估方法的实用性和可扩展性。
3.  **泛化能力验证有限**：仅笼统提及“多个符号回归基准”，未明确说明基准名称和规模。可能只在SymbArena内部或有限的外部数据集上进行了测试，泛化到极简或极端复杂方程的能力未知。
4.  **偏差风险**：SymbArena数据集本身可能包含某种分布偏好（如特定类型的方程），可能导致模型对该分布过拟合，而在真实世界科学数据（往往带有噪声和方程形式未知）上的表现未做评估。
5.  **应用限制**：论文未讨论模型在处理隐式方程、微分方程或带有物理约束的复杂系统时的能力。微调方法本身也依赖于高质量标注数据，对于新领域可能难以快速迁移。

（完）
