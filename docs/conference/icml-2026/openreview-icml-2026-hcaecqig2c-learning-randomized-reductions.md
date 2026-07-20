---
title: Learning Randomized Reductions
title_zh: 学习随机化归约
authors: "Ferhat Erata, Orr Paradise, Thanos Typaldos, Timos Antonopoulos, ThanhVu Nguyen, Shafi Goldwasser, Ruzica Piskac"
date: 2026-04-30
pdf: "https://openreview.net/pdf/347cf5cad260da0034ae2a9c92c2465df2f41fa4.pdf"
tags: ["query:sr"]
score: 8.0
evidence: 使用符号回归作为学习随机化归约的后端
tldr: 针对随机化自归约（RSR）需人工推导的局限，提出Bitween系统集成符号回归、遗传编程等多种后端，自动学习RSR，在80个函数的基准上发现43个函数的归约，包括首个已知归约，展示了符号回归在算法发现中的潜力。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: RSR发现长期依赖人工，缺乏自动化方法。
method: 开发Bitween系统，集成符号回归、线性回归等后端自动学习RSR。
result: 在RSR-Bench基准上成功发现43/80函数的归约。
conclusion: 符号回归可自动化发现理论计算机科学中的归约关系。
---

## Abstract
Randomized self-reductions (RSRs) express $f(x)$ using $f$ evaluated at random correlated points, enabling self-correcting programs, instance-hiding protocols, and applications in complexity theory and cryptography. Yet discovering RSRs has required manual expert derivation for over 40 years, limiting their practical use.
We present Bitween for automated RSR learning. First, we formalize RSR learning with sample complexity analysis under correlated sampling. Second, we develop Vanilla Bitween, which integrates multiple backends (linear regression, genetic programming, symbolic regression, and mixed-integer programming). The linear regression backend outperforms the others, discovering RSRs for 43 of 80 functions (54%) in RSR-Bench, our benchmark suite, including the first known reduction for sigmoid. Third, we introduce Agentic Bitween, a neuro-symbolic approach where LLM agents propose novel query functions beyond the fixed set ($x+r$, $x-r$, $x \cdot r$, $x$, $r$) in prior work. Agentic Bitween discovers RSRs for 64 of 80 functions (80%), outperforming pure neural baselines in both RSR discovery and verification accuracy.

---

## 论文详细总结（自动生成）

# 论文《Learning Randomized Reductions》中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：随机化自归约（RSR）是一种重要的理论工具，它利用函数在随机相关点上的求值来表达原函数值，广泛应用于程序自纠正、实例隐藏协议以及复杂性理论和密码学。然而，过去40多年来，RSR的发现完全依赖人工专家推导，缺乏自动化方法，限制了其实际应用。
- **整体含义**：本文旨在开发自动化学习RSR的系统，从而降低人工门槛，推动RSR在更广泛场景中的实用化，并探索符号回归在算法发现中的潜力。

## 2. 论文提出的方法论：核心思想、关键技术细节与流程
- **核心思想**：将RSR的发现形式化为一个学习问题，通过采样分析保证样本复杂性，并利用多种后端自动搜索函数归约表达式。
- **关键技术细节**：
  - **形式化RSR学习**：定义给定目标函数f，寻找一组查询函数q₁(x,r),…,qₖ(x,r)（通常取x+r, x−r, x·r, x, r等），使得存在组合函数g满足f(x) = E_r[g(f(q₁),…,f(qₖ))]。学习算法从随机采样点中推断g。
  - **Vanilla Bitween**：集成多种后端：
    - 线性回归：对g进行线性组合假设。
    - 遗传编程：进化搜索表达式。
    - 符号回归（如Eureqa/PySR）：直接搜索符号表达式。
    - 混合整数规划：将问题转化为组合优化。
  - **Agentic Bitween**：神经符号方法，利用LLM智能体提出新颖的查询函数（超出固定集合{x+r, x−r, x·r, x, r}），从而扩展搜索空间。
- **算法流程（文字说明）**：
  1. 对目标函数f，定义基本查询集Q₀。
  2. 从域中采样若干随机点x和随机种子r，计算f在相关点上的值。
  3. 使用后端（如线性回归）拟合组合函数g，使f(x) ≈ E_r[g(…)]。
  4. 验证学习到的RSR是否对所有测试点成立（通过统计检验）。
  5. 在Agentic Bitween中，LLM根据初步结果建议新的查询函数，迭代扩展Q₀，重复上述步骤。

## 3. 实验设计：数据集、benchmark、对比方法
- **数据集 / benchmark**：构造了RSR-Bench，包含80个不同函数（包括多项式、指数、三角函数、逻辑函数等），覆盖多种数学性质。
- **对比方法**：
  - Vanilla Bitween的四个后端之间相互对比（线性回归、遗传编程、符号回归、混合整数规划）。
  - Agentic Bitween与纯神经基线（如直接使用神经网络预测归约结构）对比，比较RSR发现率和验证准确率。
- **评估指标**：发现归约的函数数量（发现率）以及验证成功率。

## 4. 资源与算力
- 论文中未明确说明使用的GPU型号、数量或训练时长。仅提到“bitween”系统为CPU可运行（符号回归和遗传编程通常不依赖GPU），但Agentic Bitween可能调用LLM API，未说明具体算力消耗。因此无法提供具体资源信息。

## 5. 实验数量与充分性
- **实验数量**：
  - Vanilla Bitween：在80个函数上测试了4种后端，共320组实验（每个后端分别测试80个函数）。
  - Agentic Bitween：同样在80个函数上测试，并与纯神经基线对比；文中还报告了消融实验（如去掉LLM提案的变体），但未给出具体组数。
- **充分性与客观性**：
  - 基准函数多样，覆盖主要数学类型，实验规模中等。
  - 对比了多种方法，包括传统回归和机器学习方法，公平性较好。
  - 缺少对实际应用场景（如密码学协议）的实证验证，仅停留在归约发现层面。

## 6. 论文的主要结论与发现
- 线性回归后端在Vanilla Bitween中表现最佳，成功发现43/80个函数的RSR（54%），包括首个已知的sigmoid函数归约。
- Agentic Bitween通过LLM扩展查询空间，将发现率提升至64/80（80%），在发现和验证准确率上均优于纯神经基线。
- 符号回归（及遗传编程）在RSR学习中具有潜力，但需要更智能的搜索引导。
- 自动化RSR发现是可实现的，且能发现人工未发现的归约关系。

## 7. 优点：方法或实验设计上的亮点
- **方法创新**：首次将RSR发现转化为机器学习问题，并集成多种后端形成统一框架。
- **混合策略**：Agentic Bitween结合LLM的常识推理与符号回归的精确表达，拓展了搜索空间，克服了固定查询集合的局限。
- **基准构建**：建立RSR-Bench，为后续研究提供了标准评估平台。
- **可解释性**：学习到的RSR是显式数学表达式，而非黑箱模型，便于理论分析。

## 8. 不足与局限
- **实验覆盖**：仅测试了数学函数，未在真实密码学或复杂度理论应用（如实例隐藏协议）中验证实用性。
- **偏差风险**：RSR-Bench中的函数可能偏向某些类型（如多项式、单变量），缺乏多变量复杂函数。
- **验证方法**：仅通过采样点统计验证，未提供严格形式化证明，存在假阳性风险。
- **算力需求**：未评估大规模函数或高维空间下的可扩展性；LLM Agent的调用成本未量化。
- **泛化性**：对不可微、不连续函数或随机性强的函数效果未知。

（完）
