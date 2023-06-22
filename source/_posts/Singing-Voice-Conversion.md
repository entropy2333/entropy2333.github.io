---
title: Singing Voice Conversion
category:
  - 备忘
date: 2023-06-22 21:24:03
tags:
  - AIGC
---

基于so-vits-svc的唱歌声音转换。

<!--more-->

# Singing Voice Conversion

> 本文由ChatGPT润色生成

基于so-vits-svc的唱歌声音转换。


## 准备工作

- 克隆代码并安装依赖

```bash
git clone https://github.com/svc-develop-team/so-vits-svc.git

conda create -n aisinger python=3.8
cd so-vits-svc
pip install -r requirements.txt
```

- 下载预训练模型 [checkpoint_best_legacy_500.pt](https://ibm.box.com/s/z1wgl1stco8ffooyatzdwsqn2psd9lrr)，并将其放置在 `pretrain` 目录下
- 下载自定义模型，包括
  - `G_xxx.pth`
  - `config.json`
  - `kmeans_xxx.pt`（可选）
- 使用 [spleeter](https://github.com/deezer/spleeter/wiki/2.-Getting-started#using-docker-image) 来分离人声和伴奏

选项 1（推荐）：谷歌 Colab

```bash
spleeter separate -o output/ xxx.mp3
```

选项 2（不适用于 M1 Mac）：我们也可以在本地使用 docker

```bash
docker run -v $(pwd)/output:/output deezer/spleeter:3.8 separate -o /output xxx.mp3
```

它将生成两个文件：`vocals.wav` 和 `accompaniment.wav`，`vocals.wav` 是用于转换的。

## 入门

我们可以打开 webui 来转换 wav

```bash
python -u webUI.py
```

或者直接使用 cli 进行转换

```bash
python inference_main.py \
    -m "logs/44k/G_27200.pth" \
    -c "configs/config.json" \
    -n "/path/to/vocals.wav" \
    -t 0 \
    -s "sun" \
    -cm "logs/44k/kmeans_10000.pt"
```

生成的 wav 存放在 `results` 目录下，然后我们使用 ffmpeg 合并两个 wav

```bash
ffmpeg -i ../../results/sun_1687426444.wav -i accompaniment.wav -filter_complex amix=inputs=2:duration=longest output_1687426444.wav
```

使用 ffmpeg 将 wav 转换为 mp3

```bash
ffmpeg -i output_1687426444.wav -vn -ar 44100 -ac 2 -b:a 192k output_1687426444.mp3
```

我们还可以将两个步骤合并为一个步骤

```bash
ffmpeg -i ../../results/sun_1687426444.wav -i accompaniment.wav -filter_complex amix=inputs=2:duration=longest -vn -ar 44100 -ac 2 -b:a 192k output_1687426444.mp3
```

## 问题

`RuntimeError: Given groups=1, weight of size [xxx, 256, xxx], expected input[xxx, 768, xxx] to have 256 channels, but got 768 channels instead`

- 这是由于编码器配置和模型权重之间的不匹配。我们可以修改 `config.json` 中的 `speech_encoder`。
```diff
{
    "model"
+ "speech_encoder": "vec256l9"
}
```

## 参考资料

- https://zhuanlan.zhihu.com/p/630115251
- https://www.bilibili.com/read/cv22206231/
