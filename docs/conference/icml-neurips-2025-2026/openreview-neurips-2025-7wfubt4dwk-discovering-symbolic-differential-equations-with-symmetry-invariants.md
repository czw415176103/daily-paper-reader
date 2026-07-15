---
title: Discovering Symbolic Differential Equations with Symmetry Invariants
title_zh: 利用对称不变量发现符号微分方程
authors: "Jianke Yang, Manu Bhat, Bryan Hu, Yadi Cao, Nima Dehmamy, Robin Walters, Rose Yu"
date: 2025-04-16
pdf: "https://openreview.net/pdf?id=7WfubT4dwK"
tags: ["query:sr"]
score: 10.0
evidence: 利用对称不变量发现符号微分方程
tldr: 针对现有符号方程发现方法搜索空间大且可能违背物理规律的问题，提出利用对称不变量作为原子实体。通过将微分方程表示为对称变换的微分不变量，确保发现的方程自动满足指定对称性。该方法可无缝集成到现有方程发现管道中，实验表明在多个动力学系统上发现了更简洁且物理一致的方程，为从数据发现可解释物理模型提供了新路径。
source: NeurIPS-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 符号方程发现面临搜索空间爆炸和可能违反物理定律的问题。
method: 利用对称群的微分不变量作为原子实体，约束方程形式以满足对称性。
result: 在多个动力学系统上发现了更简洁且符合物理对称性的微分方程。
conclusion: 对称不变量显著提升了符号方程发现的效率和物理一致性。
---

## Abstract
Discovering symbolic differential equations from data uncovers fundamental dynamical laws underlying complex systems. However, existing methods often struggle with the vast search space of equations and may produce equations that violate known physical laws.
In this work, we address these problems by introducing the concept of \textit{symmetry invariants} in equation discovery. We leverage the fact that differential equations admitting a symmetry group can be expressed in terms of differential invariants of symmetry transformations. Thus, we propose to use these invariants as atomic entities in equation discovery, ensuring the discovered equations satisfy the specified symmetry. Our approach integrates seamlessly with existing equation discovery methods such as sparse regression and genetic programming, improving their accuracy and efficiency. We validate the proposed method through applications to various physical systems, such as fluid and reaction-diffusion, demonstrating its ability to recover parsimonious and interpretable equations that respect the laws of physics.

---

## 论文详细总结（自动生成）

# 论文总结：利用对称不变量发现符号微分方程

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：从数据中发现符号微分方程是揭示复杂系统动态规律的关键手段，但现有方法面临两大挑战：一是搜索空间巨大，导致效率低下；二是可能产生违背已知物理定律的方程，例如不满足守恒律或对称性。
- **背景**：物理系统中普遍存在对称性（如平移对称、旋转对称），微分方程若服从某对称群，则可用该对称变换的微分不变量表示。以往方法未充分利用这一先验知识，本工作旨在利用对称不变量约束方程形式，提升发现结果的物理一致性和搜索效率。

## 2. 方法论
- **核心思想**：将微分方程的表达约束在对称群的微分不变量（differential invariants）所张成的空间中。即：先根据指定对称性计算出所有相关的微分不变量，然后在这些不变量构成的基底下进行符号回归或稀疏回归，从而保证最终方程自动满足对称性。
- **关键技术细节**：
  - 给定一个微分方程系统，若其允许一个李群对称性，则方程可化简为关于微分不变量的关系式。
  - 预先计算对称群的微分不变量集合（例如通过Lie点对称求法或已知的微分不变量基）。
  - 将微分不变量作为“原子实体”（atomic entities）代替原始变量及其导数，融入现有方程发现流程（如稀疏回归、遗传编程）。
  - 在搜索过程中，只允许使用这些不变量构造方程，从而极大缩小搜索空间，并确保对称性自动成立。
- **算法流程概括**：
  1. 输入：时间序列观测数据、指定对称群（如平移、旋转、缩放等）。
  2. 利用李群理论计算该对称群的微分不变量（包括零阶、一阶等不变量）。
  3. 将数据转换到不变量空间：从原始变量计算不变量值。
  4. 应用方程发现方法（如SINDy的变体、遗传编程）在不变量空间中搜索方程。
  5. 输出：用不变量表示的符号微分方程，可逆变换回原始变量。

## 3. 实验设计
- **数据集/场景**：论文提及流体（fluid）和反应-扩散（reaction-diffusion）等物理系统。具体包括哪些具体方程（如Navier-Stokes？Fisher方程？）未在摘要中详述，需要查看原文。
- **Benchmark**：无明确提及标准基准数据集。
- **对比方法**：与现有符号回归方法（如稀疏回归、遗传编程等）对比，本方法通过集成对称不变量提升准确性和简洁性。
- **评估指标**：可能包括方程恢复成功率、预测误差、方程复杂度等，摘要未列出具体数值。

## 4. 资源与算力
- 论文未明确说明实验所使用的GPU型号、数量、训练时长等算力信息。推测由于主要涉及符号回归和数值优化，算力需求可能较低，但未提供细节。

## 5. 实验数量与充分性
- **实验数量**：至少涵盖两个系统（流体和反应-扩散），可能包括多个具体方程变体。未见详细消融实验或统计显著性检验的表述。
- **充分性**：由于摘要信息有限，难以判断实验是否充分。目前仅展示了在特定系统上的定性能力，缺乏大规模基准对比和统计结果，实验覆盖面和公平性有待原文验证。

## 6. 主要结论与发现
- 利用对称不变量作为原子实体，能使发现的方程自动满足指定对称性，从而保证物理一致性。
- 该方法可无缝集成到现有方程发现管道中，提升准确性和效率。
- 在流体和反应-扩散系统中，恢复了比传统方法更简洁（parsimonious）且可解释的方程，且符合物理定律。

## 7. 优点
- **方法创新**：将对称约束直接嵌入方程发现原子单元，而非后处理校验，从根本上避免了非法方程。
- **兼容性**：不改变现有发现算法主干，容易作为插件模块使用，实用性强。
- **物理一致性**：自动满足对称性，减少人工筛选的麻烦。

## 8. 不足与局限
- **实验覆盖有限**：仅用两个系统验证，未在大规模基准（如Burgers方程、混沌系统等）或其他对称性场景评估。
- **对称性假设的局限性**：需要预先知道系统的对称群，若对称群未知或不完全，方法可能失效。
- **计算复杂性**：计算微分不变量本身可能涉及复杂符号运算，对高维系统或高导数阶数可能代价高，文中未讨论。
- **缺乏消融与统计检验**：没有定量分析不同不变量选择的影响，也未提供多次运行的平均结果。

（完）
