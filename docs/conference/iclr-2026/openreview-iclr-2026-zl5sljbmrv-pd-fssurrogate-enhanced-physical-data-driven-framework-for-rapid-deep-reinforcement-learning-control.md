---
title: "PD-FS:Surrogate-Enhanced Physical Data-Driven Framework for Rapid Deep Reinforcement Learning Control"
title_zh: PD-FS：代理增强的物理数据驱动框架用于快速深度强化学习控制
authors: "RUIXIN ZHAN, Weiyuan Sun, Dongyue Huang, Shunxiang Cao"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=ZL5sLJBMrV"
tags: ["query:cfd-agent"]
score: 8.0
evidence: 深度强化学习控制鱼类机器人游泳，涉及流体动力学
tldr: 该论文针对流体动力系统中深度强化学习计算成本高昂的问题，提出PD-FS框架。该框架利用代理模型增强物理数据，加速强化学习训练，使鱼类机器人能够高效跟踪目标点并保持能量效率。实验验证了该方法在降低仿真开销同时实现有效控制。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 高保真流体仿真成本过高，限制了DRL在流体动力系统中的应用。
method: 构建代理模型增强物理数据，结合深度强化学习进行控制策略学习。
result: 在鱼类机器人控制任务中，PD-FS显著降低了训练成本并保持了良好跟踪性能。
conclusion: 代理增强的物理数据框架可有效加速流体环境下的强化学习控制。
---

## Abstract
While deep reinforcement learning (DRL) has demonstrated broad potential in se-
quential decision-making, its application to fluid–dynamic systems remains lim-
ited by the prohibitive cost of high-fidelity simulations and the difficulty of cap-
turing multi-scale unsteady behaviors. In this work, we focus specifically on
aquatic locomotion of fish-like robotic, where the control objective is to track spe-
cific target point while maintaining energy efficiency within the constrained time.
The agent observes low-dimensional kinematic states and flow-related signals,
and outputs oscillation frequency commands that drive body undulation. These
sensing–action constraints define a task that requires both accurate flow responses
and fast, iterative learning. Motivated by these domain-specific requirements, we
propose a task-oriented Physical Data-Driven Flow Simulation (PD-FS) frame-
work—a staged pipeline that couples lightweight neural surrogates with physics-
guided refinement in full-order CFD. PD-FS incorporates mode-conditioned sur-
rogate models with cycle-locked and memory-aware updates, enabling sample-
efficient training while faithfully reproducing critical frequency-switching dynam-
ics. Rather than claiming general applicability, we position PD-FS as an engineer-
ing integration tailored for fish swimming control under fluid–structure interac-
tion. Policies refined in the CFD solvers adapt to nonlinear flow responses without
relying on extensive domain randomization. In controlled fish-locomotion bench-
marks, PD-FS achieves nearly 50 times faster training compared with CFD-only
baselines, while reducing energy expenditure by over 20% at comparable success
rates. These results highlight PD-FS as a domain-specific surrogate to CFD work-
flow for efficient and physically consistent control of fish-like robotics.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：深度强化学习（DRL）在流体动力系统（如鱼类机器人游泳控制）中的应用受限于高保真流体仿真（CFD）的极高计算成本，以及多尺度非稳态流场行为难以捕获的问题。
- **具体背景**：针对鱼类机器人控制任务，目标是使机器人跟踪特定目标点，同时维持能量效率。传统的CFD仿真迭代训练DRL策略成本过高，限制了实际应用。
- **整体含义**：本文提出一种任务导向的物理数据驱动流场仿真框架（PD-FS），利用轻量神经代理模型加速训练，同时保证物理一致性，从而在合理开销下实现高效控制。

## 2. 论文提出的方法论

### 核心思想
- 构建一个分阶段流水线，将**轻量神经代理模型**与**全阶CFD物理精炼**相结合，实现样本高效的DRL训练。
- 代理模型用于快速近似流场响应，DRL策略在代理模型中初步训练，再在CFD求解器中细化，适应非线性流固耦合动力学。

### 关键技术细节
- **模式条件代理模型（Mode-conditioned surrogate models）**：代理模型能够根据不同的振荡频率模式（mode）进行条件化预测。
- **循环锁定与记忆感知更新（Cycle-locked and memory-aware updates）**：训练过程中引入周期锁定机制，确保频率切换的动态特性被准确重现；同时使用记忆感知更新策略，提高样本效率。
- **训练流程**：
  - 阶段1：基于低维运动学状态和流场信号，代理模型辅助DRL智能体输出振荡频率指令。
  - 阶段2：策略在CFD求解器中进行微调（refinement），以应对非线性流场响应。
- 该方法无需广泛的域随机化，直接利用物理数据驱动。

## 3. 实验设计

- **实验场景**：受控的鱼类机器人水动力学基准任务（fish-locomotion benchmarks），具体任务为跟踪目标点并保持能效。
- **智能体观测**：低维运动学状态和流相关信号；输出为驱动身体波动的振荡频率。
- **对比方法**：CFD-only基线（即直接在CFD环境中训练DRL）。
- **评估指标**：训练速度（训练加速倍数）、能量消耗、任务成功率。

## 4. 资源与算力

- 论文**未明确说明使用的GPU型号、数量、训练时长**等具体计算资源信息。
- 仅提到与仅使用CFD的基线相比，训练速度提升近50倍，但未说明基线使用的资源细节。
- 结论：算力开销信息缺失，无法量化。

## 5. 实验数量与充分性

- **实验数量**：论文仅报告了在鱼类机器人基准上的一个主要比较实验（PD-FS vs CFD-only），未见多组数据集或场景的额外实验。
- **消融实验**：摘要中未提及消融实验（如去除代理模型、不同更新策略等），元数据中“实验设计”缺失。
- **充分性评估**：实验覆盖范围较窄，仅一个benchmark，缺乏对框架各组件贡献的量化分析，且未在不同流体动力学任务或不同机器人形态上进行验证。因此实验的充分性、客观性、公平性有待加强；可能存在偏差风险。

## 6. 论文的主要结论与发现

- PD-FS框架能够将训练速度相比于仅使用CFD的基线提升约**50倍**。
- 在相同的成功率下，能量消耗降低**超过20%**。
- 该框架作为CFD工作流的任务导向替代方案，可有效加速鱼类机器人控制策略的学习，同时保持物理一致性。

## 7. 优点

- **显著降低计算成本**：通过代理模型大幅减少高保真仿真调用次数，训练加速近50倍。
- **物理一致性保障**：利用CFD对代理模型生成的策略进行精炼，避免纯数据驱动模型的物理偏差。
- **任务导向设计**：专门针对鱼类游泳控制中的频率切换动态和能效目标，工程针对性强。
- **无需广泛域随机化**：简化训练流程，降低调参难度。

## 8. 不足与局限

- **通用性受限**：方法专门设计用于鱼类机器人控制，未验证在更广泛流体动力系统（如飞行器、管道流）中的适用性。
- **实验覆盖不足**：仅在一个基准任务上测试，缺少多任务/多环境验证；无消融实验，难以判断各组件（如cycle-locked updates、memory-aware updates）的独立贡献。
- **资源计算信息缺失**：未报告具体算力（GPU型号、数量、训练时长），不利于其他研究者复现或对比效率。
- **可能依赖特定CFD求解器**：对CFD精炼步骤的具体依赖（如求解器类型、网格分辨率）未讨论，可能影响可迁移性。
- **偏差风险**：单任务、单基线对比可能高估性能；未考虑噪声或泛化到未见过的流场条件。

（完）
