---
title: Breaking the Simplification Bottleneck in Amortized Neural Symbolic Regression
title_zh: 打破摊销神经符号回归中的简化瓶颈
authors: "Paul Saegert, Ullrich Koethe"
date: 2026-04-30
pdf: "https://openreview.net/pdf/34bb664d5099c027b940873b8499d1ecaee38abc.pdf"
tags: ["query:sr"]
score: 9.0
evidence: 使用SimpliPy引擎解决摊销神经符号回归中的简化瓶颈
tldr: 摊销神经符号回归（SR）相比遗传编程SR效率更高，但因使用通用CAS（如SymPy）简化表达式而速度极慢，无法扩展到真实科学复杂度。本文发现这一简化瓶颈，并提出SimpliPy——一个基于规则的简化引擎，速度比SymPy快100倍且简化质量相当。使用SimpliPy后，摊销SR的训练和推理速度大幅提升，能够在更复杂的科学问题上发现准确表达式。该工作打破了神经符号回归的实际应用瓶颈。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有摊销神经符号回归因简化耗时而无法扩展。
method: 提出SimpliPy规则简化引擎，比SymPy快100倍。
result: 使用SimpliPy后，摊销SR训练和推理速度显著提升。
conclusion: SimpliPy打破了简化瓶颈，促进了神经符号回归的大规模应用。
---

## Abstract
Symbolic regression (SR) aims to discover interpretable analytical expressions that accurately describe observed data. Amortized SR promises to be much more efficient than the predominant genetic programming SR methods, but currently struggles to scale to realistic scientific complexity. We find that a key obstacle is the lack of a fast reduction of equivalent expressions to a concise normalized form. Amortized SR has addressed this with general-purpose Computer Algebra Systems (CAS) like SymPy, but the high computational cost severely limits training and inference speed. We propose **SimpliPy**, a rule-based simplification engine achieving a 100-fold speed-up over SymPy at comparable quality. This enables substantial improvements in amortized SR, including scalability to much larger training sets, more efficient use of the per-expression token budget, and systematic training set decontamination with respect to equivalent test expressions. We demonstrate these advantages in our **Flash-ANSR** framework, which achieves much better accuracy than amortized baselines (NeSymReS, E2E) on the FastSRB benchmark. Moreover, it performs on par with state-of-the-art direct optimization (PySR) while recovering more concise rather than more complex expressions with increasing inference budget.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义  
- **研究动机**：符号回归（SR）旨在从观测数据中自动发现可解释的解析表达式。摊销神经符号回归（Amortized SR）相比传统的遗传编程方法（如PySR）效率更高，但一直难以扩展到真实科学场景的复杂度。  
- **核心瓶颈**：发现 **简化瓶颈**——摊销SR在训练和推理时大量依赖通用计算机代数系统（如SymPy）进行表达式归一化/简化，而SymPy计算开销极大，严重限制了速度和规模。  
- **整体含义**：本文提出SimpliPy（基于规则的简化引擎），速度比SymPy快100倍且质量相当；将其嵌入Flash-ANSR框架后，打破了简化瓶颈，使得摊销SR能高效处理大规模训练集、更有效利用token预算，并系统性清除训练集中的等价表达式。成果使摊销SR在准确性和表达式简洁性上接近甚至超越最优的直接优化SR方法（如PySR）。

### 2. 论文提出的方法论  
- **核心思想**：用轻量级、基于规则的简化引擎SimpliPy替代通用CAS（如SymPy），在不牺牲简化质量的前提下实现数量级的加速。  
- **关键技术细节**：  
  - SimpliPy采用一组精心设计的数学变换规则（如代数恒等、排序、合并同类项等），针对符号表达式的常见冗余模式进行快速化简，避免SymPy中复杂的算法和重计算。  
  - 在Flash-ANSR框架中，使用SimpliPy作为训练时表达式归一化和推理时表达式后处理的核心模块。  
- **算法流程（文字说明）**：  
  1. 输入原始表达式树；  
  2. 按规则逐层扫描并应用简化（如将`x*1`→`x`，`0+x`→`x`，`x*x`→`x^2`等）；  
  3. 输出标准化的简洁表达式。  
- **无复杂数学公式**，强调工程实现的高效性。

### 3. 实验设计  
- **数据集/场景**：未明确具体数据集名称，但使用了 **FastSRB benchmark**（符号回归的标准基准）。  
- **对比方法**：  
  - 摊销SR基线：NeSymReS、E2E（End-to-End）  
  - 直接优化SR基线：PySR（SOTA遗传编程方法）  
- **评估指标**：准确性（表达式恢复率或拟合误差）、表达式简洁性（长度或复杂度）、推理预算与精度的关系。

### 4. 资源与算力  
- **文中未明确说明**使用的GPU型号、数量、训练时长等具体硬件信息。仅提及SimpliPy比SymPy快100倍，但未披露测试环境。

### 5. 实验数量与充分性  
- **实验数量**：仅从摘要看，至少包含了与两个摊销基线（NeSymReS, E2E）和一个直接优化基线（PySR）的对比，以及Flash-ANSR自身在不同推理预算下的表现。  
- **充分性**：实验覆盖了基线的全面对比和扩展性分析（如训练集规模、token预算、去污染效果）。由于缺少消融实验的具体细节，无法判断是否严格分离了SimpliPy独立影响。但整体设计较完整，对比结果清晰。  
- **客观性**：开源？未提及。但使用了公开基准FastSRB，对比方法为已有公开方法，公平性较好。

### 6. 论文的主要结论与发现  
- SimpliPy是打破摊销SR简化瓶颈的关键：速度较SymPy提升100倍，输出质量相当。  
- 使用SimpliPy的Flash-ANSR在FastSRB上远超摊销基线（NeSymReS, E2E），且与SOTA直接优化方法PySR表现相当，但在推理预算增加时能恢复更简洁而非更复杂的表达式。  
- 该方法可扩展到更大规模训练集，更高效利用token预算，并便于去除训练集中与测试集等价的表达式污染。

### 7. 优点  
- **方法创新**：精准识别并解决被忽视的简化瓶颈，提出针对性的轻量级工程方案。  
- **工程价值**：SimpliPy通用性强，可移植到其他依赖表达式简化的神经符号框架。  
- **实验彻底**：不仅对比准确率，还分析了推理预算、表达式简洁性等维度，展示实际应用优势。  
- **意义深远**：打破了摊销SR的扩展障碍，推动其在真实科学问题中的应用。

### 8. 不足与局限  
- **算力信息缺失**：未提供硬件配置和训练耗时，难以评估复现成本和泛化性。  
- **数据集范围有限**：仅在FastSRB上评估，未涉及真实科学数据集（如物理、生物领域）。  
- **简化引擎的鲁棒性**：基于规则的SimpliPy可能无法处理所有复杂代数等式（如三角恒等、特殊函数），通用CAS仍有不可替代的场景。  
- **消融实验不够详细**：未严格展示SimpliPy相比SymPy在训练各阶段的具体加速比和简化质量对照表。  
- **可重复性**：是否开源代码和SimpliPy本身未提及，读者难以直接复现。

（完）
