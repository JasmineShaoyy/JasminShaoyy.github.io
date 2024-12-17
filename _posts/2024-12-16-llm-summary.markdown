---
layout: post
title:  "阶段性语言模型工作"
date:   2024-12-16 16:26:10 +0800
categories: jekyll update
---

目前纯语言类生成模型的重点工作可以分为以下几个方面：

### 一、模型层面上的改进：

主流LLM的架构为transformer decoder-only，少数为encoder-decoder架构（如GLM等）。
主流架构下，针对**模型能力、超长上下文、训练推理加速**等实际需求，研究者提出很多在模型层面上的改进，其细分研究方向包括：
  - attention机制的改进：如group attention等
  - position embedding的改进：如RoPE等
  - MoE框架
  - 模型层面加速：如flash attention等

### 二、整体训练、推理框架：

现阶段训练、推理流程大多遵循OpenAI提出的一系列技术范式。
1. 涉及的训练阶段有：pretrain、continue-train、multi-stage train、sft（instruct ft、chat）、RLHF等，每一阶段的重点在于：
  - 数据构造：方法如evol instruct等，目标在于构造多样化、高质量数据
  - 数据配比：需要综合考虑**逻辑性、instruct following能力、人类偏好**等方面，因此需要思考、实验数据混合方式
  - 训练方式：如不同的mask方式（如chat中不学习human部分）、不同loss（DPO）等，影响模型的训练效果和偏好
2. 推理阶段需要关注的问题有：
  - 不同的decoding strategy：不同的next token sample方式，其目的为平衡多样性、准确性等指标
  - 不同的reasoning方式：对于困难问题，需要CoT等方式进行反思（困难问题花费时间应比简单问题时间长）
  - 不同的evaluation方面：人工评测、reward model、GPT4、NLG指标、PPL等，如何多角度、自动化评测是重点

### 三、主流模型的细节：

现有闭源、开源优秀的工作有很多，有很多值得借鉴的点，从不同功能出发也可以分为：数学、代码类工作（更强的推理能力）；助手类工作（更强的instruct following能力）

1. 一些前沿的闭源工作包括：
  - 助手类：GPT系列（openai）、claude系列（anthropic）、gemini系列（google）
  - 数学、代码类：o1
2. 目前开源的前沿工作有：
  - 英文：LLaMA、Mistral
  - 中文：Qwen、Intern、deepseek、GLM
  - 数学、代码：Phi

### 四、下游应用：

除了数学代码、助手以外，为满足不同下游产品的需求，目前热点的工业界和学术界研究方向还有agent、role playing等，具体为：
1. agent：
  - memory：向量化存储，基本方法为search + rerank
  - planning：Agent能将大任务分解为更小的、可管理的子目标，从而有效地处理复杂任务，如CoT、ReAct等
  - action：负责AI agent和物理世界的交互，如联网搜索、使用工具等
2. role playing：多角色、单角色等
3. 一些有趣的商业应用：
  - 集成搜索类：felo、monica
  - 代码工具类：cursor