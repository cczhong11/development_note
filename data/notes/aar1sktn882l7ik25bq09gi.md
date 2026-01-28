


## 🧠 1. Pretrain 量化（Pretraining Quantization）

原始训练的时候，模型参数都是用 **float32** 表示的，虽然很精确，但超级占内存🥲

为了让训练更快、更省显存，现在有些研究开始探索：

* **Quantization-aware training (QAT)**：在训练过程中模拟低精度（比如 fp8、int8）的效果，提前适应。
* **PTQ (Post-training quantization)**：训练完再量化，但预训练阶段很少这么做，因为会损伤模型性能。

不过 LLaMA 的官方预训练模型一般还是 float16/float32，真正的量化都是**训练完之后做的**！

---

## 🐏 2. LoRA 量化（Low-Rank Adaptation + Quantization）

LoRA 是一种「低秩微调」的方法，它冻结原始模型，只训练小小的 adapter 📦

👉 所以量化分两块：

1. **原始模型参数**可以量化成 int8 / int4（比如用 GPTQ、AWQ、Exllama2）
2. **LoRA adapter** 通常保留为 float16（因为很小～）

这种方式你可以边量化边微调，或者：

* 先量化 float 模型
* 再用 LoRA 做少量微调
* 推理时加载量化模型 + float16 的 adapter 💡

---

## 💡 3. FFN-only 量化（比如 Outlier-Aware Quantization）

在 LLaMA 中，**Feed-Forward Network（FFN）层的激活值分布特别“爆炸”💥，有很多 outlier（异常值）**

所以大家发现可以只量化这些部分：

* 使用 **group-wise quantization**（分组量化）来处理 outlier
* 或者只量化 Attention，不量化 FFN

👉 比如 [AWQ](https://arxiv.org/abs/2306.00978) 和 [SmoothQuant](https://arxiv.org/abs/2211.10438) 都对 FFN 做特殊处理！

目的就是：**保留重要的精度，同时压缩模型大小 ✨**

---

## 🛰️ 4. 推理阶段量化（Serving-time Quantization）

到了真正上线部署（serving）的时候，量化就超重要了！这里一般用：

### ✨ 常见 serving 量化方法：

* **GPTQ**：离线量化，很快但有些损精度
* **AWQ**：考虑 outlier，适合 FFN 层，性能和精度更平衡
* **Exllama / Exllama2**：专为 LLaMA 优化的 int4 后端（巨快）
* **AutoGPTQ**：支持 HuggingFace，易用！
* **MLC / vLLM / TensorRT-LLM**：部署神器，支持不同硬件后端！

### 🚀 Serving 的目标：

* 内存占用低（int4 一般只有原来 1/8）
* 吞吐量高（更多 token/s）
* 精度损失尽可能小

---

## 🍰 总结一张图（逻辑流）：

```
[Pretraining] (float32/fp16)
      ↓
[Post-training Quantization] → GPTQ, AWQ
      ↓
[Optional: LoRA Fine-tuning]
      ↓
[FFN-aware Optimizations] → SmoothQuant, AWQ
      ↓
[Serving] → Exllama2, AutoGPTQ, vLLM, MLC
```

---

## 💬 小贴士 Time ～

* 想用 int4 LLaMA 可以直接试试 Huggingface 上的 [GGUF 模型](https://huggingface.co/TheBloke)，配合 `llama.cpp` 或 `Exllama` 后端用超爽～
* 不同量化方案适合不同的显卡或硬件平台哦，要看是 CPU/GPU 还是手机 📱
* 推理的时候 batch size 和 quant scheme 选得好，可以快 3\~5 倍！
