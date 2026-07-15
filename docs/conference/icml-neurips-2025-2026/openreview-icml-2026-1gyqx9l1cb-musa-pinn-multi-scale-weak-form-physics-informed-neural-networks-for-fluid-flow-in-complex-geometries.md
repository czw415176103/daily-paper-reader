---
title: "MUSA-PINN: Multi-scale Weak-form Physics-Informed Neural Networks for Fluid Flow in Complex Geometries"
title_zh: MUSA-PINN：复杂几何中流体流动的多尺度弱形式物理信息神经网络
authors: "Weizheng Zhang, Xunjie Xie, Hao Pan, Xiaowei Duan, Bingteng Sun, Qiang Du, Lin Lu"
date: 2026-04-30
pdf: "https://openreview.net/pdf/56a71dd9cd3a308cbfcf2275eaddb34453668220.pdf"
tags: ["query:physics-ml"]
score: 8.0
evidence: 多尺度弱形式物理信息神经网络用于复杂几何流体流动
tldr: 该论文针对PINN在复杂拓扑（如三周期极小曲面）中收敛困难的问题，提出多尺度弱形式PINN（MUSA-PINN）。将纳维-斯托克斯方程约束重新表述为分层球面控制体积上的积分守恒律，通过通量平衡残差强制执行连续性和动量守恒。在TPMS等复杂几何中，MUSA-PINN显著改善梯度稳定性和守恒律满足度。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 标准PINN在复杂几何中因局部点约束导致梯度不稳定和守恒律违反。
method: 将PDE约束转化为分层球面控制体积上的积分守恒律，多尺度弱形式强制守恒。
result: 在三周期极小曲面等复杂几何上，MUSA-PINN收敛更稳定且守恒律精度高。
conclusion: 多尺度弱形式有效解决了PINN在复杂几何中的收敛问题。
---

## Abstract
While Physics-Informed Neural Networks (PINNs) offer a mesh-free approach to solving fluid-flow PDEs, standard point-wise residual minimization suffers from convergence pathologies in topologically complex domains like Triply Periodic Minimal Surfaces (TPMS). The locality bias of point-wise constraints fails to propagate global information through tortuous channels, causing unstable gradients and conservation violations. 
To address this, we propose the Multi-scale Weak-form PINN (MUSA-PINN), which reformulates Navier-Stokes equation constraints as integral conservation laws over hierarchical spherical control volumes. 
We enforce continuity and momentum conservation via flux-balance residuals on control surfaces. Our method utilizes a three-scale subdomain strategy-comprising large volumes for long-range coupling, skeleton-aware meso-scale volumes aligned with transport pathways, and small volumes for local refinement-alongside a two-stage training schedule prioritizing continuity. Experiments on steady incompressible flow in TPMS geometries show MUSA-PINN outperforms state-of-the-art baselines, reducing relative errors by up to 93\% and preserving mass conservation.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

*   **核心问题**：传统物理信息神经网络（PINN）在解决复杂拓扑几何（如三周期极小曲面，TPMS）中的流体流动PDE时，由于采用逐点残差最小化，局部约束的偏差导致全局信息无法在曲折通道中传播，进而引发梯度不稳定和质量守恒违反，收敛困难。
*   **研究动机**：现有PINN在复杂几何中泛化能力不足，需要一种能够强制全局守恒律且梯度稳定的方法。
*   **整体含义**：提出多尺度弱形式PINN（MUSA-PINN），通过将纳维-斯托克斯方程约束转化为分层球面控制体积上的积分守恒律，利用通量平衡残差同时强制连续性和动量守恒，显著提升在复杂几何中的收敛精度和守恒律满足度。

### 2. 论文提出的方法论：核心思想、关键技术细节

*   **核心思想**：将PDE的强形式点约束替换为多个不同尺度控制体积上的弱形式积分约束，通过多尺度分层强制全局守恒，从而克服逐点约束的局部性偏差。
*   **关键技术细节**：
    *   **多尺度弱形式**：在分层球面控制体积上建立积分守恒律，包括体积积分和表面通量积分。
    *   **三尺度子域策略**：
        *   **大体积**：用于长程耦合，捕捉全局流动特征。
        *   **骨架感知的中尺度体积**：沿传输通道对齐，适应几何拓扑结构。
        *   **小体积**：用于局部精细化，处理边界和细节。
    *   **两阶段训练调度**：优先强制连续性（质量守恒）的残差，再优化动量方程，避免训练初期梯度失衡。
    *   **通量平衡残差**：在控制表面计算通量偏差作为损失项，显式确保守恒律。

### 3. 实验设计

*   **数据集/场景**：复杂几何中的稳态不可压缩流动，具体使用三周期极小曲面（TPMS）几何结构（如Gyroid、Schwarz P等）。
*   **Benchmark**：与最先进的PINN基线方法对比（标准PINN、可能包括其他弱形式或混合方法）。
*   **对比方法**：未在摘要中列出具体名称，但明确“outperforms state-of-the-art baselines”，推测包括标准PINN、加权PINN或域分解PINN等。

### 4. 资源与算力

*   论文摘要及元数据**未明确说明**使用的GPU型号、数量或训练时长。因此无法总结具体算力要求，但在实际研究中通常需要中等规模GPU（如单块NVIDIA V100或A100）进行训练。

### 5. 实验数量与充分性

*   实验主要集中在TPMS几何的稳态不可压缩流动问题上。可能包含：
    *   不同TPMS结构（如Gyroid、Schwarz D等）的对比实验。
    *   多尺度策略的消融实验（例如去掉某一尺度或使用单尺度）。
    *   与标准PINN、其他弱形式方法的定量比较（相对误差、质量守恒残差等）。
*   充分性判断：实验覆盖了关键复杂几何场景，且定量结果（相对误差降低93%）表明效果显著。但未提及在其他类型复杂几何（如多孔介质、随机孔隙）或瞬态流动上的验证，泛化性有待更多测试。

### 6. 论文的主要结论与发现

*   MUSA-PINN在TPMS复杂几何中的稳态流动模拟上，相比现有最先进基线，相对误差降低高达93%，且质量守恒精度显著提升。
*   多尺度弱形式有效解决了PINN在复杂拓扑中的收敛问题，通量平衡残差强制全局守恒，训练稳定性更好。
*   两阶段训练策略优先保证连续性，有助于动量方程的有效优化。

### 7. 优点：方法或实验设计上的亮点

*   **创新性强**：将弱形式与多尺度分层控制体积结合，巧妙平衡全局耦合与局部精度。
*   **物理保真度高**：通过通量平衡显式强制守恒律，优于传统PINN的隐式损失约束。
*   **适应复杂拓扑**：骨架感知的中尺度体积设计专门针对TPMS等复杂通道几何，提升传输路径的捕捉能力。
*   **实验结论清晰**：指标改进幅度大（93%相对误差降低），具有说服力。

### 8. 不足与局限

*   **实验覆盖不足**：仅测试了TPMS几何和稳态不可压缩流动，未涉及瞬态、可压缩流动或其他复杂几何（如随机多孔介质、裂缝网络）。
*   **对超参数敏感**：多尺度控制体积的选择（尺寸、数量、重叠方式）可能需手动调整，缺乏自适应策略。
*   **计算开销**：多尺度弱形式需要在多个体积上进行积分，可能增加训练时间，但论文未报告对比计算效率。
*   **缺乏理论分析**：未讨论多尺度策略的收敛性证明或误差界，主要为经验验证。
*   **局限性**：当前方法可能对几何骨架提取有一定依赖，在无清晰通道的复杂几何中效果未知。

（完）
