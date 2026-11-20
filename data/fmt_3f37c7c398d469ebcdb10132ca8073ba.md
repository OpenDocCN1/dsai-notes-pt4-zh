
# Triton Conference 2024 - talk notes

> 来源：[https://learning.feishu.cn/docx/TfJidtSTCoIZbDxY4UncIoWjnWc](https://learning.feishu.cn/docx/TfJidtSTCoIZbDxY4UncIoWjnWc)

https://www.youtube.com/watch?v=nglpa_6cYYI&list=PLc_vA1r0qoiTjlrINKUuFrI8Ptoopm8Vz

Triton Conference 2024

16 videos

最后有两三个 30min 的 tutorial

今年的质量，感觉比较水，没见到太多的干货和新的 `insight`。

Philippe Tillet 和 AMD 的 talk，还不错。

出现比较多的关键词是 `software pipelining`, `inst scheduling`。

![](img/ccb44bc0cb6c2a0f70bed328b01c8116.png)

## Keynote: Philippe Tillet

https://www.youtube.com/watch?v=o3DrHb-mVLM&list=PLc_vA1r0qoiTjlrINKUuFrI8Ptoopm8Vz&index=3

Keynote: Philippe Tillet

Sep 27, 2024

![](img/09e940d78d75e4779a72b359a533f764.png)

[phil-keynote.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/AiwWbCng8oHeEwxsOUMcfoeDn6f)

Phil talks to the state-of-the-Triton-Union and his vision for the coming year

### 回顾

![](img/d6e9e67b74c9a28afb5f3c9517004b44.png)

`TritonGPU-IR` 依旧复用性较差，是后续改进的重点。

![](img/f31bdee43b390ded920b1e02b17f2754.png)

`Matmul`，速度已经超过了 `cuBLAS` 12.4

![](img/d846c933b375fa2fe8d10bf0734d53ff.png)

### 未来

Future 的计划上，跟我们的预判比较接近：

1. 以 `LinearLayout` 为中心的 `layout` + 结构化的 `Memory access` 标准化。解决 `TritonGPU IR` 的复用性问题。

2. 计算部分，基本不是现代 `compiler` 关注的焦点和难点了。

3. 没提到 `auto search` / `auto schedule` 方向的目标。

4. 性能上，提到了 3+1 个点：`register spill`，`software pipelining`，`extra smem round-trips`，`warp specialization`

![](img/571933a4b8cf58ff2f304575d628c2b1.png)

![](img/0aa8359c939f541e84e18f845813785b.png)

![](img/126dc52e6d0ed3eb976e60f5a223c548.png)

代码质量上，`software pipeliner` 的重构是重点

![](img/018d9f00ad88e292e8c37237b8a172c5.png)

## Enable Triton on Intel GPUs

https://www.youtube.com/watch?v=Z20O3BJcnE8&list=PLc_vA1r0qoiTjlrINKUuFrI8Ptoopm8Vz&index=4

Ettore Tiotto

Sep 27, 2024

![](img/24578ded37c529c49d60f02db9905c81.png)

Intel backend 做了 `redesign`。

用了 `MLIR` `llvm` dialect -> `LLVMIR` -> `SPIRV`

`dot op` 的 `layout` 是 `DPAS layout`

![](img/94001fca991e1acd98d46e3e4d6d1620.png)

![](img/b67fdcfb2d5fe24ceb177eed1320bc14.png)

![](img/c76d46e4675305490ea3dd71a1eb4372.png)

![](img/f59d4ce07ca9b74ccb8828610a649249.png)

![](img/891d65dabff0fedfaedf76d012f5fd57.png)

![](img/080acc3527ae5c57f0192db0a7275051.png)

## Triton on AMD GPUs

https://www.youtube.com/watch?v=Lbm08twNTAQ&list=PLc_vA1r0qoiTjlrINKUuFrI8Ptoopm8Vz&index=5

Sep 27, 2024

Lei Zhang and Lixun Zhang

shows off some very clever optimization techniques around chiplets and also instruction scheduling around `L1` and `global memory`.

### Features and status

![](img/e5ee3c0e591865f4dbd42e8d875fa9fc.png)

fast `exp` math for attention

![](img/53a684d6e839d5c29e6565e82477c2ca.png)

![](img/d2e47988bcf19a03a56d29f1f73b5bd0.png)

![](img/d088bb6adfed1d842332d2d9e7796ea4.png)

### Selected Optimizations

![](img/507ed671f4e02057424e0b9340fdf185.png)

#### chiplet-friendly `PID` mapping

![](img/31bb901b039b9eb50740cf3e4b94bdaa.png)

![](img/4edad1f0cc71f2773f9dd574e40dd48c.png)

![](img/f5563611777a1eb1929577f56e10d75f.png)

#### `inst` scheduling

![](img/adc75810b7b61f666faa27ec341930d5.png)

![](img/a6e59add22d5f97cea027a64c407d889.png)

![](img/a90464189ef966eee867b399bd4f00dd.png)

#### `Stream-K`

![](img/a0a36346544bab55a0055d98f59aca35.png)

![](img/cbaddbae6e9f809a458697454401e985.png)

## Triton CPU

https://www.youtube.com/watch?v=obGM7nujV00&list=PLc_vA1r0qoiTjlrINKUuFrI8Ptoopm8Vz&index=6

Sep 27, 2024

Minjang Kim (Meta) and Ilya Enkovich (Intel)

repo: https://github.com/triton-lang/triton-cpu

### CPU Exec Model

`pid` maps to CPU thread

`block` size 是 128 elements。一共 512 elements，分 4 个 block。

![](img/f214305031e4b8e25221ca45b7471a07.png)

![](img/783849e20028e96ac9d4a794f096464c.png)

![](img/73f814b2c54bca71fa3002d4a2d8652e.png)

![](img/3022a7fe0c3e3d852b4c33caebaa22bd.png)

![](img/2a5a22d2b9fb5faa7cce2bd9c8e498ca.png)

### Perf

![](img/83ed293166ed65b8fa060c043f0fe1fa.png)

## Triton on Blackwell

Accelerating the Future: Triton on Blackwell Architecture

https://www.youtube.com/watch?v=RW2-HtWaOS0&list=PLc_vA1r0qoiTjlrINKUuFrI8Ptoopm8Vz&index=11

Pradeep Ramani (NVIDIA) and Thomas Raoux (OpenAI)

Sep 27, 2024

![](img/12efd450ffe3a4d2c115e068545f6e02.png)

![](img/288dfdb49b8b19e8e9e23d5915e0bbff.png)

![](img/08a0cc1b64144c6af33e6ad2b4bb1aaa.png)

![](img/d322bec24257c71f1f255e360176478e.png)
