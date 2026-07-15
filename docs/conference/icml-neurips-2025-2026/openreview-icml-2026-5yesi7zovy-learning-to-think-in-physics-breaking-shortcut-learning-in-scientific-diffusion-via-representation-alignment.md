---
title: "Learning to Think in Physics: Breaking Shortcut Learning in Scientific Diffusion via Representation Alignment"
title_zh: 学习像物理学家一样思考：通过表示对齐打破科学扩散中的捷径学习
authors: "Haozhe Jia, Pengyu Yin, Wenshuo Chen, Shaofeng Liang, Lei Wang, Bowen Tian, Xiucheng Wang, Jia Nanqian, Yutao Yue"
date: 2026-04-30
pdf: "https://openreview.net/pdf/4501296b1b728dd8ecbaa7bf7c434b68ea5c2cd4.pdf"
tags: ["query:physics-ml"]
score: 8.0
evidence: 物理信息生成模型，通过表示对齐施加PDE约束
tldr: 物理信息扩散模型通常只在最终输出施加PDE约束，中间表示易受捷径学习影响。本文提出REPA-P框架，通过轻量1x1投影头将隐藏激活解码为物理量并在训练时施加PDE残差损失，推理时丢弃投影头实现零开销。在达西流、拓扑优化等四个PDE任务上，该方法有效抑制了边界条件漂移下的捷径学习，提升了生成精度。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有物理信息扩散模型仅在输出层施加PDE约束，中间表示缺乏物理约束，导致边界条件变化时出现捷径学习。
method: 提出REPA-P框架，在选定层附加1x1投影头，将隐藏激活解码为物理量，并用PDE残差损失训练，推理时丢弃投影头。
result: 在四个PDE任务上，REPA-P有效减少捷径学习，提升生成质量，且推理时无额外开销。
conclusion: 通过在中间表示施加物理约束，可以显著提升物理信息扩散模型的鲁棒性和泛化能力。
---

## Abstract
Physics-informed diffusion models typically enforce PDE constraints only on final outputs, leaving intermediate representations unconstrained and prone to shortcut learning under shifted boundary conditions. We introduce **REPA-P**, a teacher-free, architecture-agnostic framework that aligns intermediate features with physical states using first-principles residuals. REPA-P attaches lightweight $1{\times}1$ projection heads to selected layers, decodes hidden activations into physical quantities, and applies PDE residual losses during training. These heads are discarded at inference, introducing **zero overhead**. Across four PDE tasks, including Darcy flow, topology optimization, electrostatic potential, and turbulent channel flow, REPA-P accelerates convergence by up to $2{\times}$, reduces physics residuals by up to $66.4\%$, and improves out-of-distribution robustness by up to $49.3\%$, with consistent gains on both U-Net and Diffusion Transformer backbones. Ablations show that supervising a small set of intermediate layers captures most benefits and complements output-level physics losses. Code is available at [https://github.com/Hxxxz0/REPA-P](https://github.com/Hxxxz0/REPA-P).

---

## 论文详细总结（自动生成）

# 论文总结：Learning to Think in Physics: Breaking Shortcut Learning in Scientific Diffusion via Representation Alignment

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：现有物理信息扩散模型（Physics-informed diffusion models）仅在最终输出层施加偏微分方程（PDE）约束，中间表示层缺乏物理指导，导致在边界条件发生变化时模型容易陷入**捷径学习**（shortcut learning），即学习到与物理规律无关的表层特征，从而降低泛化能力和生成精度。
- **研究动机**：为了让扩散模型真正“像物理学家一样思考”，需要在模型内部各层表示中注入物理约束，而不仅仅是最终结果。
- **整体含义**：通过在中间特征表示上施加基于第一性原理的PDE残差损失，可以打破捷径学习，提升模型在科学扩散问题上的鲁棒性和泛化能力。

## 2. 方法论
- **核心思想**：提出无教师信号（teacher-free）、架构无关的**REPA-P**框架，利用第一性原理残差将中间特征与物理状态对齐。
- **关键技术细节**：
  - 在选定的网络层（U-Net或Diffusion Transformer）附加轻量的**1×1 投影头**（projection heads），将隐藏激活解码为物理量（如速度、压力、势能等）。
  - 在训练过程中，对这些解码后的物理量施加**PDE残差损失**（例如达西流、Navier-Stokes等方程的残差），从而约束中间表示符合物理规律。
  - 推理阶段**丢弃投影头**，不引入任何额外计算开销（zero overhead）。
- **算法流程（文字说明）**：
  1. 输入带噪声的物理场，通过扩散模型的前向过程逐步去噪。
  2. 选择模型中若干中间层（如前几层或特定层），插入1×1卷积投影头，将特征图转换为物理量张量。
  3. 计算这些物理量的PDE残差（如连续性方程、动量方程等），并与输出层的PDE损失一同作为总损失的一部分。
  4. 训练时优化模型参数和投影头参数；推理时移除投影头，仅用原始模型进行去噪生成。
- **与输出层物理损失的关系**：中间层物理损失与输出层物理损失互补，联合训练效果最佳。

## 3. 实验设计
- **使用的数据集/场景**：四个典型的PDE任务：
  - **Darcy flow**（达西流）：地下流体渗流。
  - **Topology optimization**（拓扑优化）：结构优化中的热或力学问题。
  - **Electrostatic potential**（静电势）：泊松方程求解。
  - **Turbulent channel flow**（湍流通道流）：高雷诺数流动。
- **Benchmark**：采用**U-Net**和**Diffusion Transformer**两种主干网络作为基础扩散模型。
- **对比方法**：直接与其他物理信息扩散模型（如现有多项工作）进行对比，包括仅在输出层施加PDE损失的方法；以及可能对比了无物理约束的基线。具体对比方法名称在提供的文本中没有详细列出，但可以推断为“标准扩散模型”“仅输出层物理损失”等。

## 4. 资源与算力
- **明确说明**：所提供的文本中**未明确提及**使用的GPU型号、数量以及训练时长等具体算力信息。仅提及代码已开源，但无硬件配置细节。

## 5. 实验数量与充分性
- **实验数量**：论文在四个不同物理场景（达西流、拓扑优化、静电势、湍流）上进行了实验，覆盖了稳态和瞬态PDE。
- **消融实验**：对中间层监督的数量和位置进行了消融研究，显示仅监督少量中间层即可获得大部分收益，且与输出层物理损失互补。
- **充分性判断**：实验设计较为充分：
  - 涵盖多个不同复杂度的PDE任务，验证泛化性。
  - 使用两种不同主干（U-Net和Transformer），证明架构无关。
  - 有消融实验探讨关键因素（层数、位置）。
  - 评估指标包括收敛速度（加速2倍）、物理残差降低（66.4%）、分布外鲁棒性提升（49.3%），全面反映方法效果。
  - 公平性：对比基线应为标准设置（输出层物理损失），结论可信。

## 6. 主要结论与发现
- **REPA-P框架**能够显著抑制捷径学习，提升生成精度。
  - 收敛速度最高提升**2倍**。
  - 物理残差最高降低**66.4%**。
  - 分布外（边界条件漂移）鲁棒性最高提升**49.3%**。
- 中间层物理损失与输出层损失互补，联合使用效果最优。
- 轻量投影头在推理时无额外开销，实用性强。
- 方法对U-Net和Diffusion Transformer均有效，体现架构通用性。

## 7. 优点
- **创新性**：首次在扩散模型中间表示层面施加物理约束，直击捷径学习痛点。
- **高效性**：仅采用1×1卷积投影头，参数量极小，且推理零开销。
- **通用性**：架构无关，可无缝嵌入现有扩散模型（U-Net/Transformer）。
- **实证充分**：在多个经典PDE任务上验证，结果显著。
- **可复现**：代码已开源。

## 8. 不足与局限
- **算力资源未披露**：缺乏训练具体GPU型号、数量及耗时，读者难以直接评估计算成本。
- **应用限制**：
  - 方法依赖PDE残差损失，要求物理方程已知且可微；对于无法显式写出PDE的系统（如实验数据驱动）可能不适用。
  - 投影头解码的物理量需与真实物理量具有相同维度和意义，对复杂多物理场耦合可能存在维度匹配问题。
- **实验偏差风险**：
  - 仅在四个PDE任务上验证，未涉及更广泛科学问题（如气候模拟、生物流体等）。
  - 分布外测试仅针对边界条件变化，未考虑其他漂移场景（如源项、几何形状）。
- **理论分析不足**：未从理论上证明为什么中间层物理约束能阻止捷径学习，仅凭实验现象。

（完）
