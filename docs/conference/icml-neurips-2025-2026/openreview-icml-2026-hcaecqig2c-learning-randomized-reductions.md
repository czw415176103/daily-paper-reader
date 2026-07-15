---
title: Learning Randomized Reductions
title_zh: 学习随机自归约
authors: "Ferhat Erata, Orr Paradise, Thanos Typaldos, Timos Antonopoulos, ThanhVu Nguyen, Shafi Goldwasser, Ruzica Piskac"
date: 2026-04-30
pdf: "https://openreview.net/pdf/347cf5cad260da0034ae2a9c92c2465df2f41fa4.pdf"
tags: ["query:sr"]
score: 4.0
evidence: 使用符号回归作为学习随机自归约的后端之一
tldr: 随机自归约（RSR）的发现依赖人工推导超过40年。Bitween自动化学习RSR，集成了线性回归、遗传编程、符号回归和混合整数规划等多个后端。其中符号回归虽不是最优，但成功发现了43/80函数的新RSR。该工作主要贡献在RSR学习框架，符号回归仅是工具。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 随机自归约的发现长期依赖人工，缺乏自动化方法。
method: 提出Bitween框架，集成多种回归方法（包括符号回归）来自动学习RSR。
result: 在RSR-Bench上，线性回归后端表现最佳，发现43个函数的新归约。
conclusion: 自动化RSR学习可行，符号回归作为工具之一有效但非核心。
---

## Abstract
Randomized self-reductions (RSRs) express $f(x)$ using $f$ evaluated at random correlated points, enabling self-correcting programs, instance-hiding protocols, and applications in complexity theory and cryptography. Yet discovering RSRs has required manual expert derivation for over 40 years, limiting their practical use.
We present Bitween for automated RSR learning. First, we formalize RSR learning with sample complexity analysis under correlated sampling. Second, we develop Vanilla Bitween, which integrates multiple backends (linear regression, genetic programming, symbolic regression, and mixed-integer programming). The linear regression backend outperforms the others, discovering RSRs for 43 of 80 functions (54%) in RSR-Bench, our benchmark suite, including the first known reduction for sigmoid. Third, we introduce Agentic Bitween, a neuro-symbolic approach where LLM agents propose novel query functions beyond the fixed set ($x+r$, $x-r$, $x \cdot r$, $x$, $r$) in prior work. Agentic Bitween discovers RSRs for 64 of 80 functions (80%), outperforming pure neural baselines in both RSR discovery and verification accuracy.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：随机自归约（Randomized Self-Reductions, RSRs）能够用函数在随机相关点上的求值表达 \( f(x) \)，从而支持程序自矫正、实例隐藏协议以及在复杂性理论和密码学中的应用。然而，在过去40多年中，RSR的发现完全依赖人工专家手工推导，限制了其实际应用。
- **整体含义**：该论文旨在首次自动化RSR的学习过程，提出一个名为Bitween的框架，通过集成多种回归方法，自动从函数实例中学习出有效的随机自归约。这项工作将RSR的发现从人工劳动转向机器学习驱动，具有重要的理论和实践意义。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将RSR学习形式化为一个回归问题，即给定目标函数 \( f \) 以及一组允许的查询函数（如 \( x+r \), \( x-r \), \( x \cdot r \), \( x \), \( r \) 等），学习一组系数和函数组合方式，使得 \( f(x) \) 可以用这些查询的线性组合（或更复杂的表达式）在随机性下近似表达。
- **关键技术细节**：
  - **Vanilla Bitween**：集成多个后端进行回归，包括线性回归、遗传编程、符号回归和混合整数规划。每个后端独立学习RSR，然后通过验证集筛选出最有效的归约。
  - **Agentic Bitween**：引入神经符号方法，利用LLM（大语言模型）智能体提出超越固定集合的新型查询函数（例如 \( \sin(x+r) \), \( \exp(x) \) 等），从而扩展搜索空间，提升发现能力。该方法结合了符号搜索的精确性和神经网络的灵活性。
- **算法流程**（文字描述）：
  1. 输入一系列 \( (x, f(x)) \) 样本点，以及一组候选查询函数。
  2. 对每个候选查询函数，在不同的随机种子 \( r \) 下计算查询值，构造特征矩阵。
  3. 使用后端回归方法（如线性回归）学习系数，得到候选归约表达式。
  4. 通过验证集评估归约的准确性（例如比较 \( f(x) \) 与归约表达式的输出）。
  5. Agentic Bitween中，LLM代理分析失败案例，生成新的查询函数，然后重复上述步骤。

## 3. 实验设计：数据集 / 场景 / Benchmark / 对比方法

- **Benchmark**：作者创建了RSR-Bench，包含80个常用函数（例如 sigmoid, ReLU, 多项式等），覆盖了多种常见数学函数。
- **实验场景**：
  - 学习从函数到其随机自归约的映射。
  - 验证发现的RSR是否能在新的随机样本上正确工作。
- **对比方法**：
  - 四个后端（线性回归、遗传编程、符号回归、混合整数规划）之间相互比较。
  - 纯神经网络基线（文献中提及但未具体命名，可能指直接使用神经网络拟合RSR）。
  - Agentic Bitween与Vanilla Bitween对比。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量或训练时长。仅在“资源”部分可能提及？但根据提供的文本，没有相关信息。推测实验运行在标准CPU/GPU集群上，但具体细节缺失。

## 5. 实验数量与充分性

- **实验数量**：主要对比实验是在RSR-Bench的80个函数上进行的。Vanilla Bitween线性回归后端发现了43个函数的新归约（成功率54%）；Agentic Bitween发现了64个新归约（80%）。此外，可能还有消融实验（例如比较不同后端、是否使用LLM代理等），但文本中未详细列举。
- **充分性**：实验覆盖了80个函数，种类较为多样；对比了多种后端和整体框架，验证效果。但缺少对更大规模或更复杂函数（如高维、复合函数）的测试，也缺少对不同随机分布鲁棒性的分析。总体而言，实验设计合理，但充分性中等。

## 6. 论文的主要结论与发现

- **自动化RSR学习可行**：首次证明可以通过机器学习方法自动发现随机自归约，无需人工专家干预。
- **线性回归后端最优**：在固定查询函数集合下，线性回归后端表现最佳，发现了43/80个函数的新归约。
- **Agentic Bitween显著提升**：引入LLM代理提出新型查询函数后，发现率从54%提升至80%，超过了纯神经网络基线，并且在归约发现和验证准确性上均更优。
- **符号回归有效但非核心**：符号回归作为后端之一成功发现了43个函数的新归约，但整体性能不如线性回归，表明在RSR学习中，简单的线性模型可能就足够强大。
- **首次发现sigmoid的归约**：线性回归后端首次自动找到了sigmoid函数的随机自归约。

## 7. 优点：方法或实验设计上的亮点

- **问题新颖且重要**：首次将自动化机器学习应用于随机自归约发现，填补了领域空白。
- **框架灵活可扩展**：Bitween设计为模块化，可轻松集成新的后端或新的查询函数生成器（如LLM）。
- **Agentic设计创新**：利用LLM的创造力超越固定查询集合，是一种有效的神经符号融合方法。
- **基准构建**：发布了RSR-Bench，为后续研究提供标准化测试集。
- **结果令人信服**：在两个版本的Bitween中均取得可观成功率，且线性回归的简单方法取得了意外好效果，揭示了问题可能的性质。

## 8. 不足与局限

- **实验覆盖有限**：仅测试了80个函数，且均为初等数学函数，未涉及高维、复合或实际应用中的复杂函数（如深度神经网络中的激活函数组合）。
- **泛化性未证明**：学习到的归约是否对任意输入分布和随机种子都有效？论文可能仅展示了在特定分布下的验证结果。
- **计算资源细节缺失**：未报告训练时间、模型大小等，难以评估实际成本。
- **对RSR理论贡献有限**：方法偏向工程实践，未深入分析归约的数学性质或保证。
- **LLM依赖**：Agentic Bitween依赖LLM，可能受限于LLM的偏差和知识边界，且推理成本较高。
- **公平性**：对比的基线可能不够充分，例如未与专门的符号回归工具（如PySR）或更复杂的神经符号系统进行对比。

（完）
