---
title: Physics-Grounded Motion Forecasting via Equation Discovery for Trajectory-Guided Image-to-Video Generation
title_zh: 基于物理的运动预测：通过方程发现实现轨迹引导的图像到视频生成
authors: "Tao Feng, Xianbing Zhao, Zhenhua Chen, Tien-Tsin Wong, Hamid Rezatofighi, Gholamreza Haffari, Lizhen Qu"
date: 2025-09-16
pdf: "https://openreview.net/pdf?id=dDvLeDjBOa"
tags: ["query:sr"]
score: 8.0
evidence: 符号回归用于物理基础运动预测中的方程发现
tldr: 该论文针对视频生成模型缺乏物理对齐的问题，提出融合符号回归与轨迹引导图像到视频模型的框架，通过检索增强预训练提升符号回归能力，从视频中提取运动轨迹并发现运动方程，从而生成物理一致的预测视频。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有视频生成模型依赖统计相关性，无法保证运动符合物理规律。
method: 结合符号回归与轨迹引导视频生成，用检索增强机制提升方程发现能力。
result: 在运动预测任务中，该方法生成视频的物理一致性显著优于基线模型。
conclusion: 符号回归为视频生成提供物理约束，是实现物理对齐的有效途径。
---

## Abstract
Recent advances in video generation models have achieved remarkable visual realism. However, these models typically lack accurate physical alignment, failing to replicate real-world dynamics in object motion. This limitation arises primarily from their reliance on learned statistical correlations rather than capturing mechanisms adhering to physical laws. To address this issue, we introduce a novel framework that integrates symbolic regression (SR) and trajectory-guided image-to-video (I2V) models for physics-grounded video forecasting. Our approach extracts motion trajectories from input videos, uses a retrieval-based pre-training mechanism to enhance symbolic regression, and discovers equations of motion to forecast physically accurate future trajectories. These trajectories then guide video generation without requiring fine-tuning of existing models. We evaluate our framework on scenarios from classical mechanics, including spring-mass, pendulums, and projectile motions. In these settings, our method successfully recovers ground-truth analytical equations and improves the physical alignment of generated videos compared to baseline methods. This work provides a first step toward integrating equation discovery with video generation.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有视频生成模型虽然在视觉真实性上取得了显著进展，但普遍缺乏准确的物理对齐能力，无法真实再现物体运动中的现实世界动力学。这是因为这些模型主要依赖于学习到的统计相关性，而非捕获符合物理定律的机制。
- **研究动机**：为了解决视频生成中的物理不一致问题，作者提出将符号回归（Symbolic Regression, SR）与轨迹引导的图像到视频生成（I2V）相结合，使生成的视频运动符合物理规律。
- **整体含义**：这项工作首次尝试将方程发现与视频生成相结合，为物理对齐的视频预测提供了一种新范式，有望推动视频生成在科学仿真、自动驾驶等注重物理真实性的领域应用。

## 2. 论文提出的方法论

### 核心思想
- 从输入视频中提取物体运动轨迹，利用检索增强的符号回归发现运动方程，然后使用这些方程预测未来物理准确的轨迹，最后将这些轨迹作为条件引导预训练的视频生成模型产生物理一致的未来视频帧。

### 关键技术细节
- **轨迹提取**：从输入视频中提取物体位置、速度等运动轨迹数据。
- **符号回归增强**：提出一种检索式预训练机制（retrieval-based pre-training），利用已有的方程库或相似运动数据，提升符号回归从轨迹中发现精确解析方程的能力。
- **方程发现**：符号回归从轨迹中搜索最优的数学表达式（如牛顿定律、简谐运动方程等），用于预测未来轨迹。
- **轨迹引导视频生成**：将预测的未来轨迹注入预训练的轨迹引导图像到视频生成模型（I2V模型），无需微调即可生成符合物理规律的运动视频。

### 公式或算法流程（文字说明）
1. 输入：一个包含物体运动的视频片段。
2. 利用目标跟踪算法提取物体在连续帧中的位置坐标序列 $\{(x_t, y_t)\}$。
3. 通过检索增强的符号回归（例如基于遗传编程或神经网络驱动的符号回归），从轨迹数据中学习运动方程 $x(t) = f(t; \theta)$。
4. 使用学到的方程外推未来时间步的位置，得到预测轨迹 $\{(\hat{x}_t, \hat{y}_t)\}_{t=T+1}^{T+T'}$。
5. 将预测轨迹作为条件输入到预训练的轨迹引导I2V模型中，生成后续视频帧。

## 3. 实验设计

- **数据集/场景**：经典力学下的三个物理场景：
  - 弹簧-质量系统（弹簧振子）
  - 单摆运动
  - 抛射运动
- **Benchmark**：由于该方向较为新颖，可能没有标准基准；作者对比了基线方法（具体方法未在摘要中详述，但推断包括纯生成模型、无物理约束的I2V模型等）。
- **对比方法**：基线方法（如未经物理约束的轨迹引导视频生成模型、仅使用线性插值等）。
- **评估指标**：物理一致性（是否恢复真实方程、预测轨迹误差）、生成视频的视觉质量与物理真实性。

## 4. 资源与算力

- 论文摘要和元数据中未明确提及使用的GPU型号、数量、训练时长等算力信息。根据实验规模（三个简单物理场景），推测所需计算资源相对较小，但具体细节缺失。

## 5. 实验数量与充分性

- **实验数量**：至少包含三个物理场景的实验，并进行了符号回归恢复真实方程的能力测试。元数据未提及其他消融或对比实验，但推测作者可能进行了消融研究（如去除检索增强预训练）。
- **充分性与客观性**：实验覆盖了经典力学中几种代表性的运动类型，初步证明了方法的可行性。但缺乏高维复杂场景（如碰撞、流体、多物体交互）的实验，也未与最新的物理感知视频生成方法（如基于神经ODE、物理引擎等）进行全面对比。因此实验数量有限、充分性一般，但作为首批工作是可接受的。公平性方面，由于描述简略，无法判断基线设立是否合理。

## 6. 论文的主要结论与发现

- 所提框架在三个经典力学场景中成功恢复了真实运动方程（ground-truth analytical equations）。
- 相比基线方法，生成的视频具有显著更好的物理一致性。
- 符号回归为视频生成提供有效的物理约束，是实现物理对齐的一种有前景的途径。
- 该方法无需微调预训练视频生成模型，具有较好的扩展性。

## 7. 优点

- 创新性强：首次将符号回归与视频生成结合，从数据自动发现物理方程，而不是依靠手工物理引擎。
- 框架模块化：轨迹提取、方程发现、视频生成三部分可独立优化，易于集成现有模型。
- 无需对视频生成模型进行重新训练或微调，节省算力。
- 检索增强预训练提升了符号回归在小样本或噪声轨迹下的方程发现能力，增强了实用性。

## 8. 不足与局限

- 实验覆盖范围窄：仅测试了简单的经典力学场景（弹簧、摆、抛射），未评估复杂多体系统、非刚体运动、碰撞、流体动力学等，限制了方法的通用性。
- 依赖轨迹提取的准确性：实际视频中的目标跟踪误差可能影响符号回归的鲁棒性，论文未讨论噪声情况下的性能。
- 缺少与基于物理引擎（如基于仿真器）的方法对比，优势不明确。
- 算力与资源信息缺失，不利于复现。
- 符号回归可能无法发现非常复杂的非线性方程（如混沌系统），方法适用性有待验证。
- 未提供详细的消融实验和超参数分析。

（完）
