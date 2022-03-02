---
title: 'RealFormer: Transformer Likes Residual Attention'
date: 2022-02-20 13:46:06
tags:
- NLP
- Transformer
category:
- Paper
---

ACL Findings 2021，在Transformer中引入残差Attention。

<img src="RealFormer-Transformer-Likes-Residual-Attention/image-20220223125711993.png" alt="image-20220223125711993" style="zoom: 50%;" />

<!--more-->

## Overview

<img src="RealFormer-Transformer-Likes-Residual-Attention/image-20220223125622056.png" alt="image-20220223125622056" style="zoom:33%;" />

- paper: <https://aclanthology.org/2021.findings-acl.81.pdf>
- code: <https://paperswithcode.com/paper/informer-transformer-likes-informed-attention>

## Background

Transformer按Layer Normalization的位置可以分为Post-LN和Pre-LN，BERT、XLNET、RoBERTa和ALBERT等都属于前者，GPT-2和Megatron属于后者。本文兼顾二者，引入残差注意力模块提升了Transformer的性能。

## Method

原始注意力
$$
\text{Attention}(Q',K',V') = \text{Softmax}(\frac{Q'K'^T}{\sqrt{d_k}})V'
$$
残差注意力
$$
\text{ResidualAttention}(Q',K',V', Prev') = \text{Softmax}(\frac{Q'K'^T}{\sqrt{d_k}}+Prev')V'
$$

## Experiment

在预训练和下游任务上都取得了更好的结果。

<img src="RealFormer-Transformer-Likes-Residual-Attention/image-20220223130401730.png" alt="image-20220223130401730" style="zoom:33%;" />

<img src="RealFormer-Transformer-Likes-Residual-Attention/image-20220223130504167.png" alt="image-20220223130504167" style="zoom:33%;" />

<img src="RealFormer-Transformer-Likes-Residual-Attention/image-20220223130518912.png" alt="image-20220223130518912" style="zoom:33%;" />
