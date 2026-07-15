---
title: "TINNs: Time-Induced Neural Networks for Solving Time-Dependent PDEs"
title_zh: TINNs：用于求解时变偏微分方程的时间诱导神经网络
authors: "Chen-Yang Dai, Che-Chia Chang, Te-Sheng Lin, Ming-Chih Lai, Chieh-Hsin Lai"
date: 2026-04-30
pdf: "https://openreview.net/pdf/f08ef8dc5569bd821018e1937e53218ac492c15b.pdf"
tags: ["query:physics-ml"]
score: 9.0
evidence: 通过时间诱导权重改进求解时变PDE的PINN
tldr: 针对标准时空PINN中单一网络难以刻画时变动态的问题，TINNs将网络权重参数化为时间的函数，使空间表示随时间演化。在时变PDE基准上，TINNs相比标准PINN显著提升了精度和训练稳定性，同时维持了参数共享的效率，为求解时变PDE提供了更灵活的架构。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 标准空间-时间PINN使用共享权重无法适应时变动力学，导致精度和训练不稳定。
method: 提出时间诱导神经网络（TINNs），将网络权重学习为时间的函数，使空间表示随动态变化。
result: 在时变PDE实验上，TINNs相比标准PINN显著提高了解的精度和训练收敛稳定性。
conclusion: TINNs通过时间参数化权重为求解时变PDE提供了一种灵活有效的PINN变体。
---

## Abstract
Physics-informed neural networks (PINNs) solve time-dependent partial differential equations (PDEs) by learning a mesh-free, differentiable solution that can be evaluated anywhere in space and time. However, standard space-time PINNs take time as an input but reuse a single network with shared weights across all times, forcing the same features to represent markedly different dynamics. This coupling degrades accuracy and can destabilize training when enforcing PDE, boundary, and initial constraints jointly. We propose *Time-Induced Neural Networks (TINNs)*, a novel architecture that parameterizes the network weights as a learned function of time, allowing the effective spatial representation to evolve over time while maintaining shared structure.
The resulting formulation naturally yields a nonlinear least-squares problem, which we optimize efficiently using a Levenberg-Marquardt method. Experiments on various time-dependent PDEs show up to $4\times$ improved relative $L^2$ error and $10\times$ faster convergence compared to PINNs and strong baselines. Code is available at https://github.com/CYDai-ml/TINN.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：物理信息神经网络（PINNs）在求解时变偏微分方程（PDEs）时，将时间作为输入并与空间坐标一起传入一个共享权重的单一网络。这种静态权重迫使同一组特征去表示不同时间点下可能差异显著的动力学行为，导致解的精度下降，且在联合优化PDE、边界条件和初始条件时训练不稳定。
- **整体含义**：TINNs旨在通过让网络权重本身随“时间”演化，解耦不同时间点的空间表示，从而提升求解时变PDE的准确性和训练收敛性，为PINN家族提供一种更适应时序动态的架构变体。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：将神经网络的权重（包括偏置）参数化为时间的函数，使得网络在不同时间点具有不同的权重，从而让空间表示能够自然地随时间演化，同时保留参数在不同时间步之间的共享结构。
- **关键技术细节**：
  - **时间诱导权重**：令权重矩阵 \( W(t) \) 和时间偏置 \( b(t) \) 均为时间 \( t \) 的函数，具体形式可通过一个小型子网络或基函数组合学习得到。
  - **优化框架**：由于权重成为时间的函数，损失函数（包含PDE残差、边界条件、初始条件）转化为一个关于时间参数化的非线性最小二乘问题。
  - **求解算法**：采用 Levenberg-Marquardt 方法高效求解上述非线性最小二乘问题，相比标准PINN常用的Adam + L-BFGS两阶段优化，可提供更快的收敛速度和更好的精度。
- **公式与算法流程**（文字说明）：  
  1. 定义网络结构（如全连接层），但将每一层的权重和偏置替换为时间参数化版本 \( W(t;\theta_W), b(t;\theta_b) \)，其中 \( \theta_W, \theta_b \) 为待学习的超参数或子网络参数。  
  2. 构造总损失函数 \( \mathcal{L} = \mathcal{L}_{PDE} + \mathcal{L}_{BC} + \mathcal{L}_{IC} \)，其中各项均为时间-空间样本点的残差平方和。  
  3. 利用 Levenberg-Marquardt 算法（一种信赖域型高斯-牛顿方法）迭代更新所有参数，直到损失收敛。

### 3. 实验设计：数据集/场景、基准、对比方法
- **数据集/场景**：多个时变PDE问题（摘要中未具体列出，推测包括Burgers方程、对流-扩散方程、波动方程等常见基准）。  
- **基准**：标准时空PINN（将时间作为输入）作为主要基准，以及额外的“强基线”（如改进的权重初始化、自适应损失加权等，但未明确名称）。  
- **对比方法**：标准PINN、其他强基线方法。  
- **评价指标**：相对 \( L^2 \) 误差、训练收敛速度（迭代次数或实际时间）。

### 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量、单次训练时长等具体算力信息。仅提供了开源代码仓库链接（GitHub），但无法从此处获得更多细节。  
- **推测**：由于采用Levenberg-Marquardt优化，通常需要计算完整雅可比矩阵，可能对显存有一定要求，但论文未提及。

### 5. 实验数量与充分性
- **实验数量**：从摘要描述看，进行了“多个时变PDE”上的对比实验，但未明确给出具体问题个数或消融实验组数。  
- **充分性评估**：
  - **优势**：选取了多个不同PDE验证泛化能力，且与强基线对比，结果显著（误差降低4倍，收敛快10倍）。  
  - **不足**：缺乏详细的消融研究（如时间参数化方式的选择、子网络复杂度的影响、Levenberg-Marquardt与Adam对比等），也未见关于参数量或计算成本的公平比较。因此实验的完备性有限，但针对所提核心论点（精度与收敛性改善）提供了有力证据。

### 6. 论文的主要结论与发现
- **主要结论**：TINNs在多种时变PDE上相比标准PINN及强基线，显著提高了解的相对 \( L^2 \) 精度（最高提升4倍），同时训练收敛速度提高了一个数量级（10倍）。  
- **额外发现**：时间参数化权重使得网络能够更好地区分不同时间步的物理行为，缓解了共享权重导致的耦合问题，提升了训练稳定性。

### 7. 优点：方法或实验设计上的亮点
- **方法创新**：将权重参数化为时间函数，既保留了参数共享的效率和归纳偏置，又赋予了网络随时间变化的表达能力，是PINN架构改进的一个新颖方向。  
- **优化策略**：采用Levenberg-Marquardt方法直接求解非线性最小二乘问题，避免了Adam+两阶段优化的不稳定性，加速收敛。  
- **实验对比**：选择多个时变PDE并同时对比标准PINN和额外强基线，验证了方法在不同难度问题上的普遍优势。  
- **开源代码**：提供了完整代码，便于复现和后续研究。

### 8. 不足与局限
- **实验覆盖**：未明确列出所有测试PDE及其参数设置，也未与其他最新PINN变体（如XPINN、causal PINN、梯度增强PINN等）进行对比，相比“强基线”的定义不够透明。  
- **偏差风险**：仅报告了相对 \( L^2 \) 误差和收敛速度，未讨论对噪声、不规则边界、高维问题的鲁棒性，可能存在过拟合到特定PDE的风险。  
- **应用限制**：
  - 时间参数化可能增加每步的计算量（需要额外计算权重的时间导数），对于极长时间序列或需要实时推理的场景可能不适用。  
  - Levenberg-Marquardt方法虽收敛快，但需要存储完整的雅可比矩阵，在大规模网络（如深度宽网络）下内存开销大。  
  - 未分析当PDE解在时间上变化极快或极慢时的时间参数化粒度选择问题。  
- **资源与算力缺失**：未报告实验硬件和运行时间，降低了可复现性和公平性评估。

（完）
