---
title: Plug-and-Play Benchmarking of Reinforcement Learning Algorithms for Large-Scale Flow Control
title_zh: 面向大规模流控的强化学习算法即插即用基准
authors: "Jannis Becktepe, Aleksandra Franz, Nils Thuerey, Sebastian Peitz"
date: 2026-04-30
pdf: "https://openreview.net/pdf/b835d52596d1a9fab16f8101223c976b71372a31.pdf"
tags: ["query:cfd-agent"]
score: 9.0
evidence: 基于CFD求解器的主动流控强化学习基准
tldr: 现有主动流控RL研究依赖异构设定，难以比较。本文提出FluidGym，首个完全可微的RL基准套件，基于GPU加速的PICT求解器，无需外部CFD软件。支持3D和多智能体，为智能体在CFD中的应用提供标准化平台。
source: ICML-2026-Accepted
selection_source: conference_retrieval
motivation: 现有主动流控RL研究因异构设定难以评估进展，亟需统一基准。
method: 基于PyTorch和GPU加速PICT求解器构建FluidGym，支持完全可微和独立运行。
result: FluidGym提供了标准化评估，加速RL算法在流控中的验证。
conclusion: 为智能体在CFD中的应用提供了关键基础设施。
---

## Abstract
Reinforcement learning (RL) has shown promising results in active flow control (AFC), yet progress in the field remains difficult to assess as existing studies rely on heterogeneous observation and actuation schemes, numerical setups, and evaluation protocols. Current AFC benchmarks attempt to address these issues but heavily rely on external computational fluid dynamics (CFD) solvers, are not fully differentiable, and provide limited 3D and multi-agent support. To overcome these limitations, we introduce FluidGym, the first standalone, fully differentiable benchmark suite for RL in AFC. Built entirely in PyTorch on top of the GPU-accelerated PICT solver, FluidGym runs in a single Python stack, requires no external CFD software, and provides standardized evaluation protocols. We present baseline results with PPO, SAC, DPC, and TD-MPC, and release all environments, datasets, and trained models as public resources. FluidGym enables systematic comparison of control methods, establishes a scalable foundation for future research in learning-based flow control, and is available at github.com/safe-autonomous-systems/fluidgym.

---

## 论文详细总结（自动生成）

# 面向大规模流控的强化学习算法即插即用基准——论文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **背景**：强化学习（RL）在主动流控（Active Flow Control, AFC）领域展现潜力，但现有研究依赖异构的观测/执行方案、数值设置和评估协议，难以横向比较和评估进展。
- **问题**：已有AFC基准对外部CFD求解器依赖度高，不可微，对3D和多智能体场景支持有限，缺乏统一、可复现的评估平台。
- **整体含义**：亟需一个标准化、可微分、独立运行且支持大规模流控（包括3D和多智能体）的RL基准，以加速学习型流控方法的研究和对比。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：构建首个完全基于PyTorch、独立运行、完全可微的RL基准套件FluidGym，利用GPU加速的PICT求解器替代外部CFD软件，实现单Python栈运行。
- **关键技术细节**：
  - 基于PICT求解器（GPU加速的不可压流求解器）构建全部环境。
  - 支持完全可微：允许梯度从奖励函数反向传播到控制策略，利于基于梯度的RL方法（如DPC）。
  - 标准化评估协议：统一观测、动作空间、奖励函数和评价指标。
  - 兼容3D和多智能体场景，扩展性优于现有基准。
- **算法流程**（文字说明）：用户定义流场几何和边界条件 → FluidGym内部调用PICT求解器计算流场演化 → 在每个时间步提取观测（如压力、速度场） → RL智能体输出动作（如壁面吹吸动作） → 求解器更新流场 → 计算奖励（如阻力系数、压力损失） → 更新策略。整个过程完全在PyTorch图中，支持自动微分。

## 3. 实验设计：数据集/场景、基准、对比方法

- **数据集/场景**：论文未详细列出具体场景，但基于“大规模流控”和“3D/多智能体”说明，可能包含典型主动流控问题（如圆柱绕流减阻、翼型分离控制、腔体流动等）。实际场景需查看完整论文。
- **基准（benchmark）**：FluidGym自身作为标准化平台，提供不同难度的环境集合（包括2D/3D、单/多智能体），并定义了统一评估协议。
- **对比方法**：在FluidGym上运行了四种RL基线算法：
  - PPO（Proximal Policy Optimization）
  - SAC（Soft Actor-Critic）
  - DPC（Differentiable Predictive Control，可微预测控制）
  - TD-MPC（Temporal Difference Model Predictive Control）
- **实验充分性估计**：仅给出基线结果，未见详细消融或跨场景对比。由于是基准平台论文，主要展示平台可用性和基线性能，实验数量可能有限（每个场景若干seed运行）。

## 4. 资源与算力

- 文中**未明确说明**使用的GPU型号、数量或训练时长，仅提及“GPU-accelerated PICT solver”以及“运行在单Python栈”。因此无法量化算力开销。
- 可能后续版本会补充算力消耗（如每个环境训练耗时、GPU内存占用等），但当前摘要未提及。

## 5. 实验数量与充分性

- **实验数量**：摘要提到“present baseline results with PPO, SAC, DPC, and TD-MPC”，推测至少在这些算法上各运行了若干独立实验（如不同随机种子），但未给出具体次数。未见多场景、多超参数或消融实验的详细描述。
- **充分性评估**：作为基准平台论文，实验主要验证平台功能正常、可复现基线。但缺乏：
  - 跨算法公平比较时的超参数调优细节；
  - 与已有基准（如基于OpenFOAM）的对比；
  - 对不同流场复杂度的系统评估。
  - 因此充分性**中等**，更深入的比较需后续研究补充。

## 6. 主要结论与发现

- FluidGym成功提供了首个完全可微、独立运行的RL for AFC基准套件，消除了对外部CFD的依赖，并支持3D和多智能体。
- 基线结果表明，PPO、SAC、DPC、TD-MPC等常见RL算法可在该平台上直接运行并产生有意义的控制策略，验证了平台的可用性。
- 标准化评估协议使得不同算法之间的性能比较变得公平、可复现，有助于加速该领域进步。

## 7. 优点

- **完全可微**：利用PyTorch自动微分，支持基于梯度的控制方法（如DPC），这是现有基准不具备的。
- **独立运行**：单Python栈，无需安装外部CFD软件，降低使用门槛和可复现性。
- **GPU加速**：PICT求解器在GPU上运行，可高效处理大规模3D流动。
- **多智能体支持**：扩展了RL在流控中的应用场景，例如多个执行器协同控制。
- **开源**：公开所有环境、数据集和训练模型，促进社区贡献与比较。

## 8. 不足与局限

- **实验覆盖不足**：仅提供有限数目的基线结果，未在多种流动场景（如湍流、高雷诺数、非定常流）上验证通用性。
- **偏差风险**：基准环境可能偏向可微控制方法（DPC），对基于模型的RL方法可能更有利；未与基于传统CFD的基准（如DeepFlowCylinder）进行背靠背比较，评估迁移效果。
- **应用限制**：
  - 当前求解器PICT的能力有限（如可能不适用于可压流、复杂几何或多相流），限制了AFC问题类型。
  - 未评估真实流控系统的延迟、噪音等工程因素，仅停留在仿真层面。
  - 大规模3D场景下的计算资源需求可能仍然较高，实际训练开销未给出，不利于研究者复制。
- **文档与扩展性**：摘要中未提及环境配置的灵活性（如自定义几何和边界条件），可能对高级用户不够友好。

（完）
