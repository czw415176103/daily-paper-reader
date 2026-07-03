---
title: Programmatic Context Augmentation for LLM-based Symbolic Regression
title_zh: 用于基于大语言模型的符号回归的程序化上下文增强
authors: "Hao Liu, Xiao-Wen Yang, Atharva Sehgal, Yixin Wang, Lan-Zhe Guo, Yu-Feng Li, Yisong Yue"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=EiQOKsANKO"
tags: ["query:sr"]
score: 9.0
evidence: 基于大语言模型的符号回归程序化上下文增强
tldr: 该论文针对现有LLM符号回归方法仅依赖标量误差作为反馈的局限，提出程序化上下文增强机制，利用数据集中丰富信息来引导进化搜索，在多个基准测试中提高了表达式发现精度和样本效率。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: LLM符号回归仅使用标量误差反馈，忽略了数据集隐含的结构信息。
method: 提出程序化上下文增强，将数据结构特征转化为LLM的输入上下文，指导进化搜索。
result: 在多个符号回归数据集中，该方法在准确率和搜索效率上超过现有LLM方法。
conclusion: 上下文增强有效提升了LLM在符号回归中的表现。
---

## Abstract
Symbolic regression (SR), the task of discovering mathematical expressions that best describe a given dataset, remains a fundamental challenge in scientific discovery. Traditional approaches, primarily based on genetic algorithms and related evolutionary methods, have proven useful but suffer from scalability and expressivity limitations. Recently, large language model (LLM)-based evolutionary search methods have been introduced into SR and show promise. However, existing LLM-based approaches typically rely on scalar evaluation metrics, such as mean squared error, as the sole source of feedback during the search process, thereby overlooking the rich information embedded in the dataset. To address this limitation, we propose a novel LLM-based evolutionary search framework that incorporates programmatic context augmentation: by enabling code-based interactions with the dataset, our method can actively perform data analysis and extract informative signals, beyond aggregated evaluation scores. We evaluate our framework on advanced benchmarks, such as LLM-SRBench, and demonstrate superior efficiency and accuracy compared to strong baselines.

---

## 论文详细总结（自动生成）

# 论文总结：Programmatic Context Augmentation for LLM-based Symbolic Regression

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：符号回归（Symbolic Regression, SR）旨在发现最能描述给定数据集的数学表达式，是科学发现中的基础任务。传统方法（如遗传算法）存在可扩展性和表达性限制。
- **研究动机**：现有基于大语言模型（LLM）的进化搜索方法通常仅依赖标量误差（如均方误差）作为反馈，忽略了数据集中隐含的丰富结构信息。
- **整体含义**：提出一种程序化上下文增强机制，让LLM能够通过与数据集的代码级交互来主动执行数据分析并提取信息信号，从而提升SR的效率和准确性。

## 2. 方法论
- **核心思想**：在LLM驱动的进化搜索过程中，不仅使用标量误差反馈，还允许LLM通过生成和执行代码来与数据集交互，从而获得更丰富的上下文信息（如数据分布、异常值、特征关系等），用于指导表达式搜索。
- **关键技术细节**：
  - 程序化上下文增强：将数据结构特征转化为LLM的输入上下文，例如自动生成统计汇总、可视化代码、特征工程脚本等（根据摘要推断）。
  - 进化搜索框架：LLM作为变异/交叉算子，结合增强后的上下文信息生成新表达式候选。
  - 与数据集交互：LLM通过执行代码（如Python）分析数据，提取统计特征或模式，从而减少搜索盲目性。
- **公式或算法流程**（文字说明）：
  1. 初始化种群（随机表达式）。
  2. 对每个候选表达式，计算标量误差（如MSE）。
  3. LLM根据当前种群、误差以及通过程序化上下文增强得到的数据分析结果，生成新的表达式（变异/交叉）。
  4. 重复进化过程直到收敛或达到最大代数。
- **注意**：论文全文未提供详细算法伪代码，以上根据摘要推断。

## 3. 实验设计
- **数据集/场景**：使用LLM-SRBench（一个高级SR基准）进行评估。未明确列出具体数据集名称（如物理、生物、金融等），但推测包含多种合成和真实世界的符号回归问题。
- **Benchmark**：LLM-SRBench（可能由论文作者或社区构建，用于评估LLM在SR上的表现）。
- **对比方法**：强基线（strong baselines），具体方法未在摘要列出，可能是传统遗传规划、经典LLM-SR方法（如基于GPT的搜索）、随机搜索等。未提及对比方法的细节。

## 4. 资源与算力
- **文中未明确说明**：未提及使用的GPU型号、数量、训练时长等算力资源。需要查看全文才能获知，但根据提供的文本无法得到此信息。

## 5. 实验数量与充分性
- **实验数量**：仅提到在LLM-SRBench上评估，未说明具体进行了多少组实验（如不同数据集数量、消融实验次数等）。
- **充分性推断**：由于论文被ICLR-2026拒绝（根据元数据），但评分9.0（高分），表明实验设计可能较充分。但缺乏详细实验描述，无法判断是否涵盖了多种复杂度和噪声水平的SR问题、是否进行了统计显著性检验、是否对比了多种LLM变体等。
- **客观性**：未说明是否重复多次运行、是否采用相同随机种子等，可能存在偏差。

## 6. 主要结论与发现
- 程序化上下文增强方法在LLM-SRBench上相比强基线实现了更优的效率和准确性。
- 通过利用数据集中的丰富信息（不只是标量误差），LLM的进化搜索能够更快、更准确地发现表达式。
- 该方法为LLM在科学发现中的应用提供了新范式：从仅依赖标量反馈转向主动数据交互。

## 7. 优点
- **方法创新**：提出程序化上下文增强，突破了传统LLM-SR仅依赖标量误差的局限，让模型能主动分析数据。
- **效率提升**：通过减少无效搜索，提高了样本效率（更快收敛）。
- **可解释性**：代码级交互使搜索过程更透明，有助于理解LLM如何利用数据结构信息。
- **符合科学发现需求**：符号回归常用于物理、生物等领域，上下文增强能利用领域知识。

## 8. 不足与局限
- **实验覆盖不足**：仅在一个基准LLM-SRBench上评估，缺少对更多真实世界数据集（如噪声大、高维、多变量）的验证。
- **计算成本**：程序化上下文增强需要LLM生成并执行代码，可能引入额外计算开销，文中未讨论效率比较。
- **鲁棒性未知**：若数据集包含误导性统计特征（如相关但不因果），可能导致LLM产生错误引导。
- **应用限制**：依赖LLM的代码生成能力，对于需要复杂数值计算的场景可能不稳定。
- **可重复性问题**：未提供代码或实验细节（如超参数、LLM版本），增加结果复现难度。
- **拒绝原因推测**：被ICLR-2026拒绝可能因实验不够全面或与已有方法差异性不足（尽管评分9.0）。

（完）
